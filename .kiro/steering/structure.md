# Project Structure

## Root Directory Layout

```
gemini-commit-assistant/
├── bin/                    # Executable files
│   ├── aic                 # Main CLI executable (bash)
│   └── postinstall.js      # Post-installation script (Node.js)
├── .github/                # GitHub workflows and templates
├── .kiro/                  # Kiro IDE configuration
│   └── steering/           # AI assistant guidance files
├── .vscode/                # VS Code settings
├── CHANGELOG.md            # Version history and changes
├── LICENSE                 # MIT license
├── README.md               # English documentation
├── README.ko.md            # Korean documentation
├── package.json            # npm package configuration
├── package-lock.json       # Dependency lock file
├── publish.sh              # Automated publishing script
├── screenshot.gif          # Demo screenshot
├── .gitignore              # Git ignore patterns
└── .npmignore              # npm publish ignore patterns
```

## Key File Purposes

### Executables (`bin/`)

- **`aic`**: Main bash script containing all CLI logic, internationalization, and AI integration
- **`postinstall.js`**: Node.js script for post-installation messages with language detection

### Documentation

- **`README.md`**: Primary English documentation with usage examples
- **`README.ko.md`**: Korean translation of documentation
- **`CHANGELOG.md`**: Semantic versioning changelog following Keep a Changelog format

### Configuration

- **`package.json`**: Defines binary commands (`aic`, `ai-commit`), peer dependencies, and npm metadata
- **`.gitignore`**: Standard Node.js ignore patterns plus IDE and OS files
- **`.npmignore`**: Controls which files are included in npm package

### Development Tools

- **`publish.sh`**: Interactive publishing script with pre-flight checks
- **`.github/`**: GitHub Actions workflows and issue templates
- **`.vscode/`**: VS Code workspace settings and extensions

## File Organization Principles

- **Minimal package size**: Only essential files included in npm distribution
- **Bilingual support**: Parallel documentation in English and Korean
- **Developer experience**: Comprehensive tooling for development and publishing
- **Cross-platform compatibility**: No platform-specific dependencies or paths
