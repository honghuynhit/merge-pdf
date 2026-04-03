# Contributing to PDF Merger

Thank you for your interest in contributing! This is a lightweight, client-side PDF merging tool built with plain HTML and JavaScript — no frameworks, no build tools, no server required.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [How It Works](#how-it-works)
- [Making Changes](#making-changes)
- [Contribution Guidelines](#contribution-guidelines)
- [Reporting Issues](#reporting-issues)
- [Feature Ideas](#feature-ideas)

---

## Project Overview

**PDF Merger** is a zero-dependency, browser-based tool that lets users combine two PDF files into one — entirely client-side. No files are ever uploaded to a server.

**Tech stack:**
- Plain HTML5 + CSS3 + Vanilla JavaScript
- [pdf-lib](https://pdf-lib.js.org/) `v1.17.1` (loaded from CDN) — handles PDF parsing and merging

---

## Getting Started

No installation or build step is needed.

```bash
# 1. Clone the repository
git clone https://github.com/your-org/pdf-merger.git
cd pdf-merger

# 2. Open the tool in your browser
open index.html
# or just double-click the file in your file explorer
```

That's it. The tool runs entirely in the browser.

> **Note:** Some browsers restrict local `file://` access. If drag-and-drop or file input doesn't work, serve the file over a simple local server:
> ```bash
> npx serve .
> # or
> python3 -m http.server 8080
> ```

---

## Project Structure

```
pdf-merger/
├── index.html     # The entire application (HTML + CSS + JS in one file)
├── CONTRIBUTING.md     # This file
└── README.md           # User-facing documentation
```

The project is intentionally a **single self-contained file**. All styles and scripts live inside `index.html` to keep distribution simple — users only need to share or download one file.

---

## How It Works

| Step | What happens |
|------|-------------|
| 1. File selection | User picks two PDFs via click or drag-and-drop. Files are stored as `File` objects in memory — nothing is sent anywhere. |
| 2. Reading | `FileReader.readAsArrayBuffer()` converts each file to a raw binary buffer. |
| 3. Parsing | `pdf-lib`'s `PDFDocument.load()` parses each buffer into a PDF object. |
| 4. Merging | Pages are copied from both documents into a new blank `PDFDocument` using `copyPages()` and `addPage()`. |
| 5. Download | The merged document is serialized to bytes with `save()`, wrapped in a `Blob`, and offered as a download via a temporary object URL. |

Key functions in `index.html`:

```
mergePDFs()      — orchestrates the full merge flow
setFile()        — validates and stores a selected file
readFile()       — wraps FileReader in a Promise
onDrop()         — handles drag-and-drop events
clearSlot()      — resets a file slot
```

---

## Making Changes

### Editing styles

All CSS lives in the `<style>` block at the top of `index.html`. CSS custom properties (variables) are defined under `:root` — prefer changing those over scattered hex values.

```css
:root {
  --bg: #0d0d0d;
  --accent: #e8ff47;   /* primary highlight color */
  --success: #4fffb0;  /* used for the download confirmation */
  /* ... */
}
```

### Editing logic

All JavaScript lives in the `<script>` block at the bottom of the file. Keep functions small and focused. The global `files` object holds references to the two selected `File` instances:

```js
const files = { slot1: null, slot2: null };
```

### Updating pdf-lib

The library is loaded from cdnjs. To update the version, change the `<script src>` URL in the `<head>`:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf-lib/1.17.1/pdf-lib.min.js"></script>
```

Check available versions at [cdnjs.com/libraries/pdf-lib](https://cdnjs.cloudflare.com/ajax/libs/pdf-lib/).

---

## Contribution Guidelines

### Code style

- Keep everything in the single `index.html` file.
- Use `const` and `let` — no `var`.
- Prefer `async/await` over raw Promise chains.
- Avoid adding external dependencies beyond `pdf-lib`.
- Write clear, descriptive function and variable names.

### Commits

Use short, descriptive commit messages:

```
fix: handle drag-and-drop on Firefox
feat: show page count per file
style: adjust slot hover color
```

Prefix with `fix:`, `feat:`, `style:`, `docs:`, or `chore:`.

### Pull requests

1. Fork the repository and create a branch from `main`:
   ```bash
   git checkout -b feat/your-feature-name
   ```
2. Make your changes and test in at least two browsers (Chrome + Firefox recommended).
3. Open a pull request with a clear description of what changed and why.
4. PRs that introduce new external scripts or CDN dependencies will need discussion first.

---

## Reporting Issues

Please open a GitHub Issue and include:

- **Browser and version** (e.g. Chrome 123, Safari 17)
- **OS** (e.g. macOS 14, Windows 11)
- **Steps to reproduce** — what did you do?
- **Expected behavior** — what did you expect?
- **Actual behavior** — what happened instead?
- **PDF details** (if relevant) — approximate file size, number of pages, whether the PDF is password-protected

> Password-protected PDFs are not currently supported. `pdf-lib` will throw an error if a locked PDF is loaded.

---

## Feature Ideas

Looking for something to work on? Here are some ideas:

- [ ] Support merging more than 2 PDFs (dynamic slot list)
- [ ] Drag to reorder pages before merging
- [ ] Preview thumbnails for each uploaded PDF
- [ ] Offline support via a `<script>` bundle instead of CDN
- [ ] Custom output filename input
- [ ] Password-protected PDF support

Feel free to open an issue to discuss any of these before starting work.

---

## License

By contributing, you agree that your contributions will be licensed under the same license as this project. See `LICENSE` for details.