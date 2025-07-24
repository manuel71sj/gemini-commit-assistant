---
inclusion: always
---

# Core Development Rules

## Communication Standards

- **Always respond in Korean** - All user communication and explanations must be in Korean
- **Detailed explanations** - Provide comprehensive explanations with implementation details
- **Korean code comments** - All code comments, docstrings, and commit messages in Korean
- **English for Kiro documentation** - All Kiro-related documentation must be written in English
- **Bilingual documentation** - Maintain parallel README.md (English) and README.ko.md (Korean)

## CLI Tool Conventions

- **Primary command**: `aic` with `ai-commit` as backward compatibility alias
- **POSIX-compliant bash** - Main executable must work across macOS, Linux, Windows
- **Interactive prompts** - Always provide edit/custom/cancel options for generated content
- **Graceful fallbacks** - Degrade to manual editor when AI services unavailable
- **Colored output** - Use consistent color coding for user experience

## Code Architecture Rules

- **Minimal dependencies** - Keep runtime dependencies to absolute minimum
- **Single bash script** - All CLI logic in `bin/aic` executable
- **Node.js for tooling only** - Use Node.js only for post-install and development scripts
- **Cross-platform paths** - No platform-specific file paths or commands
- **Executable permissions** - Always ensure `chmod +x` on bash scripts before publishing

## AI Integration Guidelines

- **Gemini 2.5 Flash model** - Use Google's Gemini CLI as peer dependency
- **Rate limit awareness** - Respect 60 requests/minute + 1,000 requests/day limits
- **Conventional commits** - Generate messages following conventional commit format
- **Language detection** - Auto-detect Korean/English based on user configuration
- **Error handling** - Comprehensive fallback mechanisms for API failures

## Development Workflow

- **Semantic versioning** - Follow semver for all releases
- **Pre-publish checks** - Run `npm pack --dry-run` before publishing
- **Permission setup** - Execute `chmod +x bin/*` before any publish
- **Local testing** - Use `npm install -g .` for local installation testing
- **Configuration management** - Store user config in `~/.gemini-commit-config.json`
- **No testing or server execution** - Development completion does not require separate testing or server execution
- **No automatic commits** - Do not automatically commit changes

## Documentation and Research

- **Use Context7 for libraries** - Always use Context7 for latest documentation and code for all libraries
- **Implementation with details** - Implement with detailed explanations and comprehensive implementation details
