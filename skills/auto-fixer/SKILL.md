---
name: auto-fixer
description: "Applies approved fixes to the codebase, cleans up formatting, and runs tests to guarantee correctness."
---

# Auto Fixer Skill

This skill applies code replacements to files, runs formatting tools, and verifies that the system compiles and passes tests.

## Responsibilities
- Parse and apply code recipes from `approved_fixes.json`.
- Execute auto-formatting tools (e.g., Prettier, Black, ESLint --fix).
- Run project build and test commands (e.g., `npm run test`, `pytest`) to verify corrections.
- Output build and test results.

## Inputs
- `approved_fixes.json` (from `interactive-approver`)
- `test_command` (string, optional: command to run tests, e.g., `npm test`).
- `format_command` (string, optional: command to run formatters).

## Outputs
- `fix_results.json`:
  ```json
  {
    "timestamp": "2026-06-13T14:08:00Z",
    "fixes_applied": [
      {
        "recipe_id": "REC-001",
        "filepath": "C:/.../src/index.js",
        "status": "applied_successfully"
      }
    ],
    "formatting_success": true,
    "tests_passed": true,
    "test_logs": "..."
  }
  ```

## Instructions and Guidelines
1. Load `approved_fixes.json` from the scratch directory.
2. Apply each approved recipe by executing file edit operations (using code replacement tools).
3. Execute formatting tools if applicable to ensure neat styles.
4. Run the project's test command. If tests fail, revert the code or generate a failure log.
5. Save results to `fix_results.json` in the scratch directory.
6. Trigger the next step in the pipeline: `git-flow-pusher`.
