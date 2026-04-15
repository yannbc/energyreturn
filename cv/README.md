# CV generation workflow

This repo uses a terminal-first workflow for turning Markdown CVs into consistently formatted PDFs on macOS.

## Recommended approach

Use:

- `pandoc` to convert Markdown to HTML
- a dedicated CV stylesheet to control spacing, typography, bullets, and page layout
- `weasyprint` to render HTML to PDF

This is more reliable for CVs than printing raw Markdown from a browser or relying on Pandoc's default PDF output.

## Why this workflow

For CVs, the main requirement is repeatable layout control: section spacing, bullet indentation, line wrapping, margins, and page breaks. HTML + CSS gives much better control over those than raw Markdown-to-PDF conversion.

## One-time setup

Install dependencies on macOS:

```bash
brew install pandoc weasyprint
```

## Usage

From the repo root:

```bash
chmod +x cv/tools/mdcv
cv/tools/mdcv cv/cv-base.md
```

To specify an explicit output path:

```bash
cv/tools/mdcv cv/cv-base.md output/YannBurdenCV.pdf
```

## Files

- `cv/tools/mdcv` — wrapper script for conversion
- `cv/tools/cv.css` — baseline CV styling

## Notes for iterative editing

This setup is intended to be easy to tune with Claude Code or Codex. In practice, the fastest loop is:

1. edit the Markdown
2. regenerate the PDF
3. tweak `cv/tools/cv.css`
4. regenerate again

Most visual improvements should happen in CSS, not in the Markdown.
