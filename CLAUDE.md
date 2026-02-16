# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a LaTeX resume/CV for Pradyumn Tendulkar, a Data Science professional. The single source file `main.tex` produces a one-page PDF resume using the `extarticle` document class (9pt, letter paper). The template originates from [RenderCV](https://github.com/sinaatalay/rendercv).

## Build

```bash
pdflatex main.tex
```

Requires a LaTeX distribution (MiKTeX on Windows, TeX Live on macOS/Linux). VS Code users: LaTeX Workshop is configured in `.vscode/settings.json` — save the file or press `Ctrl+Alt+B` to build. If `pdflatex` is not found, update the `command` path in `.vscode/settings.json` to match your local MiKTeX/TeX Live install.

Key packages: `geometry`, `titlesec`, `tabularx`, `xcolor`, `enumitem`, `fontawesome5`, `hyperref`, `eso-pic`, `changepage`, `paracol`, `needspace`, `charter`, `iftex`.

## File Layout

- `main.tex` — The only source file. Contains both the preamble (environment definitions, styling) and the document body (resume content).
- `.vscode/settings.json` — LaTeX Workshop build recipe (pdflatex, no latexmk).
- `main.pdf` — Build output (not committed, listed in `.gitignore` patterns).

## Document Architecture

### Preamble (lines 1–155): Custom Environments

The preamble defines reusable environments that all content sections depend on:

| Environment | Purpose | Usage |
|---|---|---|
| `header` | Centered name + contact info block | Items separated by `\AND` (renders as `\|`) |
| `twocolentry{date}` | Two-column row: content (left), date/location (right, 4.5cm) | Section headers for education and experience |
| `threecolentry` | Three-column variant | Not currently used but available |
| `onecolentry` | Full-width block (via `adjustwidth`) | Wraps degree info, bullet lists, skills |
| `highlights` | Bulleted list (`itemize` with custom spacing) | Experience/project bullet points |
| `highlightsforbulletentries` | Alternate bullet list with tighter left margin | Available but not currently used |

**Critical ordering**: Inside `\begin{document}`, `\newsavebox\ANDbox` and `\sbox\ANDbox` must appear **before** `\newcommand{\AND}` — the `\AND` command references the box.

### Body (lines 156–333): Resume Content

Sections appear in this fixed order:
1. **Header** — Name, location, email, phone, LinkedIn, GitHub
2. **Skills** — Five categories: Languages, ML & NLP, LLM & GenAI, Cloud & MLOps, Data & Tools
3. **Education** — `twocolentry` for school+dates, then `onecolentry` for degree and courses (lines separated by `\\`)
4. **Experience** — `twocolentry` for title+dates, then `onecolentry` > `highlights` for bullet points
5. **Projects** — Same pattern as experience but `twocolentry{}` with empty date column

## Editing Conventions

- **Bullet point style**: Bold key technical terms and technologies within each bullet point. Example: `\item Built system using \textbf{AWS Lambda}, \textbf{Docker}, and \textbf{PostgreSQL} to achieve 99\% accuracy...`
- **Links**: Use `\hrefWithoutArrow{url}{text}` (a saved copy of `\href` without arrow decoration, defined at line 151)
- **PDF metadata**: Update `pdftitle` and `pdfauthor` in the `\usepackage[...]{hyperref}` block (lines 21–29) when changing the resume owner
- **Last updated watermark**: `\placelastupdatedtext` (line 139) places a gray italic timestamp — update the date string inside when modifying content
- **Page margins**: Set to 1.4cm on all sides (lines 6-9) for optimal single-page fit
- **Spacing**: Use `\vspace{0.10 cm}` between entries within a section, `\vspace{0.2 cm}` between experience entries
- **ATS compatibility**: The `\ifPDFTeX` block (lines 41–47) enables glyph-to-unicode mapping for machine-readable PDFs — do not remove

## Common Pitfalls

- **`\\` after `\end{twocolentry}`**: Never place a bare `\\` line break immediately after closing `twocolentry` — there is no active paragraph to break. Wrap subsequent text in `onecolentry` first.
- **`\textbf\\`**: This applies bold to nothing then breaks. Use just `\\` for line breaks, or `\textbf{text}` for bold text.
- **Adding new sections**: Follow the existing pattern — `\section{Title}` then entries using the environment combos above.
- **Special characters**: Escape `%` as `\%`, `&` as `\&`, `$` as `\$`. En-dashes (`–`) can be used directly with UTF-8 input encoding.
