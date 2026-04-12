---
name: release-notes
description: 'Generate and validate release notes using the centralized release-notes manifest, YAML schema, and rendering rules. Use for release documentation, changelog generation, and consumer repos that fetch standards from this public .github repository.'
argument-hint: 'Provide the release version, source inputs, and output target'
---

# Release Notes

## When To Use
- To validate a release notes provided by product team to ensure it meets the standards defined in the manifest and schema.
- Consume shared release-notes standards from another repository using direct file URLs.
- Validate a release-notes YAML payload before converting it to Markdown.

## Canonical Inputs (Remote)

- Manifest:
https://raw.githubusercontent.com/svahsek/.github/main/standards/content-types/release-notes.yaml

- Schema:
https://raw.githubusercontent.com/svahsek/.github/main/templates/release-notes/release-notes-schema.yaml

- Rendering rules:
https://raw.githubusercontent.com/svahsek/.github/main/templates/release-notes/release-notes-rendering.yaml

- Quick reference:
https://raw.githubusercontent.com/svahsek/.github/main/templates/release-notes/agent-quick-reference.md

## Repository Inputs and Extraction

Take the crude content from here: docs/roadmap-and-releases/releases/android-registration-client-v1.3.0.md

## Analyze and classify changes

With the content already available in the release notes, the next step is to analyze the changes and classify them according to the categories defined in the manifest. This involves reading through the release notes, identifying the key changes, and categorizing them based on their nature (e.g., new features, bug fixes, breaking changes).

| Category | What belongs here |
|---|---|
| **✨ New Features** | New capabilities, endpoints, UI, commands, config options |
| **🐛 Bug Fixes** | Defect corrections, crash fixes, incorrect behavior resolved |
| **💥 Breaking Changes** | Removed/renamed APIs, changed signatures, config format changes, dropped support |
| **🔄 Migration Guide** | Step-by-step instructions required due to breaking changes |
| **🔧 Internal / Chores** | Refactors, dependency bumps, CI changes (include briefly or omit) |

## Procedure

1. Read the release-notes manifest to discover the current schema, rendering rules, and examples.
2. Gather source inputs for the release from available content here: 
3. Structure the release data in YAML that matches the schema.
4. Validate required fields, formatting, and conditional rules against the schema.
5. Render Markdown using the rendering rules.
6. Run style checks before publishing.

## Consumer Repo Pattern
- Resolve the schema and rendering files from the manifest instead of hardcoding every path independently.

## Output Expectations

- YAML is the source of truth for automation.
- Markdown is the publishable output.
- If validation fails, report the failing fields and stop before rendering.


## Output
- Revise, classify, recompose and segregate the content as per instructions here in this skills file and the remote guidelines.
- Put the content here: .github/output/output.md
