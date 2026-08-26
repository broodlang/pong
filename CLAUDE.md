# pong — guidance for Claude

Brood Pong — a full-screen, over-the-top game of Pong. The whole game lives in
one module, `src/pong.blsp` (physics, rendering, menus, and the self-clocked
frame loop); `tests/pong_test.blsp` covers it. There is no other source module.

## Running

- `nest test`   — run the test suite (each test runs in its own green process).
- `nest run`    — invoke the entry point. This project declares `:main pong`
  in `project.blsp`, so it runs `pong/main` (without a `:main`, `nest` would
  default to `main/main`). Entries are bare symbols — the manifest is data:
  `:main app` runs `app/main`, `:main (app start)` runs `app/start`; never
  quote them.
  Each module is a namespace (ADR-065): a file's `defmodule name` makes its
  `def`/`defn` define `name/foo`; a bare reference resolves in the current
  namespace, then through `(:use …)` imports, then root/prelude. So the same
  short name in two modules is fine (`a/parse` vs `b/parse`) — only earmuffed
  `*foo*` vars are ambient/bare and must stay unique.
- `nest run --for 2s` — run a loop / full-screen TUI for a bounded time, then
  exit cleanly (`2s`, `500ms`, or a bare integer of ms). The way to exercise
  a never-returning program end-to-end or in CI.
- `nest format` — format Brood source.

## Writing Brood

`docs/brood-for-claude.md` is the language reference geared for AI assistants
— syntax, idioms, and the patterns that aren't shared with other Lisps. Read
it before generating Brood code. The `.claude/skills/writing-brood` skill
carries the short version and auto-loads when Claude Code edits `.blsp` files.

Brood ships randomness (`rand/int`/`rand/float`/`shuffle`/`sample` — pure and
seedable, thread the seed), bitwise ops (`bit-and`/`bit-or`/`bit-xor`/...),
and discovery (`apropos` / `doc-search`, plus the `all-globals` MCP tool) —
use those to find what exists instead of guessing names. Subsystem builtins
are namespaced: `os/now-ns`, `dev/gc-collect`, `gui/…`, `term/…`, `text/…`,
`rand/…`. A bare, unnamespaced name that `nest test` reports as an "unbound
symbol" warning is almost always one of these that needs its prefix.

## MCP integration

`.mcp.json` points Claude Code at this project's `nest mcp` server, so `cd pong && claude`
auto-attaches an agent that can `eval`, `load`, `lookup`, `macroexpand`, `format`,
and discover the image with `apropos` / `all-globals` / `doc-search`, against the
live image (ADR-036, `docs/mcp.md` upstream).
