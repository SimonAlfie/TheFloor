# Generate Test floor.csv

## Task

Scan the game folder's directory structure and generate a `floor.csv` file suitable for testing the THE FLOOR picture quiz game. Make up plausible one-or-two-word answers for each image based on the filename.

## Instructions

1. **Scan the directory** containing `the-floor.html` for sub-folders. Each sub-folder is a category.

2. **Within each category folder**, find all image files with extensions: `.jpg`, `.jpeg`, `.png`, `.gif`, `.webp` (case-insensitive).

3. **Derive a test answer** from the image filename:
   - Strip the file extension
   - Replace underscores and hyphens with spaces
   - Convert to Title Case
   - Remove any leading numbers and dots (e.g. `1.` or `01_`)
   - Keep it to 1–2 words where possible — if the filename is long, use just the first meaningful word or two
   - Examples: `Giant_Panda.jpg` → `Giant Panda`, `01_eiffel_tower.jpg` → `Eiffel Tower`, `lion.jpg` → `Lion`

4. **Write `floor.csv`** to the same folder as `the-floor.html` with the following format:

```
@image,answer
category/filename.jpg,Answer
```

   - The `@image` column must be the relative path: `category folder name/image filename` (use the exact folder and file names as found on disk, preserving case)
   - No quotes needed unless a value contains a comma
   - Include a header row exactly as shown above

5. **Sort** the output: alphabetically by category, then by filename within each category.

## Example Output

```csv
@image,answer
animals/Giant_Panda.jpg,Giant Panda
animals/lion.jpg,Lion
landmarks/01_eiffel_tower.jpg,Eiffel Tower
landmarks/02_big_ben.jpg,Big Ben
```

## Notes

- Do not recurse deeper than one level — only immediate sub-folders of the game folder are categories
- Skip any files that are not images (e.g. `.csv`, `.html`, `.md`)
- Skip the game folder's own files at the root level
- If the folder is empty or has no images, skip it silently
- Overwrite `floor.csv` if it already exists
- Print a summary when done: number of categories found, total images added
