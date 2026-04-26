# Simple Timer for Even G2

A hands-free countdown / count-up timer for [Even Realities G2](https://www.evenrealities.com/) smart glasses, designed to be operated entirely from the R1 ring. No phone tap is needed once you're set up.

> **Status:** v1.0.0 — preparing for Even Hub submission.

## Features

- **Down (countdown) and Up (count-up) modes**
- **R1-only operation** — set duration, start, pause / resume, and return to the menu without ever touching your phone
- **Persistent settings** — the last-used duration and mode are remembered across launches
- **Visual progress** — segmented progress bar with quarter markers, plus an `MM:SS / total` readout
- **Completion alert** — the bar flashes in a triple-burst rhythm (×3) when the timer finishes, so you don't miss the moment
- **Lightweight text rendering** — single-frame on-glass updates, no image upload

## Preview (on-glass)

Menu (default — cursor on the first row):

```
★ Simple Timer ★
▔▔▔▔▔▔▔▔▔▔▔▔
▷ Count min: [ 5 min ]
▒ Count sec: [ 0 sec ]
▒ D/U: [ Down ]
▒ Start
```

Editing a value (cursor switched to "active" mode):

```
★ Simple Timer ★
▔▔▔▔▔▔▔▔▔▔▔▔
▶ Count min: [ 10 min ]
▒ Count sec: [ 0 sec ]
▒ D/U: [ Down ]
▒ Start
```

Running (Down mode, ~30 % elapsed):

```
★ Simple Timer ★
▔▔▔▔▔▔▔▔▔▔▔▔

▒▒▒▒▒▒▒──┼─────┼─────

03:30 / 05:00

▷ [Pause]
▒ Return to Menu
```

Finished (the full bar flashes; cursor auto-jumps to Return):

```
★ Simple Timer ★
▔▔▔▔▔▔▔▔▔▔▔▔

▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒

00:00 / 05:00

▒ [Done]
▷ Return to Menu
```

## Controls

| Input | Action |
|---|---|
| Swipe up / down | Move cursor / cycle the active value |
| Tap | Activate / confirm (`▷` → `▶` → commit) |
| Double-tap | Exit confirmation dialog (Even Hub page-lifecycle convention) |

### Menu screen

1. **Swipe** to move the `▷` cursor through `Count min` → `Count sec` → `D/U` → `Start`.
2. **Tap** to enter "edit" mode (cursor changes to `▶`), then **swipe** to cycle the value.
3. **Tap** again to commit (back to `▷`).
4. With the cursor on `Start`, **tap** to begin — you'll see a 500 ms `▶` flash before the running screen appears.

If both `Count min` and `Count sec` are 0, the `Start` row is hidden — pick a non-zero duration first.

Available values:

| Field | Choices |
|---|---|
| Count min | 0, 1, 3, 5, 10, 15, 20, 30, 45, 60 |
| Count sec | 0, 10, 20, 30, 40, 50 |
| D/U | Down, Up |

### Running screen

1. **Swipe** to toggle the `▷` cursor between `[Pause]` and `Return to Menu`.
2. **Tap** on `▷ [Pause]` to pause (label flips to `[Start]`); **tap** again to resume.
3. **Tap** on `▷ Return to Menu` to stop the timer and go back. Your last-chosen duration and mode are remembered.
4. When the timer finishes, the cursor auto-jumps to `Return to Menu` so a single tap takes you home.

Each running-screen tap shows the same 500 ms `▶` confirmation flash as the menu's Start.

## Try it

- **Browser dev:** `npm run dev`, then open http://localhost:5176 — the in-browser UI mirrors what the glasses show.
- **On-glass:** sideload the bundled `.ehpk` through the Even Hub dev portal (or wait for Hub approval).

## Development

### Requirements

- Node.js 20+
- `@evenrealities/evenhub-cli` 0.1.12 or later (installed as a devDependency)

### Build & run

```bash
npm install
npm run dev        # browser dev at :5176
npm run typecheck  # tsc --noEmit
npm run build      # production build into dist/
npm run pack       # build + package into simple-timer.ehpk
```

## Tech stack

- TypeScript + Vite (flat asset output, required by `evenhub-cli`)
- React + [even-toolkit](https://www.npmjs.com/package/even-toolkit) for the smartphone UI
- `@evenrealities/even_hub_sdk`
- Bridge + `localStorage` hybrid persistence (`bridge.setLocalStorage` / `bridge.getLocalStorage`)

## Author

Built by **TakeMotions**.
