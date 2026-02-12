# Creating Global AI Resources: Skills, Prompts & Agents

## Overview

This guide explains how to create personal/global AI resources (agents, skills, prompts, instructions) that are available across **all your repositories**, not just one project.

---

## 1. GitHub Copilot: User-Level Instructions

### Create Global Copilot Instructions

**Location**: `~/.github/copilot-instructions.md` (on your local machine)

```markdown
# Global Copilot Instructions for @yourusername

## My Preferences

### Code Style
- Use Python type hints always
- Prefer functional programming patterns
- Write docstrings in Google style

### Documentation
- Always include frontmatter in Markdown
- Use active voice, present tense
- Provide code examples in 3 languages

### Testing
- Write tests using pytest
- Aim for 80%+ coverage
- Include edge cases

## My Common Patterns

### API Documentation Template
[Your reusable template]

### Error Handling Pattern
```python
try:
    result = operation()
except SpecificError as e:
    logger.error(f"Operation failed: {e}")
    raise CustomException("User-friendly message") from e
```

## My Domain Knowledge

### MOSIP Concepts
- UIN vs VID distinction
- Authentication flow patterns
- Common module interactions

---
*These instructions apply to all my repositories.*
```

**How it works**:
- Copilot looks for `~/.github/copilot-instructions.md` on your machine
- Loads it for EVERY repository you work on
- Combines with repo-specific `.github/copilot-instructions.md`

---

## 2. Personal GitHub Repository: Reusable Resources

### Create: `github.com/yourusername/.github` (special repo)

GitHub has a **special repository** convention: creating a repo named `.github` under your username makes it available across all your repos.

**Structure**:
```
yourusername/.github/
├── README.md                  # Your profile README
├── profile/
│   └── README.md             # Shows on your GitHub profile
├── workflow-templates/        # Reusable GitHub Actions
│   ├── ai-doc-review.yml
│   ├── link-checker.yml
│   └── metadata.json
├── prompts/                   # Reusable prompts
│   ├── documentation/
│   ├── code-review/
│   └── analysis/
├── schemas/                   # Validation schemas
│   ├── frontmatter.json
│   └── api-spec.json
├── style-guides/              # Personal standards
│   └── documentation-style.md
└── ai-agents/                 # Agent configurations
    ├── claude-config.yaml
    └── custom-agents/
```

### How to Use Across Repos

**In any repository**, reference resources from your personal `.github` repo:

**Example workflow** (in any repo):
```yaml
# .github/workflows/doc-review.yml
name: Documentation Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      # Use workflow from your personal .github repo
      - uses: yourusername/.github/.github/workflows/ai-doc-review.yml@main
```

**Example in code**:
```python
# scripts/review.py
import requests

# Fetch prompt from your personal repo
prompt_url = "https://raw.githubusercontent.com/yourusername/.github/main/prompts/documentation/create-api-doc.md"
response = requests.get(prompt_url)
prompt_template = response.text
```

---

## 3. MCP Servers: Global Claude Code Skills

### What is MCP?

**Model Context Protocol (MCP)** allows you to create reusable "servers" (tools/skills) that Claude Code can use across ALL repositories.

### Install MCP Servers Globally

**Location**: `~/.config/claude-code/mcp-servers.json` (or similar)

```json
{
  "mcpServers": {
    "my-personal-docs-agent": {
      "command": "npx",
      "args": ["-y", "@yourusername/docs-agent-mcp"]
    },
    "github-tools": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "local-knowledge-base": {
      "command": "python",
      "args": ["/Users/you/ai-tools/knowledge-base-server.py"]
    }
  }
}
```

### Create Your Own MCP Server

**Example**: Personal documentation agent

**File**: `~/ai-tools/docs-agent-mcp/index.ts`

```typescript
#!/usr/bin/env node
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';
import {
  CallToolRequestSchema,
  ListToolsRequestSchema,
} from '@modelcontextprotocol/sdk/types.js';

const server = new Server(
  {
    name: 'personal-docs-agent',
    version: '1.0.0',
  },
  {
    capabilities: {
      tools: {},
    },
  }
);

// Define your personal tools
server.setRequestHandler(ListToolsRequestSchema, async () => ({
  tools: [
    {
      name: 'create_api_doc',
      description: 'Create API documentation using my personal template',
      inputSchema: {
        type: 'object',
        properties: {
          endpoint: { type: 'string' },
          method: { type: 'string' },
          description: { type: 'string' },
        },
        required: ['endpoint', 'method'],
      },
    },
    {
      name: 'check_mosip_standards',
      description: 'Validate documentation against MOSIP standards',
      inputSchema: {
        type: 'object',
        properties: {
          file_path: { type: 'string' },
        },
      },
    },
  ],
}));

// Implement tool handlers
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  switch (request.params.name) {
    case 'create_api_doc': {
      const { endpoint, method, description } = request.params.arguments;

      // Load your personal template
      const template = await loadTemplate('api-doc-template.md');

      // Generate documentation
      const doc = generateApiDoc(template, { endpoint, method, description });

      return {
        content: [
          {
            type: 'text',
            text: doc,
          },
        ],
      };
    }

    case 'check_mosip_standards': {
      const { file_path } = request.params.arguments;

      // Your custom validation logic
      const issues = validateMosipStandards(file_path);

      return {
        content: [
          {
            type: 'text',
            text: JSON.stringify(issues, null, 2),
          },
        ],
      };
    }

    default:
      throw new Error(`Unknown tool: ${request.params.name}`);
  }
});

// Helper functions
async function loadTemplate(name: string): Promise<string> {
  // Load from your personal GitHub repo or local file
  const response = await fetch(
    `https://raw.githubusercontent.com/yourusername/.github/main/templates/${name}`
  );
  return response.text();
}

function generateApiDoc(template: string, data: any): string {
  // Template processing logic
  return template
    .replace('{{endpoint}}', data.endpoint)
    .replace('{{method}}', data.method)
    .replace('{{description}}', data.description);
}

function validateMosipStandards(filePath: string): any[] {
  // Your validation logic
  return [];
}

// Start server
const transport = new StdioServerTransport();
server.connect(transport);
```

**Package it**:
```json
// package.json
{
  "name": "@yourusername/docs-agent-mcp",
  "version": "1.0.0",
  "type": "module",
  "bin": {
    "docs-agent-mcp": "./index.js"
  },
  "dependencies": {
    "@modelcontextprotocol/sdk": "^0.5.0"
  }
}
```

**Publish to npm**:
```bash
npm publish --access public
```

**Now available in ANY repo**:
```
Claude Code automatically loads your MCP server
Your tools appear in the tools list
Works across all repositories
```

---

## 4. Personal Prompt Library as GitHub Repo

### Create: `github.com/yourusername/ai-prompts`

**Structure**:
```
yourusername/ai-prompts/
├── README.md
├── documentation/
│   ├── api-reference.md
│   ├── tutorial.md
│   ├── troubleshooting.md
│   └── release-notes.md
├── code-review/
│   ├── security-review.md
│   ├── performance-review.md
│   └── style-review.md
├── analysis/
│   ├── architecture-analysis.md
│   └── dependency-analysis.md
├── templates/
│   ├── api-doc-template.md
│   └── tutorial-template.md
└── schemas/
    ├── frontmatter-schema.json
    └── api-spec-schema.json
```

### Use in Any Repository

**Method 1: Git submodule**
```bash
# In any repo
git submodule add https://github.com/yourusername/ai-prompts .github/prompts

# Update in .github/copilot-instructions.md
# See prompts in .github/prompts/ for templates
```

**Method 2: Download at runtime**
```python
# scripts/use-prompt.py
import requests

def get_personal_prompt(prompt_name):
    url = f"https://raw.githubusercontent.com/yourusername/ai-prompts/main/{prompt_name}"
    return requests.get(url).text

# Use it
api_doc_prompt = get_personal_prompt('documentation/api-reference.md')
```

**Method 3: GitHub Actions**
```yaml
# .github/workflows/use-personal-prompts.yml
jobs:
  build:
    steps:
      - name: Checkout personal prompts
        uses: actions/checkout@v3
        with:
          repository: yourusername/ai-prompts
          path: .prompts

      - name: Use prompt
        run: |
          python scripts/generate-docs.py --prompt .prompts/documentation/api-reference.md
```

---

## 5. Personal AI Agent Configuration Repository

### Create: `github.com/yourusername/ai-agent-configs`

**Structure**:
```
yourusername/ai-agent-configs/
├── claude/
│   ├── global-config.yaml
│   ├── personal-instructions.md
│   └── mcp-servers.json
├── copilot/
│   └── global-instructions.md
├── custom-agents/
│   ├── docs-agent/
│   ├── code-reviewer/
│   └── link-fixer/
└── install.sh  # Setup script
```

**install.sh**:
```bash
#!/bin/bash
# Install personal AI configurations globally

# Copy Copilot instructions
mkdir -p ~/.github
cp copilot/global-instructions.md ~/.github/copilot-instructions.md

# Copy Claude config
mkdir -p ~/.config/claude-code
cp claude/global-config.yaml ~/.config/claude-code/

# Install MCP servers
cp claude/mcp-servers.json ~/.config/claude-code/

# Install custom agents
for agent in custom-agents/*; do
    cd "$agent"
    npm install
    npm link
    cd -
done

echo "✅ Personal AI configurations installed!"
```

**Use across machines**:
```bash
# On any new machine
git clone https://github.com/yourusername/ai-agent-configs ~/ai-configs
cd ~/ai-configs
./install.sh
```

---

## 6. Environment-Based Configuration

### Create: `~/.ai-config/`

Store personal configurations locally that work everywhere:

```bash
~/.ai-config/
├── prompts/
├── schemas/
├── templates/
├── knowledge-base/
└── config.yaml
```

**config.yaml**:
```yaml
personal:
  name: Your Name
  github_username: yourusername

  repositories:
    prompts: https://github.com/yourusername/ai-prompts
    agents: https://github.com/yourusername/ai-agent-configs

  preferences:
    code_style: google
    doc_format: markdown
    review_depth: thorough

  mcp_servers:
    - name: personal-docs
      path: ~/mcp-servers/docs-agent
    - name: knowledge-base
      path: ~/mcp-servers/knowledge-base

  api_keys:
    openai: ${OPENAI_API_KEY}
    anthropic: ${ANTHROPIC_API_KEY}
```

**Load in scripts**:
```python
import yaml
import os

config_path = os.path.expanduser('~/.ai-config/config.yaml')
with open(config_path) as f:
    config = yaml.safe_load(f)

prompts_repo = config['personal']['repositories']['prompts']
```

---

## 7. npm Package: Shareable AI Tools

### Publish Your Agent as npm Package

**Create**: `@yourusername/ai-docs-toolkit`

```
ai-docs-toolkit/
├── package.json
├── bin/
│   ├── create-api-doc
│   ├── review-docs
│   └── fix-links
├── lib/
│   ├── prompts/
│   ├── schemas/
│   └── templates/
└── README.md
```

**package.json**:
```json
{
  "name": "@yourusername/ai-docs-toolkit",
  "version": "1.0.0",
  "description": "Personal AI documentation tools",
  "bin": {
    "create-api-doc": "./bin/create-api-doc",
    "review-docs": "./bin/review-docs",
    "fix-links": "./bin/fix-links"
  },
  "files": [
    "bin",
    "lib"
  ],
  "repository": {
    "type": "git",
    "url": "https://github.com/yourusername/ai-docs-toolkit"
  }
}
```

**Install globally**:
```bash
npm install -g @yourusername/ai-docs-toolkit
```

**Use in ANY repo**:
```bash
cd any-project/
create-api-doc --endpoint /v1/auth/login --method POST
review-docs --file docs/guides/tutorial.md
fix-links --auto
```

---

## Complete Setup Guide

### Step 1: Create Personal `.github` Repo

```bash
# Create special repo
gh repo create .github --public --description "Personal GitHub configurations"
cd .github

# Add structure
mkdir -p prompts schemas templates workflows
echo "# My Personal GitHub Resources" > README.md

git add . && git commit -m "Initial setup"
git push
```

### Step 2: Create Global Copilot Instructions

```bash
# Create directory
mkdir -p ~/.github

# Create instructions file
cat > ~/.github/copilot-instructions.md << 'EOF'
# Global Instructions for @yourusername

## My Standards
- Always use type hints in Python
- Write docstrings for all functions
- Prefer composition over inheritance

## My Patterns
[Your patterns here]

## Domain Knowledge
[Your domain knowledge]
EOF
```

### Step 3: Create MCP Server (Optional)

```bash
# Create MCP server directory
mkdir -p ~/mcp-servers/personal-toolkit
cd ~/mcp-servers/personal-toolkit

# Initialize
npm init -y
npm install @modelcontextprotocol/sdk

# Create server (see example above)
# Then configure in Claude Code settings
```

### Step 4: Create Prompt Library Repo

```bash
gh repo create ai-prompts --public
cd ai-prompts

mkdir -p documentation code-review analysis
# Add your prompts

git add . && git commit -m "Personal prompt library"
git push
```

### Step 5: Link Everything

**In `~/.github/copilot-instructions.md`:**
```markdown
# My Global AI Setup

## Resources
- Prompt Library: https://github.com/yourusername/ai-prompts
- Shared Configs: https://github.com/yourusername/.github
- MCP Servers: Configured locally

## How to Use
In any repo, I can reference:
- Prompts from ai-prompts repo
- Schemas from .github repo
- Tools from npm packages
```

---

## Comparison: Approaches

| Approach | Scope | Persistence | Shareability | Best For |
|----------|-------|-------------|--------------|----------|
| **`~/.github/copilot-instructions.md`** | Local machine, all repos | Machine-specific | Not shareable | Personal preferences |
| **`username/.github` repo** | All your repos (when referenced) | GitHub-based | Shareable via URL | Reusable workflows, templates |
| **MCP Servers** | Claude Code, all repos | Machine or npm package | Shareable as package | Custom tools/skills |
| **npm package** | Any machine (after install) | Registry-based | Fully shareable | CLI tools, libraries |
| **`~/.ai-config/`** | Local machine | Machine-specific | Not shareable | Local configurations |
| **Prompt library repo** | Reference from anywhere | GitHub-based | Shareable | Reusable prompts |

---

## Recommended Setup for MOSIP Documentation Work

### 1. Create `yourusername/.github` repo with:
```
.github/
├── prompts/
│   └── documentation/
│       ├── mosip-api-doc.md
│       ├── mosip-tutorial.md
│       └── mosip-release-notes.md
├── schemas/
│   └── mosip-frontmatter-schema.json
├── templates/
│   └── mosip-doc-template.md
└── workflows/
    └── mosip-doc-review.yml
```

### 2. Create `~/.github/copilot-instructions.md` with:
```markdown
# Global Copilot Instructions

## MOSIP Domain Knowledge
- UIN: Unique Identification Number (permanent)
- VID: Virtual ID (temporary, revocable)
- IDA: ID Authentication module
- Biometric modalities: Fingerprint, Iris, Face

## Documentation Standards
- Always include YAML frontmatter
- Use active voice, present tense
- Provide code examples in Python, Java, cURL
- Link to related MOSIP modules

## Common Tasks
- Creating API documentation
- Writing tutorials
- Updating release notes
- Fixing broken links

## Personal Preferences
- Prefer REST over SOAP examples
- Include authentication in all API examples
- Add troubleshooting section to guides
```

### 3. Create MCP server for MOSIP-specific tools:
```typescript
// ~/mcp-servers/mosip-docs-agent/index.ts

server.setRequestHandler(ListToolsRequestSchema, async () => ({
  tools: [
    {
      name: 'create_mosip_api_doc',
      description: 'Create API documentation using MOSIP standards',
    },
    {
      name: 'validate_mosip_frontmatter',
      description: 'Validate frontmatter against MOSIP schema',
    },
    {
      name: 'check_mosip_links',
      description: 'Check internal links to MOSIP modules',
    },
    {
      name: 'generate_mosip_glossary',
      description: 'Generate glossary from MOSIP terminology',
    },
  ],
}));
```

### 4. Publish npm package:
```bash
# Create @yourusername/mosip-docs-toolkit
npm init
npm publish

# Now available globally:
npm install -g @yourusername/mosip-docs-toolkit

# Use in any MOSIP project:
mosip-create-api-doc --endpoint /v1/idauth/authenticate
mosip-validate-docs --dir ./docs
mosip-fix-links --auto
```

---

## Access Patterns

### From Any Repository

**Access your prompts:**
```python
import requests

# Method 1: Direct URL
prompt = requests.get(
    "https://raw.githubusercontent.com/yourusername/.github/main/prompts/documentation/api-doc.md"
).text

# Method 2: Git submodule
# git submodule add https://github.com/yourusername/.github .github/shared
# Then read from .github/shared/prompts/
```

**Use your MCP server:**
```
# Automatically loaded by Claude Code
# Just use the tools in your conversation
```

**Use your npm tools:**
```bash
# Installed globally
npx @yourusername/mosip-docs-toolkit create-api-doc
```

**Reference your schemas:**
```python
import requests
import jsonschema

schema_url = "https://raw.githubusercontent.com/yourusername/.github/main/schemas/frontmatter.json"
schema = requests.get(schema_url).json()

# Validate frontmatter
jsonschema.validate(frontmatter, schema)
```

---

## Syncing Across Machines

### Option 1: Dotfiles Repository

Create `yourusername/dotfiles`:
```
dotfiles/
├── .github/
│   └── copilot-instructions.md
├── .ai-config/
│   └── config.yaml
├── mcp-servers/
└── install.sh
```

**Setup on new machine:**
```bash
git clone https://github.com/yourusername/dotfiles ~/dotfiles
cd ~/dotfiles
./install.sh  # Symlinks everything to correct locations
```

### Option 2: Cloud Sync (Dropbox, iCloud)

```bash
# Store in cloud
mkdir ~/Dropbox/ai-configs
ln -s ~/Dropbox/ai-configs ~/.ai-config

# Accessible on all synced machines
```

### Option 3: GitHub Gist

```bash
# Store small configs as Gists
gh gist create ~/.github/copilot-instructions.md --public

# Download on other machines
gh gist clone <gist-id> ~/.github
```

---

## Security Considerations

### API Keys
- **Never commit API keys** to public repos
- Use environment variables: `${OPENAI_API_KEY}`
- Store in `~/.ai-config/secrets.env` (gitignored)

### Sensitive Prompts
- Keep proprietary prompts in **private** repos
- Use GitHub token for private repo access
- Consider self-hosted MCP servers for sensitive tools

### Access Control
- Public `.github` repo: Safe for general templates/prompts
- Private configs repo: For company-specific content
- Local-only: For client work, NDAs

---

## Troubleshooting

### Issue: Global instructions not loading

**Solution:**
```bash
# Verify file location
ls -la ~/.github/copilot-instructions.md

# Check permissions
chmod 644 ~/.github/copilot-instructions.md

# Restart VS Code / Claude Code
```

### Issue: MCP server not appearing

**Solution:**
```bash
# Check MCP config
cat ~/.config/claude-code/mcp-servers.json

# Test server manually
npx @yourusername/docs-agent-mcp

# Check logs
tail -f ~/.config/claude-code/logs/mcp-server.log
```

### Issue: Cannot access personal repo resources

**Solution:**
```bash
# Verify repo is public
gh repo view yourusername/.github

# Test URL directly
curl https://raw.githubusercontent.com/yourusername/.github/main/prompts/test.md

# Check authentication for private repos
gh auth status
```

---

## Best Practices

### 1. Version Your Resources
```bash
# In your .github repo
git tag v1.0.0
git push --tags

# Reference specific versions
uses: yourusername/.github/.github/workflows/doc-review.yml@v1.0.0
```

### 2. Document Your Setup
```markdown
# README.md in yourusername/.github

## Available Resources

### Prompts
- `prompts/documentation/api-doc.md` - API documentation template
- `prompts/code-review/security.md` - Security review checklist

### Schemas
- `schemas/frontmatter.json` - Documentation frontmatter validation

### How to Use
[Instructions for using these resources]
```

### 3. Keep It DRY (Don't Repeat Yourself)
- Single source of truth for prompts
- Reuse schemas across projects
- Share common workflows

### 4. Regular Updates
```bash
# Update your global resources
cd ~/.github/.github  # Your personal .github repo
git pull origin main

# Update MCP servers
npm update -g @yourusername/docs-agent-mcp
```

---

## Example: Complete MOSIP Setup

### 1. Personal `.github` Repo Structure
```
yourusername/.github/
├── prompts/
│   └── mosip/
│       ├── api-doc.md
│       ├── module-guide.md
│       └── release-notes.md
├── schemas/
│   └── mosip-frontmatter.json
├── templates/
│   ├── api-doc-template.md
│   └── module-guide-template.md
└── workflows/
    ├── mosip-doc-review.yml
    └── mosip-link-check.yml
```

### 2. Global Copilot Instructions
**`~/.github/copilot-instructions.md`:**
```markdown
# Global Copilot Instructions for MOSIP Documentation

## Context
I work primarily on MOSIP (Modular Open Source Identity Platform) documentation.

## MOSIP Domain Knowledge
- **Core modules**: Pre-Reg, Registration, IDA, ID Repository, Partner Management
- **Key concepts**: UIN, VID, biometric authentication, packet processing
- **Architecture**: Microservices, event-driven, cloud-native

## Documentation Standards
See: https://github.com/yourusername/.github/tree/main/templates

Always:
1. Include YAML frontmatter (module, audience, version, type)
2. Use active voice, present tense
3. Provide working code examples
4. Link to related MOSIP modules
5. Add troubleshooting section

## Available Resources
- Prompts: https://github.com/yourusername/.github/tree/main/prompts/mosip
- Schemas: https://github.com/yourusername/.github/tree/main/schemas
- Templates: https://github.com/yourusername/.github/tree/main/templates
```

### 3. MCP Server for MOSIP Tools
**Install:**
```bash
npm install -g @yourusername/mosip-docs-mcp
```

**Configure in Claude Code:**
```json
{
  "mcpServers": {
    "mosip-docs": {
      "command": "npx",
      "args": ["-y", "@yourusername/mosip-docs-mcp"]
    }
  }
}
```

### 4. Usage in Any MOSIP Repo
```bash
cd mosip-authentication-docs

# Copilot automatically loads global instructions
# MCP server provides MOSIP-specific tools
# Can reference prompts from personal .github repo

# Create API doc using personal template
gh api repos/yourusername/.github/contents/templates/api-doc-template.md \
  --jq '.content' | base64 -d > api-doc.md

# Or use your CLI tool
npx @yourusername/mosip-docs-toolkit create-api-doc \
  --endpoint /v1/idauth/authenticate \
  --module ida
```

---

## Conclusion

By creating global AI resources, you can:

✅ **Reuse prompts, templates, and tools** across all projects
✅ **Maintain consistency** in documentation style and standards
✅ **Share knowledge** with team members via public repos
✅ **Sync configurations** across multiple machines
✅ **Build custom tools** specific to your domain (MOSIP)
✅ **Scale your documentation** efforts efficiently

## Quick Start Checklist

- [ ] Create `yourusername/.github` GitHub repo
- [ ] Set up `~/.github/copilot-instructions.md`
- [ ] Create personal prompt library repo
- [ ] Build MCP server for custom tools (optional)
- [ ] Publish npm package for CLI tools (optional)
- [ ] Document your setup in README
- [ ] Test in a new repository

---

*Last Updated: February 2026*
*Related: AI Agents, Documentation Automation, Developer Tools, GitHub Configuration*
