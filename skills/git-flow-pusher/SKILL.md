---
name: git-flow-pusher
description: "Commits tested auto-fixes using Conventional Commits guidelines and pushes changes to the remote repository, completing the loop."
---

# Git Flow Pusher Skill

This skill automates git commits and pushes when changes have been successfully corrected and tested.

## Responsibilities
- Review the status results from `fix_results.json`.
- Formulate a conventional commit message describing the fixes applied.
- Run git commands to commit and push changes.
- Return success state to prompt pipeline re-evaluation.

## Inputs
- `fix_results.json` (from `auto-fixer`)
- `commit_scope` (string, optional: the scope for conventional commit, e.g. `auth`, `ui`).

## Outputs
- `push_status.json`:
  ```json
  {
    "timestamp": "2026-06-13T14:10:00Z",
    "git_commit_hash": "a1b2c3d4...",
    "remote_push_success": true,
    "trigger_repeat": true
  }
  ```

## Instructions and Guidelines
1. Load `fix_results.json` from the scratch directory. Verify that tests passed.
2. Draft a commit message conforming to Conventional Commits (e.g., `fix(scope): auto-resolve security vulnerabilities and styling issues`).
3. Execute `git add .` followed by `git commit -m "..."` and `git push origin <branch>`.
4. Save the push status report to `push_status.json`.
5. Signal that the pipeline has completed one full iteration and is ready to watch/repeat (`trigger_repeat: true`).
