# Quantillon Whitepaper — Build Instructions

This repository lets you generate a typeset PDF whitepaper from a DOCX source using Pandoc and XeLaTeX.

## Prerequisites
- Pandoc
- TeX Live with XeLaTeX and common LaTeX packages (geometry, setspace, graphicx, longtable, booktabs, caption, listings, tabularx, array, calc, hyperref, url, microtype, xcolor, ulem, fontspec, polyglossia)

### Quick install
- Ubuntu/Debian:
  ```bash
  sudo apt update
  sudo apt install -y pandoc texlive-xetex texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended
  ```
- macOS (Homebrew):
  ```bash
  brew install pandoc
  brew install --cask mactex-no-gui  # or MacTeX
  ```
- Windows:
  - Install Pandoc (from pandoc.org) and TeX Live (or use WSL and the Ubuntu commands above).

## Build
The default source DOCX is `whitepaper.docx`. To generate the PDF:
```bash
make -f Makefile.pandoc
```
Outputs:
- `quantillon_whitepaper.pdf` (final PDF)
- `converted.tex` (Pandoc-converted LaTeX body)
- `media/` (extracted images)

Utility targets:
- `make -f Makefile.pandoc clean` — remove LaTeX aux files
- `make -f Makefile.pandoc veryclean` — also remove PDF, converted.tex, and `media/`

## How it works
1. Pandoc converts `whitepaper.docx` → `converted.tex`, extracting embedded images into `./media/`.
2. A small cleanup step removes the DOCX cover image line from `converted.tex` and trims preamble content up to the first numbered section.
3. XeLaTeX compiles `quantillon_whitepaper_template.tex`, which:
   - Includes `converted.tex` as the content body
   - Sets the table of contents depth to include subentries (sections and subsections)
   - Inserts a page break before each main entry (mapped to `\subsection`) except the first

## Customization
- Change the source DOCX name by overriding the variable:
  ```bash
  make -f Makefile.pandoc DOCX=my_source.docx
  ```
- Change the output PDF name:
  ```bash
  make -f Makefile.pandoc PDF=my_whitepaper.pdf
  ```
- Edit layout/TOC/page-break behavior in `quantillon_whitepaper_template.tex`:
  - TOC depth: `\setcounter{tocdepth}{3}` (use `2` to include up to subsections only)
  - Page breaks before main entries: see the `\renewcommand{\subsection}{...}` hook

## Troubleshooting
- "pandoc is not installed" — install Pandoc (see Prerequisites).
- LaTeX package not found or XeLaTeX missing — ensure TeX Live is installed with the collections listed above.
- If your DOCX cover image path differs, adjust the `sed` line in `Makefile.pandoc` that removes the cover image reference. 