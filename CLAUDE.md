# THE FLOOR — Picture Quiz Game

## Overview

A single-page HTML quiz game inspired by "The Floor". Two players compete by identifying pictures from categories. The quizmaster controls the game via on-screen buttons. No server required — runs directly from the filesystem using the browser's File System Access API (Chrome/Edge only).

## File Structure

```
game-folder/
├── the-floor.html       ← The game (open this in Chrome/Edge)
├── floor.csv            ← Question data (case-insensitive filename)
├── CLAUDE.md            ← This file
└── <category>/          ← One folder per category
    ├── image1.jpg
    ├── image2.png
    └── ...
```

## CSV Format

`floor.csv` must have these two columns (other columns are ignored):

| Column   | Description                                      | Example                   |
|----------|--------------------------------------------------|---------------------------|
| `@image` | Relative path: `category folder/image filename` | `animals/Giant_Panda.jpg` |
| `answer` | The correct answer (1–2 words typical)           | `Panda`                   |

Supported image formats: `.jpg`, `.jpeg`, `.png`, `.gif`, `.webp`

Example CSV:
```
@image,answer,Player1,Player2
animals/Giant_Panda.jpg,Panda,Tom,Pat
animals/lion.jpg,Lion,Tom,Pat
```

## Game Flow

1. Open `the-floor.html` in Chrome or Edge — a player screen window opens automatically
2. Allow popups if the browser blocks the second window
3. Click **📂 Load floor.csv** on the QM window and select the game folder (remembered between sessions)
4. Select a category from the dropdown
5. **GET READY** screen shows on both windows — click **▶ START** (or Space) to begin
6. Player 1's clock starts running; the image shows on the player screen
7. Use **✓ CORRECT** (Space) or **PASS** (P) to control play
8. Round ends when all pictures are shown or a player's clock hits zero

## Controls

| Button / Key | Behaviour |
|--------|-----------|
| **✓ CORRECT** / Space | Stops clock for 1s, shows answer in white, awards point, switches player, advances picture |
| **PASS** / P | Clock keeps running for 3s while answer is shown in red, advances picture — **same player continues** |
| **RESET ROUND** | Resets clocks and scores, returns to GET READY screen |
| **NEW ROUND** (winner screen) | Returns to GET READY for same category |
| **📺 Player Screen** | Reopens the player window if it was closed |

## Dual-Screen Architecture

The game uses `BroadcastChannel('thefloor')` to sync state between the QM and player windows.

- **QM window** (`?role=qm`, default): runs all game logic; broadcasts full state after every tick (100ms) and every state change; reads images from disk and sends them as `ArrayBuffer` messages so the player window never needs folder access
- **Player window** (`?role=player`): no game logic; receives `state`, `image`, and `img-error` messages and renders them via `applyState()`; controls and QM answer bar are hidden via `body.player` CSS class

On load, the QM window opens the player window via `window.open()`. The player window sends a `hello` message on load; the QM responds with the current state and last image so the player window catches up immediately.

## Constants (top of `<script>`)

```javascript
const ROUND_TIME   = 45;   // seconds per player per round
const PASS_PENALTY = 3;    // duration (s) of pass answer display
```

## Key Functions

| Function | Description |
|----------|-------------|
| `loadCSV()` | Opens folder picker, finds floor.csv, parses it |
| `processDir(dirHandle)` | Reads floor.csv from a directory handle, builds category list |
| `parseCSV(text)` | Parses CSV text, returns `[{path, answer}]` |
| `buildCategories(rows)` | Groups questions by category folder name, populates dropdown |
| `findImageHandle(dirHandle, filename)` | Tries exact filename then all supported extensions |
| `showPicture(idx)` | Loads image, broadcasts it as ArrayBuffer, updates QM answer bar |
| `showAnswer(mode)` | Displays answer overlay — `'correct'` (white/1s) or `'pass'` (red/3s) |
| `startRound()` | Called by START button — shows first picture, starts timer |
| `resetRound(suppressStart)` | Resets all state; if `suppressStart=true` skips returning to GET READY |
| `tick()` | Called every 100ms — decrements active player's clock, broadcasts state |
| `endRound(reason)` | Shows winner overlay; reason: `'timeout'` or `'pictures'` |
| `snapshot()` | Builds serialisable state object from current DOM/vars |
| `broadcastState()` | Posts snapshot to BroadcastChannel (QM only) |
| `applyState(s)` | Renders received state onto the player window DOM |
| `openPlayerWindow()` | Opens/refocuses the player screen window |

## Persistence

The folder handle is saved to IndexedDB (`thefloor` database, `handles` store, key `rootDir`). On page load the saved handle is restored and permission re-requested if needed. Only the QM window uses IndexedDB.

## Browser Compatibility

Requires **Chrome or Edge** (Chromium-based). The File System Access API and `BroadcastChannel` are not fully supported in Firefox or Safari. **Popups must be allowed** for the player window to open automatically.

## Styling

All colours are CSS variables in `:root`. Key values:

```css
--accent:  #e8c44a   /* gold — logo, badges, start button */
--p1:      #4a9ee8   /* blue — player 1 */
--p2:      #e84a7a   /* pink/red — player 2 */
--danger:  #e84a4a   /* red — pass answer, time warnings */
--warning: #e8a44a   /* amber — clock warning colour */
```

Fonts loaded from Google Fonts: `Bebas Neue` (display/headings) and `Barlow Condensed` (body).
