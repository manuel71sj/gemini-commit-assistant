# Requirements Document

## Introduction

This feature addresses a critical bug in the AI Commit Assistant where the `aic` command generates commit messages based on all changed files (both staged and unstaged) instead of only the staged files when creating commits. This leads to inaccurate commit messages that include changes not intended for the current commit.

## Requirements

### Requirement 1

**User Story:** As a developer using the AI Commit Assistant, I want commit messages to be generated only from staged files when using the basic `aic` command, so that my commit messages accurately reflect the changes I intend to commit.

#### Acceptance Criteria

1. WHEN I run `aic` command without `--all` flag THEN the system SHALL analyze only the staged files for commit message generation
2. WHEN I have both staged and unstaged files and use `aic` without `--all` THEN the system SHALL ignore unstaged files in commit message generation
3. WHEN generating commit messages for staged-only commits THEN the system SHALL use only `git diff --cached` output for AI analysis

### Requirement 2

**User Story:** As a developer, I want the change summary to reflect only staged files when using basic `aic` command, so that I can verify the correct files are being committed.

#### Acceptance Criteria

1. WHEN displaying change summary for `aic` without `--all` THEN the system SHALL count only staged files (added, modified, deleted)
2. WHEN showing file statistics for staged-only commits THEN the system SHALL exclude unstaged files from the count
3. WHEN presenting file information to AI for staged-only commits THEN the system SHALL filter git status to show only staged file states

### Requirement 3

**User Story:** As a developer, I want consistent behavior between the commit message content and the actual commit, so that there are no discrepancies between what the AI describes and what gets committed.

#### Acceptance Criteria

1. WHEN AI generates a commit message THEN the message SHALL describe only the changes that will be included in the commit
2. WHEN the commit is created THEN the committed changes SHALL match exactly what was described in the AI-generated message
3. WHEN analyzing file changes THEN the system SHALL use the same staged file set for both AI analysis and actual commit operation

### Requirement 4

**User Story:** As a developer using `aic --all` or `aic -a`, I want the current behavior to be maintained, so that all files are staged and included in the commit message generation.

#### Acceptance Criteria

1. WHEN I run `aic --all` or `aic -a` THEN the system SHALL stage all files before generating commit message
2. WHEN using `--all` flag THEN the system SHALL analyze all changed files (both previously staged and unstaged) for commit message generation
3. WHEN `--all` flag is used THEN the system SHALL maintain the current behavior of including all file changes in the AI analysis
