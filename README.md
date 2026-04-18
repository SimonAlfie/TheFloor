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

Open `the-floor.html` in a modern browser (Chrome or Edge recommended — they fully support the File System Access API used to read local files).

### 4. Load your content

Click **Load floor.csv** and select the game folder (the folder containing `the-floor.html`). The game reads the CSV and scans the folder for images. Your categories will appear in the dropdown.

The browser will remember the folder between sessions and ask to re-confirm access on next visit.

---

## Playing the game

1. **Select a category** from the dropdown. A "Get Ready" screen shows the category name and picture count.
2. **Enter player names** by clicking on the default names at the top of each player panel.
3. Press **Start** to begin. Each player starts with **45 seconds**.
4. The active player's clock counts down while they try to identify the image.
5. Press **Correct** when the active player gives the right answer — the clock pauses briefly, the answer is revealed, and play switches to the other player.
6. Press **Pass (−3s)** to skip the current image. Three seconds are deducted immediately and the same player continues with the next image.
7. A player is eliminated when their clock reaches zero. The other player wins the round.
8. If all pictures are used up before either player runs out of time, the player with the higher score wins.

### Timer colours

| Colour | Meaning |
|--------|---------|
| Blue / Pink (normal) | More than 10 seconds remaining |
| Amber | 10 seconds or fewer |
| Red (flashing) | 5 seconds or fewer |

---

## Generating a test CSV

A helper script is included. See `generate-test-csv.md` for instructions — it scans your image folders and generates a `floor.csv` with placeholder answers derived from the filenames.
