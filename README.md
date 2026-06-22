# Brood Pong

A loud, over-the-top game of Pong written in [Brood](https://github.com/broodlang/brood).
Pick a mode — **Player vs Player**, **Player vs AI**, or **AI vs AI** — dial in the
difficulty and points-to-win, then play. Rallies ramp up power-ups and guns, the
crowd erupts, streakers invade the court, the wind turns to a gale, and the winner
gets a fireworks finale. It runs full-screen in its own window.

## Prerequisites

You need the **Brood toolchain** — the `brood` runtime and the `nest` project tool.
Install it from the Brood repository and follow its instructions:

- https://github.com/broodlang/brood

Once installed, `brood` (the runtime/REPL) and `nest` (build/run/test) should be on
your `PATH`. Verify with:

```sh
nest --version
```

## Running

From the project root:

```sh
nest run
```

This launches `pong/main` — it opens a maximised window on the setup menu, then
plays the match you configure. (`nest run` uses the `:main` entry declared in
`project.blsp`, which is `pong`.)

To run for a bounded time and then exit cleanly (handy for a quick smoke test or
CI, since the game otherwise runs until you quit):

```sh
nest run --for 2s
```

## Controls

**Setup menu**

| Key            | Action                              |
| -------------- | ----------------------------------- |
| ↑ / ↓          | Move the highlight                  |
| Enter / Space  | Confirm / advance a step            |
| q / Esc        | Quit                                |

You choose a **mode** (PvP / PvAI / AI-vs-AI), an **AI difficulty** and **game
difficulty** (1–10 dials for pace and power-up chaos), and **points to win**
(first to 5 / 10 / 30).

**In a match**

| Side  | Move          | Shoot         |
| ----- | ------------- | ------------- |
| Left  | `W` / `S`     | `Space`       |
| Right | `↑` / `↓`     | `Enter` / `/` |

- **Esc** or **q** opens the pause menu (Resume / Main Menu / Quit) — neither quits
  outright.
- Closing the window or **Ctrl-C** tears down.

A side controlled by the AI ignores its keys, so in Player-vs-AI only your side
responds.

## Development

```sh
nest test     # run the test suite (each test runs in its own process)
nest format   # format the Brood source
```

The game lives in [`src/pong.blsp`](src/pong.blsp); tests are under
[`tests/`](tests/). For the Brood language itself — syntax, idioms, and the
patterns that differ from other Lisps — see
[`docs/brood-for-claude.md`](docs/brood-for-claude.md).

## Project layout

```
project.blsp          project manifest (name, version, :main entry)
src/pong.blsp         the game — physics, rendering, menus, the self-clocked loop
src/main.blsp         the scaffold's logger/greeting entry (unused by the game)
tests/                the test suite
docs/                 Brood language reference
```
