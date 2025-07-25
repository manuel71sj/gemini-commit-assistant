# Design Document

## Overview

This design addresses the bug where the AI Commit Assistant generates commit messages based on all changed files instead of only staged files when using the basic `aic` command. The fix involves modifying the file analysis logic to differentiate between staged-only commits and all-files commits based on the presence of the `--all` flag.

## Architecture

### Current Implementation Analysis

The current code has the following flow:

1. Parse command line arguments and set `STAGE_ALL` flag
2. If `STAGE_ALL=true`, run `git add .` to stage all files
3. Get git status using `git status --porcelain` (includes both staged and unstaged files)
4. Get staged diff using `git diff --cached`
5. Pass both STATUS (all files) and STAGED_DIFF to AI for analysis
6. Generate commit message and create commit

### Problem Identification

The issue occurs in step 5 where the Gemini prompt receives:

- `$STATUS`: Contains both staged and unstaged files from `git status --porcelain`
- `$ANALYSIS_TEXT`: Contains only staged changes from `git diff --cached`

This mismatch causes the AI to see file names from unstaged changes in the status but only staged content in the diff, leading to confusion and inaccurate commit messages.

### Solution Architecture

The solution involves creating two distinct code paths:

1. **Staged-only path** (default `aic` command):

   - Filter `git status --porcelain` to show only staged files
   - Use only staged diff for AI analysis
   - Count only staged files in change summary

2. **All-files path** (`aic --all` command):
   - Maintain current behavior
   - Use full status and diff after staging all files

## Components and Interfaces

### Modified Functions

#### 1. File Status Analysis

```bash
# Current
STATUS=$(git status --porcelain 2>/dev/null)

# Modified
STATUS=$(git status --porcelain 2>/dev/null)
if [ "$STAGE_ALL" = false ]; then
    STAGED_STATUS=$(echo "$STATUS" | grep '^[AMDRC]')
else
    STAGED_STATUS="$STATUS"
fi
```

#### 2. AI Prompt Generation

```bash
# Current prompt uses $STATUS (all files)
Git Status:
$STATUS

# Modified prompt uses filtered status
Git Status:
$STAGED_STATUS
```

#### 3. Change Summary Calculation

```bash
# Current (counts all files)
ADDED_FILES=$(echo "$STATUS" | grep "^??\|^A" | wc -l)
MODIFIED_FILES=$(echo "$STATUS" | grep "^.M\|^M" | wc -l)
DELETED_FILES=$(echo "$STATUS" | grep "^.D\|^D" | wc -l)

# Modified (counts based on mode)
if [ "$STAGE_ALL" = false ]; then
    ADDED_FILES=$(echo "$STAGED_STATUS" | grep "^A" | wc -l)
    MODIFIED_FILES=$(echo "$STAGED_STATUS" | grep "^M" | wc -l)
    DELETED_FILES=$(echo "$STAGED_STATUS" | grep "^D" | wc -l)
else
    ADDED_FILES=$(echo "$STATUS" | grep "^??\|^A" | wc -l)
    MODIFIED_FILES=$(echo "$STATUS" | grep "^.M\|^M" | wc -l)
    DELETED_FILES=$(echo "$STATUS" | grep "^.D\|^D" | wc -l)
fi
```

## Data Models

### Git Status Format

Understanding `git status --porcelain` output format:

```
XY filename
```

Where:

- X: Staged status (A=added, M=modified, D=deleted, R=renamed, C=copied, space=unchanged)
- Y: Unstaged status (M=modified, D=deleted, space=unchanged)

### Filtering Logic

For staged-only commits, we need files where X is not a space:

- `^[AMDRC]`: Files with staged changes
- `^A`: Newly added files (staged)
- `^M`: Modified files (staged)
- `^D`: Deleted files (staged)

## Error Handling

### Edge Cases

1. **No staged files**: Current behavior maintained (show error message)
2. **Mixed staged/unstaged**: Only staged files considered for commit message
3. **All files already staged**: No change in behavior
4. **Empty repository**: Current error handling maintained

### Fallback Mechanisms

- If filtering results in empty status, fall back to current behavior
- Maintain all existing error handling for Gemini CLI failures
- Preserve editor fallback functionality

## Testing Strategy

### Test Scenarios

1. **Basic staged-only commit**:

   - Stage specific files
   - Run `aic`
   - Verify commit message mentions only staged files

2. **Mixed staged/unstaged files**:

   - Stage some files, leave others unstaged
   - Run `aic`
   - Verify unstaged files not mentioned in commit message

3. **All-files commit**:

   - Have unstaged files
   - Run `aic --all`
   - Verify all files are staged and mentioned in commit message

4. **Edge cases**:
   - No staged files with `aic`
   - Empty repository
   - Only unstaged files

### Validation Methods

- Compare commit message content with `git diff --cached --name-only`
- Verify file counts in change summary match staged files
- Test both Korean and English language modes
- Verify Gemini prompt contains only relevant file information

## Implementation Notes

### Backward Compatibility

- `aic --all` behavior remains unchanged
- All existing command line options preserved
- Error messages and user interactions unchanged
- Configuration and setup commands unaffected

### Performance Considerations

- Additional grep filtering has minimal performance impact
- No additional git commands required
- Existing caching and optimization preserved

### Internationalization

- All user-facing messages remain in existing msg() function
- No new translatable strings required
- Both Korean and English modes supported
