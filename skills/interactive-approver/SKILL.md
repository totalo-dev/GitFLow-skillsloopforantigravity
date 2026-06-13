---
name: interactive-approver
description: "Prompts the developer to interactively approve or reject recommendations, outputting approved fixes for execution."
---

# Interactive Approver Skill

This skill mediates between automated scanners/reviews and actual file mutations, prompting the user for approval.

## Responsibilities
- Parse issues from `review_report.json` and recommendations from `scan_recommendations.json`.
- Present details of the issues and proposed recipes clearly to the developer.
- Solicit explicit user feedback (Approve/Reject/Edit) for each recipe.
- Output a file containing only the approved corrections.

## Inputs
- `review_report.json` (from `automated-reviewer`)
- `scan_recommendations.json` (from `recommendation-scanner`)

## Outputs
- `approved_fixes.json`:
  ```json
  {
    "timestamp": "2026-06-13T14:06:00Z",
    "approved_fixes": [
      {
        "recipe_id": "REC-001",
        "filepath": "C:/.../src/index.js",
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
1. Fetch and load the latest findings from `review_report.json` and `scan_recommendations.json`.
2. Present a summary of issues and recommendations. Use interactive questions (such as a checklist or options dialog) to request approval.
3. If the user approves one or more changes, write them to `approved_fixes.json`.
4. Trigger the next step in the pipeline: `auto-fixer`.
