---
name: recommendation-scanner
description: "Runs security vulnerability, dependency checks, and performance scans, producing actionable recipes to fix identified issues."
---

# Recommendation Scanner Skill

This skill parses file changes and existing review findings to identify security flaws, dependency issues, or performance issues, recommending specific fixing actions.

## Responsibilities
- Scan modified files for security vulnerabilities (e.g., SQL injection, hardcoded secrets, XSS).
- Scan for dependency issues or performance regressions.
- Generate precise correction instructions ("recipes") for each vulnerability or code review issue.

## Inputs
- `modified_files.json` (from `code-watcher`)
- `review_report.json` (from `automated-reviewer`)

## Outputs
- `scan_recommendations.json`:
  ```json
  {
    "timestamp": "2026-06-13T14:04:00Z",
    "recommendations": [
      {
        "id": "REC-001",
        "filepath": "C:/.../src/index.js",
        "issue_type": "security",
        "description": "Hardcoded API Token detected.",
        "recipe": {
          "type": "replace",
          "target": "const API_KEY = 'secret-token';",
          "replacement": "const API_KEY = process.env.API_KEY;"
        }
      }
    ]
  }
  ```

## Instructions and Guidelines
1. Read `modified_files.json` and `review_report.json` from the scratch directory.
2. Run tools like secret scanners (e.g. gitleaks, detect-secrets) or run pattern checking.
3. Formulate concrete code replacement payloads (recipes) for code review issues and security alerts.
4. Save the results to `scan_recommendations.json` in the scratch directory.
5. Trigger the next step in the pipeline: `interactive-approver`.
