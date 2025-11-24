# AdoptNest-jsDelivr

Repository for storing and serving AdoptNest image assets via jsDelivr (GitHub + jsDelivr-friendly structure).

AdoptNest-jsDelivr stores static images organized by folder (blog, pets, stories, surrenders, users, etc.). Images are named and committed to the repo so they can be served through jsDelivr or referenced directly by filename.

## Stack

- **GitHub repository** — Source of truth for images
- **jsDelivr** — CDN for serving images from GitHub repos
- **(Optional) GitHub Actions** — CI to automate image publish/cleanup

## Commands

```bash
# clone the repo
git clone https://github.com/<your-username>/AdoptNest-jsDelivr.git
cd AdoptNest-jsDelivr

# add new image
mkdir -p pets
cp /path/to/my-image.jpg pets/pet-123.jpg

# commit & push
git add .
git commit -m "Add pet image: pet-123.jpg"
git push origin main
```

## Environment Variables (examples)

These are useful if you automate uploads or run CI:

- `GIT_TOKEN=ghp_xxx`        # Personal access token or GITHUB_TOKEN in Actions
- `GIT_USERNAME=your-username`
- `GIT_REPO=AdoptNest-jsDelivr`
- `GIT_BRANCH=main`

**Required:** `GIT_REPO`, `GIT_BRANCH` (for automation); `GIT_TOKEN` if using a script outside Actions.

## CI (optional) — GitHub Actions example

This short example shows how to run a workflow that can add/commit files programmatically (use carefully — adapt to your workflow).

```yaml
name: Publish Images

on:
  push:
    paths:
      - 'uploads/**'

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Commit images (example)
        run: |
          git config user.name "${{ secrets.GIT_USERNAME }}"
          git config user.email "${{ secrets.GIT_USERNAME }}@users.noreply.github.com"
          git add uploads/*
          git commit -m "Add uploaded images" || echo "No changes"
          git push origin main
    env:
      GIT_TOKEN: ${{ secrets.GIT_TOKEN }}
```

**Tip:** Prefer using `GITHUB_TOKEN` (built-in) for pushes from Actions when possible. Store `GIT_TOKEN` / `GIT_USERNAME` in repository Secrets.

## Manual workflow (recommended for small updates)

1. Add your images into the appropriate folder (e.g., `pets/`, `blog/`, `stories/`).
2. Use descriptive filenames (e.g., `pet-100.jpg`, `blog-50.jpg`, `story-20.jpg`).
3. Commit and push:

```bash
git add .
git commit -m "Add pets image: pet-100.jpg"
git push origin main
```

4. Access via jsDelivr:

```
https://cdn.jsdelivr.net/gh/<username>/AdoptNest-jsDelivr@main/pets/pet-100.jpg
```

## Project structure

```
/
├── blog/          # blog images (blog-*.jpg)
├── pets/          # pet images (pet-*.jpg)
├── stories/       # success stories images (story-*.jpg)
├── surrenders/    # surrender forms images
├── users/         # user avatars / profile images
└── README.md
```

The repository organizes images by folder name — each folder stores images that match their naming convention so they are easy to find and fetch.

## Development tasks / Usage

- Ensure images are optimized (reasonable dimensions & compression) before committing.
- Use consistent naming so clients can predict filenames (e.g., `pet-<id>.jpg`).
- When updating many images, prefer a CI workflow to batch-commit and push changes.

### Preview seeded images locally

No build step is required — images are static. To validate:

1. Clone the repo
2. Inspect files in `pets/`, `blog/`, etc.
3. Verify jsDelivr URL works after pushing to main:

```
https://cdn.jsdelivr.net/gh/<username>/AdoptNest-jsDelivr@main/pets/pet-100.jpg
```

## Quick checklist before publishing

✅ Images optimized and appropriately sized  
✅ Filenames follow convention (`pet-`, `blog-`, `story-*`)  
✅ Pushed to main branch (or `GIT_BRANCH` used by your CDN)  
✅ Secrets set up for any automation (`GIT_TOKEN`, `GIT_USERNAME`)

## Verification

To verify end-to-end:

1. Push an example image to `pets/pet-demo.jpg`.
2. Open the jsDelivr URL in a browser:

```
https://cdn.jsdelivr.net/gh/<username>/AdoptNest-jsDelivr@main/pets/pet-demo.jpg
```

3. Confirm the image loads.

