# Generate Test Data

## Task

For each category specified by the user, fetch 45 images from the web, save them into numbered folders, and generate a `floor.csv` with the correct answer for each image.

## Instructions

### 1. Get categories from the user

Ask the user which categories they want to generate (e.g. "F1 Drivers", "Capital Cities", "Dog Breeds"). Each category becomes a folder.

### 2. For each category, find 45 subjects

Come up with 45 distinct, well-known subjects that fit the category. For example, for "F1 Drivers": Lewis Hamilton, Max Verstappen, Ayrton Senna, etc.

### 3. Fetch an image for each subject

For each subject, search the web for a clear, recognisable image (a photo or illustration where the subject is easily identifiable). Download the image file.

- Save it into a sub-folder named after the category (e.g. `F1 Drivers/`)
- Number the files sequentially: `1.jpg`, `2.png`, `3.jpg`, etc. — use the actual file extension of the downloaded image
- Use only supported formats: `.jpg`, `.jpeg`, `.png`, `.gif`, `.webp`

### 4. Write floor.csv

Write a `floor.csv` file in the same folder as `the-floor.html` with the following format:

```csv
@image,answer
F1 Drivers/1.jpg,Lewis Hamilton
F1 Drivers/2.jpg,Max Verstappen
```

- Header row must be exactly `@image,answer`
- The `@image` path is `category folder/filename.ext` — match the exact folder and file names on disk
- The `answer` is the subject's name (1–2 words where possible)
- Sort: alphabetically by category, then numerically by filename within each category
- If a category folder already exists, append to it and update the CSV accordingly
- Update `floor.csv` if it already exists, preserving any existing entries

## Notes

- Prefer images with a plain or simple background where the subject is clearly identifiable
- Avoid images with text overlays, watermarks, or collages
- If an image cannot be fetched for a subject, skip it and try another subject so the folder always has 45 images
- Print a summary when done: categories generated, images downloaded per category, any subjects skipped
