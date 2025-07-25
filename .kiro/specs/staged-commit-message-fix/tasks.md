# Implementation Plan

- [ ] 1. Add staged file filtering logic

  - Modify the file status analysis section to create STAGED_STATUS variable
  - Add conditional logic to filter git status output based on STAGE_ALL flag
  - Implement grep pattern to extract only staged files (^[AMDRC])
  - _Requirements: 1.1, 1.2, 4.3_

- [ ] 2. Update AI prompt generation to use filtered status

  - Replace $STATUS with $STAGED_STATUS in both English and Korean Gemini prompts
  - Ensure DETAILED_MULTILINE_PROMPT uses the correct status information
  - Update fallback SIMPLE_PROMPT to use filtered status for staged-only commits
  - _Requirements: 1.1, 1.3, 3.1_

- [ ] 3. Fix change summary calculation

  - Modify ADDED_FILES, MODIFIED_FILES, and DELETED_FILES calculation logic
  - Add conditional logic to count files based on STAGE_ALL flag
  - Use STAGED_STATUS for staged-only commits and STATUS for --all commits
  - Update grep patterns to match staged file status codes correctly
  - _Requirements: 2.1, 2.2, 4.2_

- [ ] 4. Test and validate the fix
  - Create test scenarios with mixed staged/unstaged files
  - Verify commit messages mention only staged files when using basic aic command
  - Confirm --all flag behavior remains unchanged
  - Test both Korean and English language modes
  - _Requirements: 3.2, 3.3, 4.1_
