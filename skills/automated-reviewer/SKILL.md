---
name: automated-reviewer
description: "Performs static code review of modified files, identifying bugs, code smells, complexity issues, and style violations."
---

# Automated Reviewer Skill

This skill takes a list of modified files and reviews their code changes against quality and architectural guidelines.

## Responsibilities
- Review the diffs and file contents of all modified files.
- Identify logic bugs, syntax problems, performance overheads, and code smells.
- Classify issues by severity (Critical, Major, Minor).
- Produce a structured output report.

## Inputs
- `modified_files.json` (JSON payload from `code-watcher`):
  - List of filepaths and their diffs.

## Outputs
- `review_report.json`:
  ```json
  {
    "timestamp": "2026-06-13T14:02:00Z",
    "summary": {
      "total_issues": 2,
      "critical": 0,
      "major": 1,
      "minor": 1
    },
    "issues": [
      {
        "filepath": "C:/.../src/index.js",
        "line": 42,
        "severity": "major",
        "category": "logic",
        "description": "Possible null pointer reference here if user object is undefined.",
        "snippet": "const name = user.profile.name;"
      }
    ]
  }
  ```

## Instructions and Guidelines
1. Read the `modified_files.json` input from the scratch directory.
2. For each modified file, inspect the lines changed. Use local linters (eslint, flake8, etc.) or LLM evaluation to find flaws.
3. Keep the checks clean: do not mix security scans in this step, focus entirely on quality, bugs, structure, and design patterns.
4. Save the result as `review_report.json` in the scratch directory.
5. Trigger the next step in the pipeline: `recommendation-scanner`.
