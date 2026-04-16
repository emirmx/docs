---
name: docs-pr-review
description: Analyzes all changes in a pull request and generates suggestions for improvements, clarifications, inconsistencies, and structural changes without making any edits. Use when the user asks to review, analyze, audit, or provide feedback on a pull requestumentation, or when they want suggestions before making changes.
user-invocable: true
disable-model-invocation: false
---

Analyze all the changes in the current pull request or branch and return a list of suggestions or questions about any points to clarify, potential inconsistencies, and sections to restructure, add, or remove. Read the whole of each document modified in the pull request to ensure the changes do not make inconsistencies.

When referring to the files, ensure you are clear about the file and lines ahich raise your concerns.

Make no edits.
