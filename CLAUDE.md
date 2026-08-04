# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a LaTeX resume/CV for Pradyumn Tendulkar, an AI Engineer. The single source file `main.tex` produces a one-page PDF resume using the `extarticle` document class (9pt, letter paper). The template originates from [RenderCV](https://github.com/sinaatalay/rendercv).

## Build

```bash
pdflatex main.tex
```

Requires a LaTeX distribution (MiKTeX on Windows, TeX Live on macOS/Linux). VS Code users: LaTeX Workshop is configured in `.vscode/settings.json` — save the file or press `Ctrl+Alt+B` to build. If `pdflatex` is not found, update the `command` path in `.vscode/settings.json` to match your local MiKTeX/TeX Live install.

Key packages: `geometry`, `titlesec`, `xcolor`, `enumitem`, `hyperref`, `changepage`, `paracol`, `needspace`, `charter`, `iftex`.

## File Layout

- `main.tex` — The only source file. Contains both the preamble (environment definitions, styling) and the document body (resume content).
- `.vscode/settings.json` — LaTeX Workshop build recipe (pdflatex, no latexmk).
- `main.pdf` — Build output (not committed, listed in `.gitignore` patterns).

## Document Architecture

### Preamble: Custom Environments

The preamble defines reusable environments that all content sections depend on:

| Environment | Purpose | Usage |
|---|---|---|
| `header` | Centered name + contact info block | Items separated by `\AND` (renders as `\|`) |
| `twocolentry{date}` | Two-column row: content (left), date/location (right, 4.5cm) | Section headers for education and experience |
| `onecolentry` | Full-width block (via `adjustwidth`) | Wraps degree info, bullet lists, skills |
| `highlights` | Bulleted list (`itemize` with custom spacing) | Experience/project bullet points |

**Critical ordering**: Inside `\begin{document}`, `\newsavebox\ANDbox` and `\sbox\ANDbox` must appear **before** `\newcommand{\AND}` — the `\AND` command references the box.

### Body: Resume Content

Sections appear in this fixed order (order is deliberate — set by a 2026 section-ordering research pass: skills high for a tool-heavy technical role, experience before projects, the cert prominent but below the substance, publications/education last):
1. **Header** — Name, location, email, phone, LinkedIn, GitHub, portfolio (three-line layout), immediately followed by an italic one-line summary in `onecolentry`
2. **Skills** — Four categories in a `description` environment with a fixed `labelwidth` for aligned labels: LLM & GenAI, ML & NLP, Cloud & MLOps, Languages & Tools. Use non-breaking spaces (`~`) inside multi-word terms (e.g. `Unity~Catalog`) so they never wrap mid-term.
3. **Experience** — `twocolentry` for title+dates, then `onecolentry` > `highlights` for bullet points
4. **Projects** — Same pattern as experience but `twocolentry{link}` with `\hrefWithoutArrow{url}{Link}` in the right column
5. **Certifications** — `twocolentry{date}` with the cert name (bold) + `Link` on the left, issue date on the right
6. **Publications** — `twocolentry` per paper; title-only (italic), with `Link` in the right column where a URL exists
7. **Education** — `twocolentry` for school+dates, then `onecolentry` > `highlights` for the courses line

## Editing Conventions

- **Bullet point style**: Bold key technical terms and technologies within each bullet point. Example: `\item Built system using \textbf{AWS Lambda}, \textbf{Docker}, and \textbf{PostgreSQL} to achieve 99\% accuracy...`
- **Links**: Use `\hrefWithoutArrow{url}{text}` (a saved copy of `\href` without arrow decoration, defined via `\let\hrefWithoutArrow\href` in the preamble)
- **PDF metadata**: Update `pdftitle` and `pdfauthor` in the `\usepackage[...]{hyperref}` block when changing the resume owner
- **Page margins**: Top=0.8cm, Bottom=0.9cm, Left/Right=1.3cm. **When adding or removing content, check the PDF for top/bottom whitespace imbalance and adjust `top`/`bottom` in the `geometry` options accordingly.** Dynamic vertical centering is not possible due to the header's `\vspace{-2cm}` hack — manual margin balancing is required.
- **Spacing hierarchy** (tuned against a spacing-audit round — biggest gaps between sections, medium between entries, smallest between bullets): section top spacing `0.22 cm` (in `\titlespacing`), `\vspace{0.13 cm}` between entries within a section, `\vspace{0.04 cm}` between an entry title and its bullet points, bullet `itemsep=0.5pt` (in the `highlights` environment). Prefer paying for new content by cutting the weakest/most-redundant entry rather than shrinking these — the current values were set to fix "wall of text" feedback.
- **ATS compatibility**: The `\ifPDFTeX` block in the preamble enables glyph-to-unicode mapping for machine-readable PDFs — do not remove

## Common Pitfalls

- **`\\` after `\end{twocolentry}`**: Never place a bare `\\` line break immediately after closing `twocolentry` — there is no active paragraph to break. Wrap subsequent text in `onecolentry` first.
- **`\textbf\\`**: This applies bold to nothing then breaks. Use just `\\` for line breaks, or `\textbf{text}` for bold text.
- **Adding new sections**: Follow the existing pattern — `\section{Title}` then entries using the environment combos above.
- **Special characters**: Escape `%` as `\%`, `&` as `\&`, `$` as `\$`. En-dashes (`–`) can be used directly with UTF-8 input encoding.
