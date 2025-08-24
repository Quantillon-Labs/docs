# Quantillon Protocol

<div align="center">
  <img src="https://github.com/Quantillon-Labs/gitbook/raw/main/.gitbook/assets/banner.png" alt="Quantillon Protocol Banner" width="100%">
</div>

# Quantillon Documentation

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Pandoc](https://img.shields.io/badge/Pandoc-Latest-blue.svg)](https://pandoc.org/)
[![XeLaTeX](https://img.shields.io/badge/XeLaTeX-Latest-orange.svg)](https://www.latex-project.org/)

> **Documentation repository for Quantillon Protocol - Euro-pegged stablecoin ecosystem documentation and presentation assets**

## 📁 Repository Structure

This repository contains all documentation assets for the Quantillon Protocol ecosystem, organized for both technical documentation and presentation materials.

```
docs/
├── presentation/               # Presentation assets and slides
│   ├── slides/                # PowerPoint/PDF presentations
│   ├── images/                # Visual assets and diagrams
│   └── templates/             # Presentation templates
├── whitepaper/                # Whitepaper source and build pipeline
│   ├── src/                   # Source documents (.docx format)
│   ├── generated/             # Generated PDF outputs
│   ├── media/                 # Extracted images and assets
│   ├── Makefile.pandoc        # Build automation
│   ├── quantillon_whitepaper_template.tex  # LaTeX template
│   └── README.md              # Detailed build instructions
└── README.md                  # This file
```

## 🚀 Quick Navigation

### Documentation Assets
**[📊 presentation/](./presentation/)** - Presentation slides and visual assets

### Whitepaper
**[📄 whitepaper/](./whitepaper/)** - Complete whitepaper build pipeline and sources

### Key Files
- **[📋 Makefile.pandoc](./whitepaper/Makefile.pandoc)** - Automated build system
- **[📝 LaTeX Template](./whitepaper/quantillon_whitepaper_template.tex)** - Professional typesetting template
- **[📚 Build Instructions](./whitepaper/README.md)** - Comprehensive setup and build guide

## 🎯 What is Quantillon Protocol?

Quantillon Protocol is a comprehensive DeFi ecosystem built around **QEURO**, a Euro-pegged stablecoin designed for European markets. The protocol includes:

### Core Components:

- **QEURO**: Euro-pegged stablecoin with overcollateralized backing
- **stQEURO**: Yield-bearing wrapper with auto-compounding mechanics  
- **QTI**: Governance token with vote-escrow mechanics for protocol governance
- **Dual-Pool Architecture**: Separation of user deposits from hedging operations
- **Native EUR/USD Hedging**: Integrated foreign exchange risk management

### Key Features:
- **MiCA Compliance**: Designed for European regulatory framework
- **Overcollateralization**: USDC-backed security model
- **Yield Generation**: Multiple revenue streams for token holders
- **Decentralized Governance**: DAO-controlled protocol evolution

## 📄 Whitepaper Quick Start

### Building the Whitepaper PDF

1. **Install Prerequisites**: Ensure Pandoc and XeLaTeX are installed (see [detailed instructions](./whitepaper/README.md))

2. **Build from Repository Root**:
   ```bash
   make -f whitepaper/Makefile.pandoc
   ```

3. **Output Location**: 
   ```
   whitepaper/generated/<docname>.pdf
   ```

### Advanced Build Options

**Custom Source Selection**:
```bash
make -f whitepaper/Makefile.pandoc DOCX="whitepaper/src/Quantillon Protocol Whitepaper v1.2.docx"
```

**Build from Whitepaper Directory**:
```bash
cd whitepaper/
make -f Makefile.pandoc
```

## 🔄 Document Versioning

The build system automatically selects the highest version number from `whitepaper/src/`:
- **Version Detection**: Natural version sorting (`v1.2` > `v1.1` > `v1`)
- **Auto-Selection**: Latest version built by default
- **Manual Override**: Explicit source file specification supported

## 📊 Presentation Assets

### Available Materials
- **Pitch Decks**: Investor and partnership presentations
- **Technical Diagrams**: Protocol architecture visualizations
- **Business Model**: Economic structure and revenue flows
- **Roadmap Slides**: Development timeline and milestones

### Usage
View and use presentation files directly from the `presentation/` directory. All assets are designed for professional use with investors, regulators, and technical audiences.

## 🛠️ Development Workflow

### Prerequisites
- **Pandoc** (latest version) - Document conversion
- **XeLaTeX** (TeX Live distribution) - PDF generation
- **Git** - Version control

### Building Documentation
```bash
# Clone repository
git clone https://github.com/quantillon-labs/docs.git
cd docs

# Build whitepaper
make -f whitepaper/Makefile.pandoc

# Serve generated documentation locally
cd whitepaper/generated/
python -m http.server 8000
```

### File Generation Process
1. **Source Processing**: `.docx` files converted via Pandoc
2. **LaTeX Generation**: Professional typesetting applied
3. **Media Extraction**: Images and diagrams automatically processed
4. **PDF Output**: Final publication-ready document generated

## 📋 Build Outputs

### Generated Files
- **`whitepaper/generated/<docname>.pdf`** — Final typeset PDF
- **`whitepaper/converted.tex`** — Pandoc-converted LaTeX body
- **`whitepaper/media/`** — Extracted images and assets

### Intermediate Files
All intermediate build files are automatically managed by the Makefile system.

## 🔧 Configuration

### LaTeX Template Customization
Modify `whitepaper/quantillon_whitepaper_template.tex` for:
- Layout adjustments
- Typography changes  
- Brand styling updates
- Additional sections

### Build System Configuration
The `Makefile.pandoc` supports various configuration options - see [whitepaper README](./whitepaper/README.md) for details.

## 🐛 Troubleshooting

### Common Issues
- **Pandoc not found**: Ensure Pandoc is installed and in PATH
- **XeLaTeX errors**: Install complete TeX Live distribution
- **Missing packages**: Install required LaTeX packages per OS
- **Build failures**: Check whitepaper/README.md for specific solutions

### Getting Help
1. Check [whitepaper/README.md](./whitepaper/README.md) for detailed troubleshooting
2. Verify all prerequisites are properly installed
3. Review build logs for specific error messages

## 🤝 Contributing

We welcome contributions to improve our documentation! 

### Documentation Updates
- **Presentations**: Add or update assets in `presentation/`
- **Whitepaper**: Update source documents in `whitepaper/src/` and rebuild
- **Templates**: Enhance LaTeX template for improved formatting

### Development Workflow
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/documentation-update`)
3. Make your changes
4. Test build process to ensure no regressions
5. Submit a Pull Request

### Content Standards
- **Professional Quality**: All content must meet institutional standards
- **Technical Accuracy**: Verify all technical claims and specifications
- **Brand Consistency**: Follow Quantillon visual identity guidelines
- **Regulatory Compliance**: Ensure all content aligns with MiCA requirements

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

## 🌐 Links

- **Website**: [quantillon.money](https://quantillon.money)
- **Documentation**: [docs.quantillon.money](https://docs.quantillon.money)
- **X (Twitter)**: [@QuantillonLabs](https://x.com/QuantillonLabs)
- **Discord**: [discord.gg/uk8T9GqdE5](https://discord.gg/uk8T9GqdE5)
- **Telegram**: [@QuantillonLabs](https://t.me/QuantillonLabs)

## 🙏 Acknowledgments

- **Pandoc Team** for excellent document conversion capabilities
- **LaTeX Community** for professional typesetting standards
- **European DeFi Community** for regulatory guidance and feedback
- **Contributors** for continuous documentation improvements

---

**Built with ❤️ by the Quantillon Labs team**