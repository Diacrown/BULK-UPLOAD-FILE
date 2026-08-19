# Gemstone Bulk Upload Generator

A single-page, self-contained web tool that scans a folder of item
photos/videos/certificates and generates the platform's bulk-upload Excel
file (`Certificate | Image | Item # | Media4 | Media5 | Media6 | Media7 | Media8 | Report# | Video`)
with S3 link formulas already filled in.

Runs **entirely in your browser** — nothing is uploaded anywhere, no
backend, no server, no install. It reads your local folder via the
browser's File System Access API and writes the `.xlsx` straight to your
downloads.

**Browser support:** Chrome or Edge on desktop. Safari and Firefox don't
support the File System Access API yet, so the folder picker won't work
there.

## Using it

1. Open the page (locally by double-clicking `index.html`, or at whatever
   URL you deploy it to).
2. Pick **Gemstone** or **Natural** — this sets the S3 base link:
   - Gemstone → `https://dnadc.s3.ap-southeast-2.amazonaws.com/GS/{item}/...`
   - Natural → `https://dnadc.s3.ap-southeast-2.amazonaws.com/{item}/...`
   (edit the URL field directly if the bucket layout ever changes)
3. Click **+ Add a folder** and grant access to the folder you want to
   scan. Add more than one if you're combining several batch folders into
   one export.
4. Click **Scan selected folder(s)**. Review the table — any row that's
   missing a photo/video, has an unrecognised file type, or has more than
   one unexplained extra file is flagged for a quick look before you
   export.
5. Click **Download bulk_upload.xlsx**.

## Deploying this

This is a fully static site (one HTML file, no build step), so any static
host works. Two easy options:

### GitHub Pages

```
git init
git add index.html README.md
git commit -m "Gemstone bulk upload generator"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

Then in the repo: **Settings → Pages → Build and deployment → Source:
Deploy from a branch → Branch: main / (root)**. Save, wait about a minute,
and it'll be live at `https://<your-username>.github.io/<repo-name>/`.

### Netlify

Easiest path: go to [app.netlify.com/drop](https://app.netlify.com/drop)
and drag this folder in — it deploys immediately with a live HTTPS URL, no
git required. To keep it updating automatically instead, connect this
GitHub repo from the Netlify dashboard (**Add new site → Import an
existing project**) with:
- Build command: *(none — leave blank)*
- Publish directory: `.`

Netlify is worth it over GitHub Pages if this ever needs a server-side
piece later (e.g. actually pushing files to S3 with credentials, instead
of just generating the link sheet) — GitHub Pages can only serve static
files, Netlify can also run serverless functions.

## Notes for future changes

The scanning logic mirrors a Python script (`generate_bulk_upload.py`,
part of the same toolkit) line-for-line, so behavior stays identical
whether you run the Python version or this page. If you change the
column layout, extension lists, or category URLs, update both.
