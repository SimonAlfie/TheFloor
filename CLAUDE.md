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

1. Open `the-floor.html` in Chrome or Edge
2. Click **📂 Load floor.csv** and select the game folder (remembered between sessions)
3. Select a category from the dropdown
4. **GET READY** screen shows — click **▶ START** to begin
5. Player 1's clock starts running; quizmaster shows the picture
6. Use **✓ CORRECT** or **PASS (−3s)** to control play
7. Round ends when all pictures are shown or a player's clock hits zero

## Controls

| Button | Behaviour |
|--------|-----------|
| **✓ CORRECT** | Stops clock for 1s, shows answer in white, awards point, switches player, advances picture |
| **PASS (−3s)** | Deducts 3s from active player's clock immediately, shows answer in red for 3s, advances picture — **same player continues** |
| **RESET ROUND** | Resets clocks and scores, returns to GET READY screen |
| **NEW ROUND** (winner screen) | Returns to GET READY for same category |

## Constants (top of `<script>`)

```javascript
const ROUND_TIME   = 45;   // seconds per player per round
const PASS_PENALTY = 3;    // seconds deducted on pass
```

Change these to adjust game timing.

## Key Functions

| Function | Description |
|----------|-------------|
| `loadCSV()` | Opens folder picker, finds floor.csv, parses it |
| `processDir(dirHandle)` | Reads floor.csv from a directory handle, builds category list |
| `parseCSV(text)` | Parses CSV text, returns `[{path, answer}]` |
| `buildCategories(rows)` | Groups questions by category folder name, populates dropdown |
| `showPicture(idx)` | Navigates directory handle to load and display image by index |
| `showAnswer(mode)` | Displays answer overlay — `'correct'` (white/1s) or `'pass'` (red/3s) |
| `startRound()` | Called by START button — shows first picture, starts timer |
| `resetRound(suppressStart)` | Resets all state; if `suppressStart=true` skips returning to GET READY |
| `tick()` | Called every second — decrements active player's clock |
| `endRound(reason)` | Shows winner overlay; reason: `'timeout'` or `'pictures'` |
| `switchActive()` | Toggles active player (1↔2), updates panel highlighting |
| `correctAnswer()` | Calls `showAnswer('correct')` |
| `passQuestion()` | Calls `showAnswer('pass')` |

## Persistence

The folder handle is saved to IndexedDB (`thefloor` database, `handles` store, key `rootDir`). On page load the saved handle is restored and permission re-requested if needed. This means the folder picker only needs to be used once.

## Browser Compatibility

Requires **Chrome or Edge** (Chromium-based). The File System Access API (`showDirectoryPicker`, `FileSystemDirectoryHandle`) is not supported in Firefox or Safari.

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
