# Implementation Plan

- [ ] 1. Add staged file filtering logic (Line 474 in bin/aic)

  - Add STAGED_STATUS variable creation after STATUS variable (around line 474)
  - Implement conditional logic: if STAGE_ALL=false, filter STATUS to show only staged files using grep '^[AMDRC]'
  - If STAGE_ALL=true, set STAGED_STATUS equal to STATUS to maintain current --all behavior
  - _Requirements: 1.1, 1.2, 4.3_

- [ ] 2. Update AI prompt generation to use filtered status (Lines 524, 562, 640, 642)

  - Replace $STATUS with $STAGED_STATUS in English DETAILED_MULTILINE_PROMPT (line 524)
  - Replace $STATUS with $STAGED_STATUS in Korean DETAILED_MULTILINE_PROMPT (line 562)
  - Update English SIMPLE_PROMPT to use STAGED_STATUS instead of STATUS (line 640)
  - Update Korean SIMPLE_PROMPT to use STAGED_STATUS instead of STATUS (line 642)
  - _Requirements: 1.1, 1.3, 3.1_

- [ ] 3. Fix change summary calculation (Lines 738-740)

  - Modify ADDED_FILES calculation to use conditional logic based on STAGE_ALL flag
  - Modify MODIFIED_FILES calculation to use conditional logic based on STAGE_ALL flag
  - Modify DELETED_FILES calculation to use conditional logic based on STAGE_ALL flag
  - For STAGE_ALL=false: use STAGED_STATUS with patterns '^A', '^M', '^D'
  - For STAGE_ALL=true: keep current STATUS patterns '^??|^A', '^.M|^M', '^.D|^D'
  - _Requirements: 2.1, 2.2, 4.2_

- [ ] 4. Test and validate the fix
  - Create test scenarios with mixed staged/unstaged files
  - Verify commit messages mention only staged files when using basic aic command
  - Confirm --all flag behavior remains unchanged
  - Test both Korean and English language modes
  - _Requirements: 3.2, 3.3, 4.1_
