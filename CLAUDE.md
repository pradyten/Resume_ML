# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a LaTeX resume/CV for a Data Science professional. The single source file `main.tex` produces a PDF resume using the `extarticle` document class (9pt, letter paper). The template originates from RenderCV.

## Build Command

```bash
pdflatex main.tex
```

Requires MiKTeX (installed at `C:/Users/Pradyumn K Tendulkar/AppData/Local/Programs/MiKTeX/miktex/bin/x64/`). VS Code LaTeX Workshop is configured in `.vscode/settings.json` to use this path directly.

Key packages: `geometry`, `titlesec`, `tabularx`, `xcolor`, `enumitem`, `fontawesome5`, `hyperref`, `paracol`, `charter`.

## Document Structure

`main.tex` uses custom LaTeX environments for consistent layout:

- **`header`** — Centered name and contact info block at the top, items separated by `\AND` (pipe delimiter)
- **`twocolentry`** — Two-column layout: left column for content, right column (4.5cm) for dates/location
- **`threecolentry`** — Three-column variant with an additional leading column
- **`onecolentry`** — Full-width entry using `adjustwidth`
- **`highlights`** — Bulleted list for achievement items within entries
- **`highlightsforbulletentries`** — Variant bullet list with different spacing

## Editing Conventions

- Sections follow the order: Education, Relevant Experience, Project Experience, Skills
- Education entries use `twocolentry` for the header (school + dates), followed by `onecolentry` for degree and courses (separated by `\\`)
- Each experience entry uses `twocolentry` for the header (title + dates), followed by `onecolentry` with a `highlights` environment for bullet points
- Bold lead phrases in bullet points state the measurable impact, followed by the method/approach
- External links use `\hrefWithoutArrow` (a saved copy of `\href` without arrow decoration)
- The `\placelastupdatedtext` command places a "Last updated" watermark — update the date string when modifying content
- PDF metadata (title, author) is set in the `hyperref` package options near the top of the file
