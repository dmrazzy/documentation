# VS Code Copilot Best Practices Guide

**Date**: 24 January 2026  
**Topic**: Best practices for creating Skills, Rules, Templates, Prompts, and other AI-consumable artifacts

---

## Current State Analysis

Your workspace has:
- ✅ **Agent**: `ks-tw.agent.md` (custom agent)
- ✅ **Skills**: 3 skills (release-notes, information-architect, integration-guide)
- ✅ **Templates**: 11 templates (release notes, features page, overview page, etc.)
- ✅ **Rules**: 1 rule file (release-notes validation)
- ✅ **Prompts**: 2 prompt files
- ✅ **Copilot Instructions**: Root-level instructions file

---

## Best Practices by Type

### 1. Skills (`.github/skills/{skill-name}/SKILL.md`)

**Structure Pattern:**
```markdown
---
title: [Skill Name]
description: [One-line purpose]
version: 1.0.0
tags: [documentation, validation, creation]
---

# [Skill Name] Skill

## Purpose
[What this skill helps accomplish]

## When to Use
- [Use case 1]
- [Use case 2]

## Prerequisites
- [Required knowledge/files]

## Instructions
1. [Step-by-step process]

## Resources
- Template: `.github/templates/{template-name}.md`
- Rules: `.github/skills/{skill-name}/rules.md`
- Examples: `.github/skills/{skill-name}/examples/`

## Success Criteria
- [ ] Checklist item 1
- [ ] Checklist item 2
```

**Best Practices:**
- Keep each skill focused on ONE specific task
- Include clear success criteria
- Reference templates and rules within the skill folder
- Add examples (good/bad) for clarity
- Version your skills to track changes

---

### 2. Rules (`.github/rules/{rule-name}/`)

**Structure Pattern:**
```markdown
---
type: validation-rules
applies-to: [release-notes, api-docs]
enforcement: strict|recommended
---

# [Rule Category] Rules

## Mandatory Requirements
- [ ] Requirement 1 (MUST)
- [ ] Requirement 2 (MUST)

## Recommended Guidelines
- [ ] Guideline 1 (SHOULD)
- [ ] Guideline 2 (SHOULD)

## Prohibited Patterns
❌ **Never do this**: [Pattern + reason]
✅ **Instead do this**: [Alternative]

## Validation Checklist
### Structure
- [ ] Item has correct format
- [ ] All sections present

### Content Quality
- [ ] Clear and concise
- [ ] No jargon

### Technical Accuracy
- [ ] Links work
- [ ] Code examples tested

## Examples
### ✅ Good Example
[Show correct implementation]

### ❌ Bad Example
[Show what to avoid]
```

**Best Practices:**
- Separate MUST (mandatory) from SHOULD (recommended)
- Use emojis for quick visual scanning (✅ ❌ ⚠️)
- Include both positive and negative examples
- Make rules enforceable (specific, measurable)
- Group related rules by category

---

### 3. Templates (`.github/templates/{template-name}.md`)

**Structure Pattern (AI-Consumable):**
```markdown
---
templateType: [release-notes|integration-guide|api-spec]
version: 1.0.0
audience: [developers|technical-writers|architects]
aiConsumable: true
lastUpdated: YYYY-MM-DD
---

# [Template Name]

## Metadata Table
| Property | Value |
|----------|-------|
| Template Type | [Type] |
| Use Case | [When to use] |
| Related Skill | [Link to skill] |

## Purpose
[What this template creates]

## Instructions for AI Agents
1. Replace `[REQUIRED]` placeholders with actual content
2. Remove `[optional]` sections if not needed
3. Follow structure exactly as shown
4. Validate against: `.github/rules/{rule-name}.md`

## Variables
- `{{version}}` - Version number (X.Y.Z)
- `{{date}}` - Release date (YYYY-MM-DD)
- `{{component}}` - Component name

## Examples
<details>
<summary>Example 1: Patch Release</summary>

[Full example here]

</details>

<!-- TEMPLATE CONTENT START -->

# [REQUIRED: Document Title]

[Your structured content here with clear placeholders]

<!-- TEMPLATE CONTENT END -->

## Validation Checklist
- [ ] All [REQUIRED] fields filled
- [ ] No placeholder text remaining
- [ ] Follows rules in [rules file]
```

**Best Practices:**
- Add YAML frontmatter for machine-readability
- Use clear markers (`<!-- START -->` / `<!-- END -->`)
- Distinguish `[REQUIRED]` from `[optional]`
- Include validation checklist at end
- Collapse examples in `<details>` tags for brevity
- Add variable definitions if using placeholders

---

### 4. Copilot Instructions (`.github/copilot-instructions.md`)

**Structure Pattern:**
```markdown
# [Project Name] — Copilot Instructions

## Project Context
This repository contains [description].

## Role Definition
You are a [role] working on [project]. You have expertise in:
- [Domain 1]
- [Domain 2]

## Documentation Principles
When working in this repository, follow these principles:
1. [Principle 1]
2. [Principle 2]

## File Organization
- `docs/` - [Purpose]
- `.github/skills/` - [Purpose]
- `.github/templates/` - [Purpose]

## Quality Standards
### Writing Style
- Use [tone: formal/conversational]
- Target audience: [audience]
- Avoid: [things to avoid]

### Technical Standards
- Follow Information Architecture v2.0
- Use semantic versioning
- Include YAML frontmatter

## Common Tasks
### Task: Create Release Notes
1. Load template: `.github/templates/release-notes.md`
2. Apply rules: `.github/rules/release-notes-rules.md`
3. Validate with skill: `.github/skills/release-notes/`

## References
- [Link to style guide]
- [Link to contribution guidelines]

## Version History
- v1.0.0 (YYYY-MM-DD): Initial instructions
```

**Best Practices:**
- Define clear role and domain expertise
- Reference specific files/folders
- Include common workflows
- Keep concise (aim for 100-200 lines)
- Update as project evolves

---

### 5. Prompts (`.github/prompts/{prompt-name}.prompt.md`)

**Structure Pattern:**
```markdown
---
type: prompt
purpose: [specific-task]
audience: [who uses this]
---

# [Prompt Name]

## Context
[Background information needed]

## Objective
[What you want to achieve]

## Input Data
- File: [path to file]
- Reference: [documentation link]
- Previous work: [context]

## Instructions
[Detailed step-by-step instructions]

## Output Format
[Expected format of the result]

## Constraints
- [Limitation 1]
- [Limitation 2]

## Examples
### Input
[Show sample input]

### Expected Output
[Show expected result]
```

**Best Practices:**
- Make prompts reusable and parameterized
- Include clear input/output specifications
- Add constraints to guide AI behavior
- Version control prompts like code

---

## Additional Features You Can Create

### 6. Snippets (`.github/snippets/`)
Code/content snippets for common patterns:
```markdown
<!-- .github/snippets/api-endpoint.md -->
## {endpoint-name}

**Endpoint**: `{method} {path}`

**Authentication**: {auth-type}

**Request**:
{json}

**Response**:
{json}
```

### 7. Checklists (`.github/checklists/`)
Reusable verification checklists:
```markdown
# Pre-Release Checklist

## Documentation
- [ ] Release notes created
- [ ] API docs updated
- [ ] Migration guide written

## Technical
- [ ] All tests passing
- [ ] Security scan complete
```

### 8. Workflows (`.github/workflows/` - for automation)
While primarily for GitHub Actions, you can create AI-friendly workflow documentation:
```markdown
# Documentation Review Workflow

1. Author creates PR
2. Copilot validates against templates
3. Reviewer checks rules compliance
4. Merge to main
```

### 9. Glossaries (`.github/glossaries/`)
Domain-specific term definitions for Copilot context:
```markdown
# MOSIP Glossary for AI Agents

## Core Terms
- **UIN**: Unique Identification Number
- **CRVS**: Civil Registration and Vital Statistics
```

### 10. Decision Trees (`.github/decision-trees/`)
Help AI make contextual decisions:
```markdown
# Which Template to Use?

Is it a release? → Use release-notes.md
Is it an integration? → Use integration-guide.md
Is it an API? → Use api-spec.md
```

### 11. Context Files (`.github/context/`)
Background knowledge files for specific domains:
```markdown
# MOSIP Architecture Context

[Detailed architecture explanation for AI to understand]
```

---

## Recommended Folder Structure

```
.github/
├── agents/
│   └── *.agent.md
├── copilot-instructions.md      # Root-level instructions
├── skills/
│   └── {skill-name}/
│       ├── SKILL.md              # Main skill (MUST be uppercase)
│       ├── rules.md              # Skill-specific rules
│       ├── template.md           # Skill-specific template
│       └── examples/
│           ├── good.md
│           └── bad.md
├── templates/                    # Shared templates
│   └── {template-name}.md
├── rules/                        # Shared rules
│   └── {rule-name}.md
├── prompts/                      # Reusable prompts
│   └── {prompt-name}.prompt.md
├── snippets/                     # Content snippets
│   └── {snippet-name}.md
├── checklists/                   # Verification lists
│   └── {checklist-name}.md
├── glossaries/                   # Term definitions
│   └── {domain}.glossary.md
├── decision-trees/               # Decision logic
│   └── {decision}.tree.md
└── context/                      # Background knowledge
    └── {domain}.context.md
```

---

## Quick Wins for Your Repository

1. **Expand your Skills**: Create skills for:
   - API documentation
   - Troubleshooting guides
   - Architecture diagrams

2. **Add more Rules**: Create rules for:
   - Documentation standards (tone, style, formatting)
   - API naming conventions
   - Security documentation requirements

3. **Create Glossaries**: Build `mosip.glossary.md` with all MOSIP-specific terms

4. **Add Decision Trees**: Help Copilot choose the right template/approach

5. **Create Checklists**: Pre-publication, pre-release, code review checklists

6. **Build Context Files**: MOSIP architecture, eSignet workflows, CRVS integration patterns

---

## Key Naming Conventions

| Type | Location | Filename Convention |
|------|----------|---------------------|
| **Agent** | `.github/agents/` | `{name}.agent.md` |
| **Skill** | `.github/skills/{skill-name}/` | `SKILL.md` (MUST be uppercase) |
| **Template** | `.github/templates/` or inside skill | `{name}-template.md` or `{name}.md` |
| **Rules** | `.github/rules/` or inside skill | `{name}-rules.md` |
| **Prompt** | `.github/prompts/` | `{name}.prompt.md` |
| **Copilot Instructions** | `.github/` | `copilot-instructions.md` |
| **Glossary** | `.github/glossaries/` | `{domain}.glossary.md` |
| **Checklist** | `.github/checklists/` | `{name}-checklist.md` |
| **Context** | `.github/context/` | `{domain}.context.md` |
| **Snippet** | `.github/snippets/` | `{name}.snippet.md` |

---

## Integration Example

**How different components work together:**

```markdown
# User asks: "Create release notes for version 1.2.0"

1. **Copilot Instructions** loads project context
2. **Skill** (release-notes/SKILL.md) provides process
3. **Template** (release-notes.md) structures content
4. **Rules** (release-notes-rules.md) validates output
5. **Glossary** (mosip.glossary.md) ensures correct terminology
6. **Checklist** (pre-release-checklist.md) verifies completeness
```

---

## Quality Standards

### For All Artifacts:
1. **YAML Frontmatter**: Always include metadata
2. **Clear Purpose**: State what and why in first paragraph
3. **Examples**: Show, don't just tell
4. **Versioning**: Track changes over time
5. **Cross-References**: Link related resources

### Validation Questions:
- Can an AI agent understand this without human interpretation?
- Are placeholders clearly marked and documented?
- Is the structure consistent with similar artifacts?
- Does it include both instructions AND examples?
- Can success be objectively measured?

---

## Advanced Patterns

### Pattern 1: Skill Composition
```markdown
# Integration Guide Skill

## Sub-Skills
1. Load: `.github/skills/api-documentation/SKILL.md`
2. Load: `.github/skills/architecture-diagram/SKILL.md`
3. Compose: Create integration guide combining both
```

### Pattern 2: Conditional Templates
```markdown
# Template Selection Logic

IF release_type == "major":
    USE: `.github/templates/major-release.md`
ELIF release_type == "patch":
    USE: `.github/templates/patch-release.md`
```

### Pattern 3: Rule Inheritance
```markdown
# API Documentation Rules

## Base Rules
INCLUDE: `.github/rules/documentation-standards.md`

## API-Specific Rules
[Additional rules here]
```

---

## Maintenance Best Practices

1. **Regular Reviews**: Quarterly review of all artifacts
2. **Version Control**: Track changes with commit messages
3. **Deprecation**: Mark outdated artifacts clearly
4. **Testing**: Validate with real use cases
5. **Documentation**: Update this guide as you learn

---

## Troubleshooting

### Issue: Copilot doesn't recognize my skill
**Solution**: Ensure filename is `SKILL.md` (all uppercase) in `.github/skills/{name}/`

### Issue: Template not being applied correctly
**Solution**: Check for clear START/END markers and validate YAML frontmatter

### Issue: Rules too vague
**Solution**: Make rules specific, measurable, and include examples

### Issue: Context overload
**Solution**: Split large context files into focused, single-purpose documents

---

## Resources

- **VS Code Copilot Documentation**: [Official Docs]
- **GitHub Copilot Extensions**: [Marketplace]
- **Community Examples**: [GitHub Repos]
- **Best Practices**: This document

---

## Version History

- **v1.0.0** (24 January 2026): Initial comprehensive guide created based on MOSIP repository analysis

---

## Contributing

When you discover new patterns or best practices:
1. Document them in this file
2. Update version number
3. Add to version history
4. Share with team

---

**End of Guide**
