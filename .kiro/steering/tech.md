# Technology Stack

## Runtime & Platform

- **Node.js**: 16.0.0+ required
- **Shell**: Bash script for main executable
- **Platform**: Cross-platform (macOS, Linux, Windows)

## Package Management

- **npm**: Primary package manager
- **Package type**: Global CLI tool with binary executables
- **Distribution**: Published to npm registry as `gemini-commit-assistant`

## Dependencies

- **@google/gemini-cli**: Peer dependency for AI model access
- **No runtime dependencies**: Keeps package lightweight

## Architecture

- **Main executable**: `bin/aic` (bash script)
- **Post-install script**: `bin/postinstall.js` (Node.js)
- **Configuration**: JSON config stored in `~/.gemini-commit-config.json`

## Build & Development Commands

### Installation

```bash
# Global installation
npm install -g gemini-commit-assistant

# Development setup
npm install
```

### Testing & Validation

```bash
# Dry run package build
npm pack --dry-run

# Test installation locally
npm install -g .
```

### Publishing

```bash
# Automated publish script
./publish.sh

# Manual publish
npm publish --access public
```

### Permissions Setup

```bash
# Required before publishing
chmod +x bin/aic
chmod +x bin/postinstall.js
```

## Code Style

- **Bash**: POSIX-compliant shell scripting
- **JavaScript**: ES6+ for Node.js scripts
- **Internationalization**: Built-in Korean/English support
- **Error handling**: Comprehensive fallback mechanisms
- **User experience**: Interactive prompts with colored output
