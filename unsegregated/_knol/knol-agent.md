# Understanding GitHub Copilot: Agents, Skills, Templates, and Rules

## Context
**Date**: 22 January 2026  
**Topic**: Creating custom agents, skills, templates, and validation rules for GitHub Copilot

---

## Q: What are Templates + Rules? Can I define them like custom agents and skills?

### Answer

Yes! **Templates + Rules** are reusable components you can define, similar to how you created custom agents and skills.

## Definitions

- **Templates** = Reusable content structures
- **Rules** = Validation criteria and guidelines

---

## Directory Structure Options

### Option 1: Templates Directory
```
.github/
├── agents/
│   └── editor-release-notes.agent.md
├── skills/
│   └── release-notes/
│       └── SKILL.md
└── templates/
    ├── release-notes-template.md
    ├── integration-guide-template.md
    └── api-spec-template.yaml
```

### Option 2: Rules Directory
```
.github/
├── agents/
├── skills/
└── rules/
    ├── release-notes-rules.md
    ├── documentation-standards.md
    └── api-naming-conventions.md
```

### Option 3: Combined in Skills (Recommended)
```
.github/skills/release-notes/
├── SKILL.md              # Main skill definition
├── template.md           # Release notes template
├── rules.md              # Validation rules
└── examples/
    ├── good-example.md
    └── bad-example.md
```



## How Agents Can Use Templates & Rules

**File**: `.github/agents/editor-release-notes.agent.md`

````markdown
---
name: Release Notes Editor
description: Validates and improves MOSIP release notes
---

# Resources

## Template
Load template from: `.github/templates/release-notes-template.md`

## Validation Rules
Apply rules from: `.github/rules/release-notes-rules.md`

# Process

When reviewing release notes:

1. **Compare against template**
   - Check for missing sections
   - Validate structure

2. **Apply validation rules**
   - Verify version format
   - Check mandatory fields
   - Validate links

3. **Provide feedback**
   - List missing sections
   - Highlight rule violations
   - Suggest improvements
````

---

## Invoking Templates & Rules

### In Copilot Chat
```
@editor-release-notes review this using the standard template and rules
```

### In Skills
**File**: `.github/skills/release-notes/SKILL.md`

````markdown
# Release Notes Validation Skill

## Steps

1. Load template: `@workspace .github/templates/release-notes-template.md`
2. Load rules: `@workspace .github/rules/release-notes-rules.md`
3. Compare user content against template structure
4. Check all rules from rules.md
5. Generate validation report
````

---

## Layered Architecture (Best Practice)

```
┌─────────────────────────────────────┐
│  Agent (Orchestrator)               │
│  - Decision making                  │
│  - User interaction                 │
│  - Context awareness                │
└──────────────┬──────────────────────┘
               │ delegates to
               ↓
┌─────────────────────────────────────┐
│  Skill (Workflow)                   │
│  - Reusable procedures              │
│  - Step-by-step process             │
└──────────────┬──────────────────────┘
               │ uses
               ↓
┌─────────────────────────────────────┐
│  Template (Structure)               │
│  - Content scaffolding              │
│  - Format definitions               │
└─────────────────────────────────────┘
               +
┌─────────────────────────────────────┐
│  Rules (Validation)                 │
│  - Quality criteria                 │
│  - Standards enforcement            │
└──────────────┬──────────────────────┘
               │ references
               ↓
┌─────────────────────────────────────┐
│  Examples                           │
│  - Good examples                    │
│  - Anti-patterns                    │
└─────────────────────────────────────┘
```

---

## Complete Example Structure

```
.github/
├── agents/
│   ├── editor-release-notes.agent.md
│   ├── api-doc-validator.agent.md
│   └── integration-guide-writer.agent.md
│
├── skills/
│   ├── release-notes/
│   │   ├── SKILL.md
│   │   ├── template.md
│   │   ├── rules.md
│   │   └── examples/
│   │       ├── good-example.md
│   │       └── bad-example.md
│   │
│   ├── api-docs/
│   │   ├── SKILL.md
│   │   ├── openapi-template.yaml
│   │   └── rules.md
│   │
│   └── integration-guides/
│       ├── SKILL.md
│       ├── template.md
│       └── rules.md
│
├── templates/
│   ├── release-notes-template.md
│   ├── integration-guide-template.md
│   ├── api-spec-template.yaml
│   └── feature-doc-template.md
│
└── rules/
    ├── release-notes-rules.md
    ├── documentation-standards.md
    ├── api-naming-conventions.md
    └── writing-style-guide.md
```

---

## Key Naming Conventions

| Component | Location | Filename Pattern |
|-----------|----------|------------------|
| **Agent** | `.github/agents/` | `{name}.agent.md` |
| **Skill** | `.github/skills/{skill-name}/` | `SKILL.md` (must be uppercase) |
| **Template** | `.github/templates/` or inside skill folder | `{name}-template.md` |
| **Rules** | `.github/rules/` or inside skill folder | `{name}-rules.md` |

---

## Usage Patterns

### Pattern 1: Direct Reference
```
@agent-name validate this against the release-notes template
```

### Pattern 2: Skill Invocation
```
Use the release-notes skill to check this document
```

### Pattern 3: Explicit Template/Rule Check
```
Compare this against .github/templates/release-notes-template.md
and validate using .github/rules/release-notes-rules.md
```

---

## Implementation Checklist

To implement a complete documentation validation system:

- [ ] Create agent file (`.github/agents/*.agent.md`)
- [ ] Create skill directory (`.github/skills/{skill-name}/`)
- [ ] Define skill workflow (`SKILL.md`)
- [ ] Create template (`template.md`)
- [ ] Define validation rules (`rules.md`)
- [ ] Add good/bad examples (`examples/`)
- [ ] Test agent + skill combination
- [ ] Document usage in team wiki

---

## Related Concepts

### Agents vs Skills vs Templates

| Aspect | Agent | Skill | Template | Rules |
|--------|-------|-------|----------|-------|
| **Purpose** | AI persona | Workflow | Structure | Validation |
| **Scope** | Broad | Specific | Format | Criteria |
| **Reusability** | Medium | High | Very High | Very High |
| **Customization** | Per agent | Per skill | Minimal | Per standard |
| **Invocation** | `@agent-name` | Via agent or chat | Referenced | Referenced |

---

## Next Steps

1. **Create Templates**: Define standard document structures
2. **Define Rules**: Establish validation criteria
3. **Build Skills**: Package templates + rules into workflows
4. **Configure Agents**: Set up agents to use skills
5. **Test**: Validate the complete system
6. **Document**: Create usage guides for team

---

## Resources

- GitHub Copilot Custom Agents Documentation
- VS Code Copilot Extensions Guide
- MOSIP Documentation Standards

---

**Document Status**: Knowledge Base Entry  
**Last Updated**: 22 January 2026  
**Category**: GitHub Copilot Configuration  
**Tags**: agents, skills, templates, rules, automation
