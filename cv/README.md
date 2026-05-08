# CV generation workflow

Terminal-first pipeline for turning Markdown CVs into consistently formatted PDFs on macOS, using `pandoc` + `weasyprint` via the `cv/tools/mdcv` wrapper. The PDF stylesheet (`cv/tools/cv.css`) is aligned with the landing site so every artefact shares the same design language.

---

## One-time setup

```bash
brew install pandoc weasyprint
```

## Render a CV or cover letter

```bash
cv/tools/mdcv cv/cv-base.md                          # -> cv/cv-base.pdf
cv/tools/mdcv cv/cv-base.md /tmp/YannBurden_CV.pdf   # explicit output
cv/tools/mdcv -l applications/{slug}/cover-letter.md # cover letter mode
```

Most editing iterations are: edit markdown -> regenerate -> open PDF -> iterate.

---

## Files

| Path | Purpose |
|---|---|
| `cv/cv-base.md` | Primary CV. AI / product / CTO emphasis. Source-of-truth for shared content (highlights, roles, technical, press, research). |
| `cv/cv-consulting.md` | Variant for advisory / consulting / GTM roles. |
| `cv/cv-ai-leadership.md` | Variant for AI deployment / forward-deployed engineering roles. |
| `cv/cv-relevance.md`, `cv/cv-decidr.md` | Older role-specific variants -- treat as historical, refresh on demand if reused. |
| `cv/tools/mdcv` | Wrapper script -- pandoc to HTML, weasyprint to PDF. |
| `cv/tools/cv.css` | CV stylesheet. Mirrors `site/index.html` design tokens. |
| `cv/tools/letter.css` | Cover-letter stylesheet -- updated in lockstep with `cv.css`. |

---

## Creating a bespoke CV for a specific role

When a target role needs significant content tailoring -- different headline, different focus, different highlight order, role-specific bullets -- create a variant rather than mutating a base CV in place.

### Process

1. **Read `CLAUDE.md` first.** The framing rules are non-negotiable: Pendula was agentic, Billcap was not, 8x investor return is the headline metric, DC Power Co customer book sold to Ion Group (not a true exit), 25-person team, etc.
2. **Pick a starting template** that's closest to the target role:
   - AI / product / CTO -> copy `cv-base.md`
   - Advisory / consulting / GTM -> copy `cv-consulting.md`
   - AI deployment / forward-deployed engineering -> copy `cv-ai-leadership.md`
3. **Save as** `cv/cv-{slug}.md` (kebab-case slug, descriptive of the role family).
4. **Edit content, not structure.** The markdown shape is what `cv.css` styles. Stick to the conventions below.
5. **Render** with `cv/tools/mdcv cv/cv-{slug}.md /tmp/preview.pdf` and inspect the PDF.
6. **Iterate on content, not CSS.** If pagination feels off, the answer is almost always shorter / longer copy, not tuning the stylesheet per CV.

### Markdown conventions (so cv.css renders correctly)

```markdown
# Yann Burden

**One-line tagline. Exited Founder. Hook.**

+61 400 941 979 | yannburden@gmail.com | energyreturn.co/me | linkedin.com/in/yannburden

Working rights: Australia, UK, EU, Canada. Native English and French.

---

## Highlights

- Bullet text (each bullet is one strong claim).

## Focus

Tag | Tag | Tag | Tag | Tag

---

## Experience

### Company Name | YYYY - YYYY
**Role Title**

- Bullet 1.
- Bullet 2.

## Earlier Experience

- **Company** | YYYY - YYYY | Role. Short description.

---

## Advisory and Boards

**Org Name** | Role (YYYY - YYYY)

---

## Education

**Org Name** | Degree (Field), Year

---

## Press and Media

- **[Publication](url)** (Month Year): Description.

## Research Collaborations

Optional intro paragraph.

- **[Title](url).** Authors. *Journal*, Year.

---

## Technical

- **Capability statement.** Body sentence with concrete tech as evidence.
```

Hyperlinks: `[text](url)` for inline links, `**[text](url)**` for bold + link (used for press publication names).

The Press section gets ochre styling automatically -- `cv.css` targets `section#press-and-media` (and `section#press`) by ID, which pandoc generates from the heading slug.

---

## Hidden share URLs (markdown on the website)

For sharing a CV directly with a recruiter or for machine-readable ingest by AI agents:

1. Copy the markdown to `site/cv/{slug}.md`.
2. Commit and push.
3. After the Pages workflow runs (~30s), the file is live at `https://energyreturn.co/cv/{slug}.md`.
4. Don't link it from navigation -- share the URL directly.

Browsers display markdown as raw text. That's fine: tech-literate readers can read it, AI agents can parse it cleanly. If you want a styled HTML page instead, that's a different exercise (template + render).

---

## Pagination

A senior CV runs 2-3 pages. The current cv.css enforces:

- Each role (h3 section) stays together (`break-inside: avoid` via `section.level3`).
- h2 sections may flow across pages.
- A4 page size, 16-18mm margins.

You will sometimes see:

- **Empty space at the bottom of an early page.** This happens when the next h2 section is too tall to start at the bottom -- it pushes to the next page, leaving breathing room behind. Acceptable; reads as polished, not wasted, on a senior CV.
- **A list splitting across pages** (e.g., Earlier Experience). Acceptable. The h2 stays with at least the first item, then bullets continue on the next page.

If a section gets orphaned on a near-empty page, the fix is content (shorten / merge / reorder), not CSS. **Don't tune `cv.css` per CV** -- it's a shared design system across all CVs and cover letters.

---

## Design system

`cv/tools/cv.css` mirrors `site/index.html`:

- **oklch tokens**: `--paper`, `--ink`, `--ink-2`, `--ink-3`, `--rule`, `--forest`, `--ochre`.
- **Two-rule grammar**: 2px ink for section breaks, 1px rule for sub-elements.
- **Type scale**: Source Serif 4 (display weight 300, body weight 400), JetBrains Mono for meta / labels with letter-spacing.
- **Colour roles**:
  - Forest = section titles + bullet markers (operating colour).
  - Ochre = Press and Media (external voice mark).
  - Ink hierarchy = body, headings, dates, secondary text.

**When updating the design system, update `cv.css` and `letter.css` together** so the CV and cover letter stay visually coherent. Do not tune one without the other.
