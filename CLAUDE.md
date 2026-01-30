# Quantillon Protocol Documentation

This repository manages documentation and presentation assets for the Quantillon Protocol, a Euro-native DeFi ecosystem built around QEURO (Euro-pegged stablecoin).

## Purpose

- Build publication-ready PDF whitepapers from DOCX source files via Pandoc + XeLaTeX
- Store presentation decks and visual assets for investor/regulatory meetings
- Document VPS infrastructure architecture and deployment topology

## Tech Stack

- **Pandoc**: Document conversion (DOCX → LaTeX)
- **XeLaTeX (TeX Live)**: Professional PDF generation with Unicode support
- **GNU Make**: Build automation
- **LaTeX Packages**: geometry, setspace, graphicx, longtable, booktabs, hyperref, fontspec, polyglossia

## Repository Structure

```
docs/
├── whitepaper/                    # Document build pipeline
│   ├── Makefile.pandoc           # Build automation
│   ├── quantillon_whitepaper_template.tex  # LaTeX master template
│   ├── src/                      # Source DOCX files (versioned)
│   ├── generated/                # PDF output
│   └── media/                    # Extracted images
├── presentation/                  # Slides & visual assets (git-ignored)
├── protocol/
│   └── architecture.md           # VPS infrastructure documentation
└── .github/workflows/            # Telegram notification CI
```

## Build Commands

```bash
# Build whitepaper (auto-selects highest version from src/)
make -f whitepaper/Makefile.pandoc

# Build specific version
make -f whitepaper/Makefile.pandoc DOCX="whitepaper/src/Quantillon Protocol Whitepaper v1.2.docx"

# Clean intermediate files
make -f whitepaper/Makefile.pandoc clean

# Full cleanup (removes PDF, converted.tex, media/)
make -f whitepaper/Makefile.pandoc veryclean
```

## Prerequisites

```bash
# Ubuntu/Debian
sudo apt install -y pandoc texlive-xetex texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended

# macOS
brew install pandoc && brew install --cask mactex-no-gui
```

## Build Pipeline

1. **Version Detection**: Natural version sort finds highest `.docx` in `whitepaper/src/`
2. **Pandoc Conversion**: DOCX → LaTeX with media extraction
3. **Content Cleanup**: Removes cover image reference, trims preamble
4. **XeLaTeX Compilation**: Two-pass compilation for cross-references
5. **Output**: Final PDF renamed and moved to `generated/`

## Conventions

- Source documents use versioning pattern: `Quantillon Protocol Whitepaper v1.2.docx`
- Template uses TeX Gyre Pagella font (requires XeLaTeX)
- Multilingual support via Polyglossia (English primary, French secondary)
- Document history tracked in LaTeX template table
- Presentation assets excluded from git (stored locally only)

## Important Notes

- All `.docx` source files must be in `whitepaper/src/` with version pattern
- Build requires full TeX Live distribution (not minimal)
- Unicode rendering requires XeLaTeX (pdflatex won't work)
- Cover image filtering assumes path `/media/image1.png` - adjust sed command if different
- CI/CD sends Telegram notifications on push to main/dev (no automated PDF builds)
