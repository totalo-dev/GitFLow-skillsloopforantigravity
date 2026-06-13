---
name: code-watcher
description: "Monitors local workspace directories for code changes, compiling list of modified files and git diff info to kick off the CI/CD pipeline."
---

# Code Watcher Skill

This skill is responsible for identifying changes in the active workspace and preparing the dataset for analysis.

## Responsibilities
- Monitor workspace folder or check current Git state to find uncommitted/modified files.
- Aggregate diff information and metadata for changed files.
- Produce a structured output payload.

## Inputs
- `workspace_path` (string): Absolute path to the repository/project.
- `ignore_patterns` (array of strings, optional): Custom glob/regex patterns of directories/files to ignore.

## Outputs
- `modified_files.json`:
  ```json
  {
    "workspace_path": "C:/...",
    "timestamp": "2026-06-13T14:00:00Z",
    "modified_files": [
      {
        "filepath": "C:/.../src/index.js",
        "status": "modified",
        "diff": "@@ -1,3 +1,4 @@..."
      }
    ]
  }
  ```

## Instructions and Guidelines
1. Execute `git status --porcelain` in the workspace directory to get a fast view of changed files.
2. For each modified file, execute `git diff <file>` to obtain the line changes.
3. Save the resulting payload inside the project's scratch directory as `modified_files.json`.
4. Trigger the next step in the pipeline: `automated-reviewer`.
