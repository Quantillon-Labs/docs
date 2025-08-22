# Quantillon Documentation

This directory contains documentation assets for the Quantillon project, including presentation materials and the whitepaper sources/build pipeline.

## Quick start

- View presentation assets: open files in `presentation/`.
- Build the whitepaper PDF:
  1) Ensure prerequisites are installed (Pandoc + XeLaTeX). See `whitepaper/README.md` for exact packages per OS.
  2) From the repository root or from `docs/`, run:
     ```bash
     make -f whitepaper/Makefile.pandoc
     ```
  3) The final PDF will be emitted to `whitepaper/generated/<docname>.pdf` where `<docname>` matches the selected source `.docx` base name.

## Source selection (whitepaper)

- By default, the whitepaper build auto-selects the highest-version `.docx` in `whitepaper/src/` (natural version sort, so `v1.2` > `v1`).
- You can explicitly override the source file (handles spaces):
  ```bash
  make -f whitepaper/Makefile.pandoc DOCX="whitepaper/src/Quantillon Protocol Whitepaper v1.2.docx"
  ```

## Outputs (whitepaper)

- `whitepaper/generated/<docname>.pdf` — Final typeset PDF
- `whitepaper/converted.tex` — Pandoc-converted LaTeX body (intermediate)
- `whitepaper/media/` — Extracted images from the source document

## Troubleshooting

Refer to `whitepaper/README.md` for common issues and their fixes (e.g., Pandoc or TeX Live not installed, missing LaTeX packages).

## Contributing

- Presentation: add or update assets in `presentation/`.
- Whitepaper: update sources under `whitepaper/src/` and rebuild; adjust LaTeX template in `whitepaper/quantillon_whitepaper_template.tex` if layout changes are needed. 