# The Floor — Picture Quiz

A two-player picture quiz game inspired by the TV show. Players race against the clock to identify images. Run out of time and you're off The Floor.

## Setup

### 1. Folder structure

Organise your images into category sub-folders inside a single game folder:

```
My Quiz/
├── the-floor.html
├── floor.csv
├── Animals/
│   ├── lion.jpg
│   └── penguin.jpg
└── Landmarks/
    ├── eiffel_tower.jpg
    └── big_ben.jpg
```

Any image format works: `.jpg`, `.jpeg`, `.png`, `.gif`, `.webp`.

### 2. Create floor.csv

The CSV tells the game the answer for each image. It must live in the same folder as `the-floor.html`.

```csv
@image,answer
Animals/lion.jpg,Lion
Animals/penguin.jpg,Penguin
Landmarks/eiffel_tower.jpg,Eiffel Tower
Landmarks/big_ben.jpg,Big Ben
```

- The header row must contain `@image` and `answer` (case-insensitive)
- The `@image` path is `CategoryFolder/filename.ext` — match the exact folder and file names on disk
- Extra columns are allowed and will be ignored
- Wrap any value in double quotes if it contains a comma

### 3. Open the game

Open `the-floor.html` in **Chrome or Edge** (Chromium-based browsers only — Firefox and Safari do not support the File System Access API used to read local files).

> **Allow popups.** When the game opens it automatically launches a second window for the player screen. If your browser blocks it, look for the popup blocked notification in the address bar and choose **Always allow popups from this site**. You can also reopen the player window at any time using the **📺 Player Screen** button in the top bar.

> **Allow file access.** When prompted, click **Allow** (or **View files**) to grant the game read access to your folder. The browser will ask again on each new session — this is normal for local file access.

### 4. Load your content

On the **QM window**, click **📂 Load floor.csv** and select the game folder (the folder containing `the-floor.html`). The game reads the CSV and scans the folder for images. Your categories will appear in the dropdown.

The browser will remember the folder between sessions and ask to re-confirm access on next visit. Only the QM window needs to load the folder — images are sent to the player screen automatically.

---

## Dual-screen setup

The game is designed for two screens:

| Window | Purpose |
|--------|---------|
| **QM window** | Quizmaster view — has all controls and always shows the current answer in a bar at the bottom of the image |
| **Player window** | Audience/player view — shows the image and timers only, no controls |

The player window opens automatically when the QM window loads. Both windows stay in sync in real time. If the player window is closed, reopen it with the **📺 Player Screen** button.

**Recommended setup:** drag the player window to the second screen and maximise it before starting the game.

---

## Playing the game

1. **Select a category** from the dropdown on the QM window. A "Get Ready" screen appears on both screens.
2. **Enter player names** by clicking on the default names at the top of each player panel.
3. Press **Start** (or **Space**) to begin. Each player starts with **45 seconds**.
4. The active player's clock counts down while they try to identify the image shown on the player screen.
5. Press **✓ Correct** (or **Space**) when the active player gives the right answer — the clock pauses briefly, the answer flashes on screen, and play switches to the other player.
6. Press **Pass** (or **P**) to skip the current image. The clock keeps running for 3 seconds while the answer is shown, then the same player continues.
7. A player is eliminated when their clock reaches zero. The other player wins the round.
8. If all pictures are used up before either player runs out of time, the player with the higher score wins.

### Keyboard shortcuts

| Key | Action |
|-----|--------|
| **Space** | Correct answer |
| **P** | Pass |

### Timer colours

| Colour | Meaning |
|--------|---------|
| Blue / Pink (normal) | More than 10 seconds remaining |
| Amber | 10 seconds or fewer |
| Red (flashing) | 5 seconds or fewer |

---

## Generating a test CSV

A helper script is included. See `generate-test-csv.md` for instructions — it scans your image folders and generates a `floor.csv` with placeholder answers derived from the filenames.
