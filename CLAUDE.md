# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repo manages documentation and infrastructure docs for the Quantillon Protocol (Euro-native DeFi / QEURO stablecoin). It produces publication-ready PDFs from DOCX sources and stores VPS architecture documentation.

## Build Commands

The Makefile must be run **from the `whitepaper/` directory** (it uses relative paths internally):

```bash
cd whitepaper

# Build whitepaper — auto-selects highest version from src/
make -f Makefile.pandoc

# Build a specific version
make -f Makefile.pandoc DOCX="src/Quantillon Protocol Whitepaper v1.2.docx"

# Remove LaTeX auxiliary files only
make -f Makefile.pandoc clean

# Full cleanup (removes PDF, converted.tex, media/)
make -f Makefile.pandoc veryclean
```

## Build Pipeline Architecture

1. **Version detection**: `ls -1 src/*.docx | sort -V | tail -n 1` picks the highest semver DOCX
2. **Pandoc conversion**: DOCX → `converted.tex` (LaTeX body fragment), with images extracted to `media/`
3. **Content cleanup** (inline in Makefile):
   - `sed` removes the cover image line (`{media/image1.png}`)
   - `awk` trims everything before the first `\section{1.` or `\subsection{1.` heading
4. **XeLaTeX two-pass**: `quantillon_whitepaper_template.tex` `\input`s `converted.tex`; compiled twice for cross-references
5. **Output**: `quantillon_whitepaper.pdf` → renamed to `generated/<BASE_NAME>.pdf`

**Key relationship**: `quantillon_whitepaper_template.tex` is the master LaTeX document that provides all preamble/packages/formatting. It `\input`s `converted.tex` which contains only the body content from Pandoc. Never mix them — the template handles all package declarations.

`converted.tex` is **not gitignored** — it is committed as a build artifact alongside the PDF.

## Conventions

- DOCX source files: `whitepaper/src/Quantillon Protocol Whitepaper vX.Y.docx`
- Font: TeX Gyre Pagella (XeLaTeX only — pdflatex will fail)
- Cover image filtering targets `/media/image1.png` — if Pandoc extracts the cover under a different name, update the `sed` line in `Makefile.pandoc`
- Presentation assets (`presentation/`) are gitignored — stored locally only

## CI/CD

`.github/workflows/Telegram Notifications.yml` sends a Telegram message on push to `main` or `dev`. Requires `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHANNEL_ID` repo secrets. No automated PDF builds in CI.

## Infrastructure Reference

`protocol/architecture.md` documents the VPS deployment topology (written in French). Key facts:
- **Nginx** reverse-proxies across prod/dev/privatedemo environments
- **PM2** manages all Node.js services (Express API, Funding Monitor, Indexer)
- **Port allocation**: Main API 4000/4001, Funding Monitor 4101/5101/6101, Indexer 4102/5102/6102 (prod/dev/privatedemo)
- **PostgreSQL 17** on localhost only; databases: `quantillon_profiles`, `umami`
- **Anvil** (Base fork) on port 8545, proxied via `privatedemo.quantillon.money/rpc`
- SSH on non-standard port **54321** with Fail2ban (3 failures → 24h ban)
