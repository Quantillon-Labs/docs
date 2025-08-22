# Quantillon Whitepaper — Build Instructions

This repository generates a typeset PDF whitepaper from the latest versioned DOCX in `src/` using Pandoc and XeLaTeX.

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
By default, the Makefile auto-selects the highest-version `.docx` in `src/` (natural version sort) and builds a PDF with the same base name into `generated/`.

```bash
make -f Makefile.pandoc
```

Outputs:
- `generated/<docname>.pdf` (final PDF, same base name as the selected `.docx`)
- `converted.tex` (Pandoc-converted LaTeX body)
- `media/` (extracted images)

Utility targets:
- `make -f Makefile.pandoc clean` — remove LaTeX aux files
- `make -f Makefile.pandoc veryclean` — also remove PDF, `converted.tex`, and `media/`

## How it works
1. The latest `.docx` in `src/` is selected via natural version sort (e.g., picks `... v1.2.docx` over `... v1.docx`).
2. Pandoc converts the selected DOCX → `converted.tex`, extracting embedded images into `./media/`.
3. A cleanup step removes the DOCX cover image line from `converted.tex` and trims preamble content up to the first numbered section.
4. XeLaTeX compiles `quantillon_whitepaper_template.tex`, which:
   - Includes `converted.tex` as the content body
   - Enables Unicode fonts and common packages
   - Builds an internal PDF (with a safe `jobname`) that is then moved to `generated/<docname>.pdf`

## Customization
- Override the source DOCX (spaces supported):
  ```bash
  make -f Makefile.pandoc DOCX="src/Quantillon Protocol Whitepaper v1.2.docx"
  ```
- Change the output directory (defaults to `generated/`):
  ```bash
  make -f Makefile.pandoc GENERATED_DIR=out
  ```
- Adjust the LaTeX jobname (internal build filename only; final PDF name still follows the DOCX base):
  ```bash
  make -f Makefile.pandoc JOBNAME=my_whitepaper
  ```
- Edit layout/TOC/page-break behavior in `quantillon_whitepaper_template.tex` as needed.

## Troubleshooting
- "No .docx files found" — place at least one versioned `.docx` in `src/` (e.g., `... v1.2.docx`), or override `DOCX` explicitly as shown above.
- "pandoc is not installed" — install Pandoc (see Prerequisites).
- LaTeX package not found or XeLaTeX missing — ensure TeX Live is installed with the collections listed above.
- If your DOCX cover image path differs, adjust the `sed` line in `Makefile.pandoc` that removes the cover image reference.

## Notes
- Filenames with spaces are supported throughout the build.
- The selected DOCX is echoed at build time for easy verification. 