# Product Overview

**Gemini Commit Assistant** is an AI-powered CLI tool that generates intelligent commit messages using Google's Gemini 2.5 Flash model.

## Core Purpose

- Automates commit message generation by analyzing git diffs
- Supports both Korean and English languages with automatic detection
- Provides sustainable usage through Gemini CLI (60 requests/minute + 1,000 requests/day)
- Follows conventional commit format standards

## Key Features

- **Primary command**: `aic` (with `ai-commit` as alias for backward compatibility)
- **Smart staging**: `--all` flag to stage all files before committing
- **Language support**: Korean/English with `--configure` option
- **Git integration**: Optional git alias setup with `--setup`
- **Fallback system**: Graceful degradation to manual editor when AI unavailable
- **Interactive workflow**: Edit, custom, or cancel options for generated messages

## Target Users

- Developers who want consistent, meaningful commit messages
- Teams requiring standardized commit formats
- Users preferring AI assistance over manual message writing

## Dependencies

- Node.js 16.0.0+
- Git 2.0+
- @google/gemini-cli (peer dependency)
- Google account for Gemini authentication
