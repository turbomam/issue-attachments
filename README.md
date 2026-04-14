# issue-attachments

Sidecar repo for verbose GitHub issue/PR content that doesn't belong in the issue body.

## What goes here

- Log output, stack traces, linter dumps
- Options-analysis tables (>3 alternatives)
- Schema comparison matrices
- Raw tool output (link-checker results, test logs, etc.)
- Any content that would push an issue body past ~1500 characters

## How to use

1. Create a file: `{repo}/{issue-number}-{short-description}.md`
2. Link from the issue body: `Details: https://github.com/turbomam/issue-attachments/blob/main/{path}`

## Naming convention

```
nmdc-schema/2523-has-unit-analysis.md
nmdc-ai-eval/24-sub-issue-tracking.md
external-metadata-awareness/275-collection-comparison.md
```

## Why

NMDC team retro (2026-04-14): "stop dumping large amounts of LLM-generated text into issues." This repo keeps issue bodies concise while preserving the detailed analysis for anyone who wants it.
