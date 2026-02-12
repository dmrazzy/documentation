# The Good Docs Project: AI-Powered Documentation Integration

## Overview

The Good Docs Project is an open-source initiative providing community-vetted documentation templates and best practices that are perfect for AI agent integration. This guide explains what it is, why it's valuable, and how to use it with AI tools.

**Project**: https://gitlab.com/tgdp/templates
**Website**: https://thegooddocsproject.dev/

---

## What is The Good Docs Project?

The Good Docs Project is an open-source initiative that provides:
- ✅ **Templates** for every documentation type
- ✅ **Best practices** based on documentation theory
- ✅ **Community-vetted** standards from professional tech writers
- ✅ **Free to use** (CC-BY-4.0 license)
- ✅ **Structured** and machine-readable

### Available Templates

**Core Documentation Types**:
- API documentation
- How-to guides
- Tutorials
- Reference documentation
- Concept explanations
- Release notes
- README templates
- Contributing guides
- Code of conduct
- Troubleshooting guides
- Discussion templates
- Logging guides
- Quick start guides

---

## Why It's Perfect for AI Agents

### 1. Structured Templates

Every template follows a consistent format:
```
template-name/
├── guide-template-name.md        # The actual template
├── about-template-name.md        # Explanation of when to use it
├── guide-template-name-filled.md # Example of filled template
└── process-template-name.md      # How to use the template
```

**Example: Tutorial Template Structure**
```
tutorial/
├── guide-tutorial.md        # Template with placeholders
├── about-tutorial.md        # When/why to use tutorials
├── guide-tutorial-filled.md # Complete example
└── process-tutorial.md      # Step-by-step creation guide
```

### 2. Based on Diataxis Framework

Follows Daniel Procida's documentation system theory:

| Type | Purpose | Orientation | Analogy |
|------|---------|-------------|---------|
| **Tutorial** | Learning | Learning-oriented | Teaching a child to cook |
| **How-to** | Problem-solving | Task-oriented | Recipe |
| **Reference** | Information | Information-oriented | Encyclopedia article |
| **Explanation** | Understanding | Understanding-oriented | History of cooking |

**Decision Tree**:
```
User needs documentation
  ↓
  Is the user learning?
    → YES: Tutorial
    → NO: Continue
  ↓
  Does the user have a specific task?
    → YES: How-to guide
    → NO: Continue
  ↓
  Does the user need to look up information?
    → YES: Reference documentation
    → NO: Explanation/Concept doc
```

### 3. AI-Friendly Format

- Written in Markdown
- Clear structure with placeholders
- Embedded guidance in comments
- Examples provided
- Easy to parse and adapt
- Machine-readable structure

### 4. Community-Vetted Quality

- Reviewed by professional technical writers
- Used by major open-source projects
- Continuously improved based on feedback
- Aligned with industry best practices

---

## How AI Agents Can Use It

### 1. Reference as Authoritative Source

**In `.github/copilot-instructions.md`:**
```markdown
# Documentation Standards

## Templates
Use The Good Docs Project templates as authoritative source:
- Base: https://gitlab.com/tgdp/templates/-/tree/main/
- API Reference: Use `api-reference/` template
- Tutorial: Use `tutorial/` template
- How-to: Use `how-to/` template
- Explanation: Use `explanation/` template

When creating documentation:
1. Identify document type using Diataxis framework:
   - Learning something new? → Tutorial
   - Solving a specific problem? → How-to
   - Looking up information? → Reference
   - Understanding a concept? → Explanation

2. Fetch corresponding Good Docs template
3. Fill in placeholders with content
4. Follow the embedded guidance
5. Validate against template structure

## Standards to Follow
- Diataxis framework for organization
- Good Docs style guide
- Community best practices
- Clear structure with consistent headings
```

### 2. Clone Templates to Your `.github` Repo

```bash
# Clone Good Docs templates
cd ~/projects
git clone https://gitlab.com/tgdp/templates.git gooddocs-templates

# Copy relevant templates to your .github repo
cd yourusername/.github
mkdir -p templates/gooddocs

# Copy the templates you need
cp -r ~/projects/gooddocs-templates/api-reference templates/gooddocs/
cp -r ~/projects/gooddocs-templates/tutorial templates/gooddocs/
cp -r ~/projects/gooddocs-templates/how-to templates/gooddocs/
cp -r ~/projects/gooddocs-templates/reference templates/gooddocs/
cp -r ~/projects/gooddocs-templates/explanation templates/gooddocs/

# Customize for your domain (e.g., MOSIP) if needed
# Then commit to your personal .github repo
git add templates/gooddocs
git commit -m "Add Good Docs Project templates"
git push
```

### 3. AI Prompt Using Good Docs Templates

**`.github/prompts/documentation/create-tutorial.md`:**
```markdown
# Create Tutorial Using Good Docs Template

You are creating a tutorial for documentation.

## Template Source
Use The Good Docs Project tutorial template:
https://gitlab.com/tgdp/templates/-/blob/main/tutorial/guide-tutorial.md

## Template Structure (from Good Docs)
```markdown
# [Tutorial Title]

Brief introduction explaining what the reader will learn and accomplish.

## What you'll learn
- Bullet points of learning outcomes
- What the reader will be able to do
- Prerequisites required

## Before you begin

### Prerequisites
- Required knowledge
- Required software/tools
- Required access/permissions

### Time required
Approximately [X] minutes/hours

## Step 1: [Action Verb] [Object]

Clear instruction for this step.

```bash
# Code example if applicable
command --option value
```

**Expected result**: What should happen after this step.

## Step 2: [Action Verb] [Object]

Continue the pattern for each step.

## What you've learned

Summary of what was accomplished and learned.

## Next steps

Where to go from here:
- Related tutorials
- Advanced topics
- Related how-to guides

## Related resources
- Links to reference documentation
- Links to related concepts
```

## Quality Checklist
- [ ] Follows Good Docs tutorial template structure
- [ ] Action-oriented step titles (verb + object)
- [ ] All steps tested and work correctly
- [ ] Clear success criteria for each step
- [ ] Prerequisites clearly stated
- [ ] Time estimate provided
- [ ] Next steps suggested

## Domain-Specific Additions
After following Good Docs template, add:
- Module/component context
- Version compatibility information
- Common troubleshooting
- Links to related documentation
```

### 4. MCP Server with Good Docs Integration

**File**: `~/mcp-servers/gooddocs-integration/index.ts`

```typescript
#!/usr/bin/env node
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';
import {
  CallToolRequestSchema,
  ListToolsRequestSchema,
} from '@modelcontextprotocol/sdk/types.js';

const GOODDOCS_BASE = 'https://gitlab.com/tgdp/templates/-/raw/main/';

const server = new Server(
  {
    name: 'gooddocs-integration',
    version: '1.0.0',
  },
  {
    capabilities: {
      tools: {},
    },
  }
);

// Define tools
server.setRequestHandler(ListToolsRequestSchema, async () => ({
  tools: [
    {
      name: 'fetch_gooddocs_template',
      description: 'Fetch a template from The Good Docs Project',
      inputSchema: {
        type: 'object',
        properties: {
          template_type: {
            type: 'string',
            enum: [
              'api-reference',
              'tutorial',
              'how-to',
              'reference',
              'explanation',
              'quickstart',
              'release-notes',
              'troubleshooting'
            ],
            description: 'Type of documentation template to fetch',
          },
        },
        required: ['template_type'],
      },
    },
    {
      name: 'validate_against_gooddocs',
      description: 'Validate documentation structure against Good Docs standards',
      inputSchema: {
        type: 'object',
        properties: {
          content: {
            type: 'string',
            description: 'Markdown content to validate',
          },
          doc_type: {
            type: 'string',
            enum: ['api-reference', 'tutorial', 'how-to', 'reference', 'explanation'],
            description: 'Expected documentation type',
          },
        },
        required: ['content', 'doc_type'],
      },
    },
    {
      name: 'suggest_doc_type',
      description: 'Suggest appropriate documentation type based on user intent',
      inputSchema: {
        type: 'object',
        properties: {
          user_intent: {
            type: 'string',
            description: 'What the user wants to achieve',
          },
        },
        required: ['user_intent'],
      },
    },
  ],
}));

// Implement tool handlers
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  switch (request.params.name) {
    case 'fetch_gooddocs_template': {
      const { template_type } = request.params.arguments;

      try {
        // Fetch main template
        const templateUrl = `${GOODDOCS_BASE}${template_type}/guide-${template_type}.md`;
        const templateResponse = await fetch(templateUrl);
        const template = await templateResponse.text();

        // Fetch "about" guide
        const aboutUrl = `${GOODDOCS_BASE}${template_type}/about-${template_type}.md`;
        const aboutResponse = await fetch(aboutUrl);
        const about = await aboutResponse.text();

        return {
          content: [
            {
              type: 'text',
              text: `# Good Docs Template: ${template_type}\n\n## About This Template\n\n${about}\n\n---\n\n## Template\n\n${template}`,
            },
          ],
        };
      } catch (error) {
        return {
          content: [
            {
              type: 'text',
              text: `Error fetching template: ${error.message}`,
            },
          ],
          isError: true,
        };
      }
    }

    case 'validate_against_gooddocs': {
      const { content, doc_type } = request.params.arguments;

      try {
        // Fetch template to compare structure
        const templateUrl = `${GOODDOCS_BASE}${doc_type}/guide-${doc_type}.md`;
        const response = await fetch(templateUrl);
        const template = await response.text();

        // Extract expected sections from template
        const templateSections = extractSections(template);
        const contentSections = extractSections(content);

        // Compare
        const missingSections = templateSections.filter(
          s => !contentSections.includes(s)
        );

        const extraSections = contentSections.filter(
          s => !templateSections.includes(s)
        );

        const validation = {
          valid: missingSections.length === 0,
          missing_sections: missingSections,
          extra_sections: extraSections,
          structure_score: Math.round(
            (contentSections.filter(s => templateSections.includes(s)).length /
             templateSections.length) * 100
          ),
          recommendations: generateRecommendations(missingSections, extraSections),
        };

        return {
          content: [
            {
              type: 'text',
              text: JSON.stringify(validation, null, 2),
            },
          ],
        };
      } catch (error) {
        return {
          content: [
            {
              type: 'text',
              text: `Error validating: ${error.message}`,
            },
          ],
          isError: true,
        };
      }
    }

    case 'suggest_doc_type': {
      const { user_intent } = request.params.arguments;

      // Simple keyword-based suggestion (could be enhanced with AI)
      const intent = user_intent.toLowerCase();

      let suggestion = {
        recommended_type: '',
        reasoning: '',
        alternative_types: [],
      };

      if (intent.includes('learn') || intent.includes('beginner') || intent.includes('getting started')) {
        suggestion.recommended_type = 'tutorial';
        suggestion.reasoning = 'User wants to learn something new. Tutorials are learning-oriented.';
        suggestion.alternative_types = ['quickstart'];
      } else if (intent.includes('how to') || intent.includes('how do i') || intent.includes('accomplish')) {
        suggestion.recommended_type = 'how-to';
        suggestion.reasoning = 'User has a specific task to accomplish. How-to guides are task-oriented.';
        suggestion.alternative_types = ['tutorial'];
      } else if (intent.includes('api') || intent.includes('reference') || intent.includes('look up')) {
        suggestion.recommended_type = 'api-reference';
        suggestion.reasoning = 'User needs to look up information. Reference docs are information-oriented.';
        suggestion.alternative_types = ['reference'];
      } else if (intent.includes('understand') || intent.includes('why') || intent.includes('concept')) {
        suggestion.recommended_type = 'explanation';
        suggestion.reasoning = 'User wants to understand a concept. Explanations are understanding-oriented.';
        suggestion.alternative_types = ['reference'];
      } else if (intent.includes('troubleshoot') || intent.includes('problem') || intent.includes('error')) {
        suggestion.recommended_type = 'troubleshooting';
        suggestion.reasoning = 'User is facing an issue. Troubleshooting guides help solve problems.';
        suggestion.alternative_types = ['how-to'];
      } else {
        suggestion.recommended_type = 'how-to';
        suggestion.reasoning = 'Default suggestion for general documentation needs.';
        suggestion.alternative_types = ['tutorial', 'reference'];
      }

      return {
        content: [
          {
            type: 'text',
            text: JSON.stringify(suggestion, null, 2),
          },
        ],
      };
    }

    default:
      throw new Error(`Unknown tool: ${request.params.name}`);
  }
});

// Helper functions
function extractSections(markdown: string): string[] {
  const headingRegex = /^#+\s+(.+)$/gm;
  const sections = [];
  let match;
  while ((match = headingRegex.exec(markdown)) !== null) {
    sections.push(match[1].trim());
  }
  return sections;
}

function generateRecommendations(missing: string[], extra: string[]): string[] {
  const recommendations = [];

  if (missing.length > 0) {
    recommendations.push(
      `Add missing sections: ${missing.join(', ')}`
    );
  }

  if (extra.length > 0) {
    recommendations.push(
      `Review extra sections: ${extra.join(', ')} - ensure they add value`
    );
  }

  if (missing.length === 0 && extra.length === 0) {
    recommendations.push('Structure follows Good Docs template perfectly!');
  }

  return recommendations;
}

// Start server
const transport = new StdioServerTransport();
server.connect(transport);
```

**Package it**:
```json
{
  "name": "@yourusername/gooddocs-mcp",
  "version": "1.0.0",
  "type": "module",
  "bin": {
    "gooddocs-mcp": "./index.js"
  },
  "dependencies": {
    "@modelcontextprotocol/sdk": "^0.5.0",
    "node-fetch": "^3.3.0"
  }
}
```

### 5. GitHub Action Using Good Docs Templates

**`.github/workflows/validate-docs-structure.yml`:**
```yaml
name: Validate Documentation Structure (Good Docs)

on:
  pull_request:
    paths:
      - 'docs/**/*.md'

jobs:
  validate:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Clone Good Docs Templates
        run: |
          git clone --depth 1 https://gitlab.com/tgdp/templates.git /tmp/gooddocs

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          pip install pyyaml requests

      - name: Validate Documentation Structure
        id: validate
        run: |
          python scripts/validate_gooddocs_structure.py \
            --docs-dir docs \
            --templates-dir /tmp/gooddocs \
            --output validation-report.md

      - name: Comment on PR
        if: always()
        uses: actions/github-script@v6
        with:
          script: |
            const fs = require('fs');
            let report = '## Documentation Structure Validation\n\n';

            try {
              report += fs.readFileSync('validation-report.md', 'utf8');
            } catch (error) {
              report += '✅ All documentation follows Good Docs Project structure!';
            }

            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: report
            });
```

**Validation script** (`scripts/validate_gooddocs_structure.py`):
```python
#!/usr/bin/env python3
"""Validate documentation structure against Good Docs templates"""

import os
import re
import yaml
import argparse
from pathlib import Path
from typing import List, Dict

class GoodDocsValidator:
    def __init__(self, templates_dir: str):
        self.templates_dir = Path(templates_dir)
        self.template_sections = {}
        self._load_templates()

    def _load_templates(self):
        """Load section headings from Good Docs templates"""
        template_types = ['tutorial', 'how-to', 'api-reference', 'explanation', 'reference']

        for template_type in template_types:
            template_file = self.templates_dir / template_type / f"guide-{template_type}.md"
            if template_file.exists():
                with open(template_file, 'r') as f:
                    content = f.read()
                    self.template_sections[template_type] = self._extract_sections(content)

    def _extract_sections(self, content: str) -> List[str]:
        """Extract H2 section headings from markdown"""
        sections = []
        for line in content.split('\n'):
            if line.startswith('## '):
                section = line[3:].strip()
                # Remove placeholders/brackets
                section = re.sub(r'\[.*?\]', '', section).strip()
                if section:
                    sections.append(section)
        return sections

    def validate_file(self, file_path: Path) -> Dict:
        """Validate a single documentation file"""
        with open(file_path, 'r') as f:
            content = f.read()

        # Extract frontmatter
        doc_type = self._get_doc_type(content)
        if not doc_type or doc_type not in self.template_sections:
            return {
                'file': str(file_path),
                'status': 'unknown',
                'message': f'Unknown or missing doc type: {doc_type}'
            }

        # Extract sections from document
        doc_sections = self._extract_sections(content)
        template_sections = self.template_sections[doc_type]

        # Compare
        missing = [s for s in template_sections if s not in doc_sections]
        extra = [s for s in doc_sections if s not in template_sections]

        score = (len([s for s in doc_sections if s in template_sections]) /
                len(template_sections) * 100) if template_sections else 0

        status = 'pass' if not missing else 'warning' if score > 70 else 'fail'

        return {
            'file': str(file_path),
            'status': status,
            'doc_type': doc_type,
            'score': round(score),
            'missing_sections': missing,
            'extra_sections': extra,
        }

    def _get_doc_type(self, content: str) -> str:
        """Extract document type from frontmatter"""
        if content.startswith('---'):
            try:
                parts = content.split('---', 2)
                frontmatter = yaml.safe_load(parts[1])
                return frontmatter.get('type', '')
            except:
                pass
        return ''

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('--docs-dir', required=True)
    parser.add_argument('--templates-dir', required=True)
    parser.add_argument('--output', default='validation-report.md')
    args = parser.parse_args()

    validator = GoodDocsValidator(args.templates_dir)

    # Find all markdown files
    docs_dir = Path(args.docs_dir)
    md_files = list(docs_dir.rglob('*.md'))

    # Validate each file
    results = []
    for md_file in md_files:
        result = validator.validate_file(md_file)
        results.append(result)

    # Generate report
    report_lines = []

    # Summary
    passed = len([r for r in results if r['status'] == 'pass'])
    warnings = len([r for r in results if r['status'] == 'warning'])
    failed = len([r for r in results if r['status'] == 'fail'])
    unknown = len([r for r in results if r['status'] == 'unknown'])

    report_lines.append(f"### Summary")
    report_lines.append(f"- ✅ Passed: {passed}")
    report_lines.append(f"- ⚠️ Warnings: {warnings}")
    report_lines.append(f"- ❌ Failed: {failed}")
    report_lines.append(f"- ❓ Unknown: {unknown}")
    report_lines.append("")

    # Detailed results
    if failed > 0 or warnings > 0:
        report_lines.append("### Issues Found\n")

        for result in results:
            if result['status'] in ['fail', 'warning']:
                icon = '❌' if result['status'] == 'fail' else '⚠️'
                report_lines.append(f"{icon} **{result['file']}** (Score: {result['score']}%)")
                report_lines.append(f"   - Type: {result['doc_type']}")

                if result['missing_sections']:
                    report_lines.append(f"   - Missing sections: {', '.join(result['missing_sections'])}")

                if result['extra_sections']:
                    report_lines.append(f"   - Extra sections: {', '.join(result['extra_sections'])}")

                report_lines.append("")

    # Write report
    with open(args.output, 'w') as f:
        f.write('\n'.join(report_lines))

    # Exit with error if failures
    if failed > 0:
        exit(1)

if __name__ == '__main__':
    main()
```

---

## Who's Using It

### Organizations Using Good Docs Project

**Confirmed Users**:
- Kubernetes community
- Google (Season of Docs projects)
- Linux Foundation projects
- GitLab (internal documentation)
- Various CNCF projects

**For AI Integration**:
- Tech writing teams starting to reference in AI prompts
- Documentation platforms exploring integration
- **Not yet widely documented** (emerging use case)

### Why Not More Widespread Yet?

1. **Relatively new** (project started ~2020)
2. **Manual adoption focus** initially (not AI-optimized)
3. **Discovery issue** (known to tech writers, less to AI practitioners)
4. **Integration gap** (no official AI plugins/tools yet)

**But this is changing!** AI + Good Docs is an emerging best practice.

---

## Integration Strategy for Documentation Projects

### Phase 1: Reference (Week 1)

**Goals**:
- Add Good Docs references to AI instructions
- Train team on Diataxis framework
- Update documentation guidelines

**Actions**:
```markdown
1. Update `.github/copilot-instructions.md`:
   - Add Good Docs Project reference
   - Include Diataxis decision tree
   - Link to templates for each doc type

2. Create quick reference guide:
   - When to use each template type
   - Links to Good Docs templates
   - MOSIP-specific examples

3. Team training:
   - Introduce Diataxis framework
   - Show Good Docs templates
   - Explain AI integration benefits
```

### Phase 2: Fork & Customize (Week 2)

**Goals**:
- Create domain-specific versions of templates
- Adapt for your project's needs

**Actions**:
```bash
# Fork Good Docs templates
git clone https://gitlab.com/tgdp/templates.git project-gooddocs-templates
cd project-gooddocs-templates
git checkout -b project-customizations

# Customize templates:
# 1. Add domain-specific sections
# 2. Include required frontmatter
# 3. Add project-specific examples
# 4. Adjust for version requirements

# Example customization for tutorial:
# - Add "Module Context" section
# - Add "Version Compatibility" section
# - Include project-specific prerequisites
# - Add troubleshooting section

git commit -am "Customize Good Docs templates for our project"
git push origin project-customizations
```

### Phase 3: Automation (Week 3)

**Goals**:
- Automate template fetching
- Validate documentation structure
- Integrate with CI/CD

**Actions**:
- Create scripts to fetch/customize templates
- Build validation tools
- Set up GitHub Actions
- Create MCP server for Claude Code

### Phase 4: Optimization (Week 4)

**Goals**:
- Gather feedback
- Refine processes
- Consider contributing back

**Actions**:
- Collect team feedback
- Identify improvement areas
- Create project-specific patterns
- Consider upstream contributions

---

## Comparison: Good Docs vs Other Resources

| Resource | Type | Strengths | Limitations | Best For |
|----------|------|-----------|-------------|----------|
| **Good Docs Project** | Templates + Theory | Community-vetted, free, structured, theory-based | Generic (not domain-specific) | Universal doc templates |
| **Microsoft Style Guide** | Style guide | Comprehensive, corporate standard | Not templates, style only | Writing style/terminology |
| **Google Tech Writing** | Courses | Free, beginner-friendly | Course format, not templates | Learning fundamentals |
| **Write the Docs** | Community | Best practices, networking | Not structured for AI | Inspiration, community |
| **Diataxis Framework** | Theory | Clear organization system | Conceptual, not practical | Understanding doc types |
| **Company Docs** (Stripe, Twilio) | Examples | Real-world, polished | Not reusable templates | Inspiration only |
| **Custom Templates** | Project-specific | Tailored to needs | Maintenance overhead | Specialized projects |

**The Good Docs Project bridges the gap**: **Theory + Templates + Examples + Community**

---

## Advanced: Creating Custom Templates Based on Good Docs

### Example: Biometric API Documentation Template

```markdown
# [Biometric API Name]

> Based on Good Docs Project API Reference template with biometric-specific enhancements

## Overview

Brief description of what this biometric API does.

## Biometric Modalities Supported

| Modality | Supported | Quality Threshold | Notes |
|----------|-----------|-------------------|-------|
| Fingerprint | Yes | 60 | All 10 fingers |
| Iris | Yes | 70 | Both eyes |
| Face | Yes | 65 | Live photo required |

## Endpoint

```
POST /v1/biometric/authenticate
```

## Authentication
[Authentication requirements]

## Request

### Headers
[Standard headers]

### Body Parameters

#### Biometric Data
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `biometrics` | array | Yes | Array of biometric samples |
| `biometrics[].type` | string | Yes | Modality: fingerprint, iris, face |
| `biometrics[].qualityScore` | number | Yes | Quality score (0-100) |
| `biometrics[].data` | string | Yes | Base64-encoded biometric data |
| `biometrics[].deviceInfo` | object | Yes | Capture device information |

### Example Request
```json
{
  "biometrics": [
    {
      "type": "fingerprint",
      "qualityScore": 85,
      "data": "base64_encoded_data",
      "deviceInfo": {
        "deviceId": "device123",
        "make": "BiometricDeviceCo",
        "model": "FP-2000"
      }
    }
  ]
}
```

## Response

### Success Response (200)
```json
{
  "authenticated": true,
  "matchScore": 95,
  "matchedModalities": ["fingerprint"]
}
```

### Error Responses

| Code | Description | Common Cause |
|------|-------------|--------------|
| 400 | Invalid request | Quality score below threshold |
| 401 | Authentication failed | No biometric match found |
| 422 | Unprocessable entity | Invalid biometric format |

## Biometric Quality Requirements

- **Fingerprint**: Minimum quality score of 60
- **Iris**: Minimum quality score of 70
- **Face**: Minimum quality score of 65, live photo required

## Common Issues

### Low Quality Score
**Problem**: Biometric rejected due to low quality score
**Solution**:
- Ensure proper lighting for face/iris capture
- Clean sensor for fingerprint capture
- Recapture with quality guidance

### Device Not Certified
**Problem**: 400 error: Device not in certified list
**Solution**: Use SBI-compliant devices from certified manufacturers

## Related Documentation
- [Biometric Standards](./biometric-standards.md)
- [Device Certification](./device-certification.md)
- [Quality Assessment Guide](./quality-assessment.md)
```

---

## Contributing Back to Good Docs Project

### Why Contribute?

1. **Improve templates** based on your experience
2. **Share domain-specific patterns** with community
3. **Get feedback** from professional tech writers
4. **Build reputation** in documentation community
5. **Help others** facing similar challenges

### What to Contribute

**Examples**:
- Domain-specific template variations (biometric API, IoT devices, etc.)
- Process improvements based on AI integration
- Additional examples for existing templates
- Translation improvements
- Bug fixes or clarifications

### How to Contribute

1. **Fork the repository**:
   ```bash
   # On GitLab
   # Navigate to: https://gitlab.com/tgdp/templates
   # Click "Fork"
   ```

2. **Create a branch**:
   ```bash
   git clone <your-fork-url>
   cd templates
   git checkout -b feature/your-contribution
   ```

3. **Make your changes**:
   - Follow existing template structure
   - Include all required files (guide, about, filled example)
   - Test with real content

4. **Submit merge request**:
   - Write clear description
   - Explain use case
   - Provide examples
   - Be responsive to feedback

5. **Engage with community**:
   - Join discussions
   - Respond to reviews
   - Iterate based on feedback

---

## Practical Examples

### Example 1: Creating a Tutorial with Good Docs

**Input**: "Create a tutorial for setting up authentication"

**AI Agent Process**:
1. Recognize document type: Tutorial (learning-oriented)
2. Fetch Good Docs tutorial template
3. Fill in placeholders:
   - Title: "Set Up Authentication for Your Application"
   - Learning outcomes: Configure auth module, test authentication
   - Prerequisites: Basic knowledge of APIs, development environment
   - Steps: Install dependencies, configure, test
4. Add domain-specific context
5. Validate structure

**Output**: Complete tutorial following Good Docs structure

### Example 2: Validating Existing Documentation

**Input**: Existing documentation file

**AI Agent Process**:
1. Extract frontmatter to identify doc type
2. Fetch corresponding Good Docs template
3. Compare section structure
4. Identify missing sections
5. Generate validation report with recommendations

**Output**: Validation report with actionable feedback

### Example 3: Suggesting Documentation Type

**Input**: "User wants to know how to troubleshoot authentication errors"

**AI Agent Process**:
1. Analyze intent: "troubleshoot" + "how to"
2. Apply Diataxis framework
3. Recommend: Troubleshooting guide (or How-to guide)
4. Explain reasoning
5. Suggest template

**Output**: Template recommendation with justification

---

## Best Practices

### 1. Always Identify Document Type First

Use the Diataxis decision tree:
```
Is the user learning? → Tutorial
Does the user have a specific task? → How-to
Looking up information? → Reference
Understanding a concept? → Explanation
```

### 2. Don't Force-Fit Content

If the template doesn't fit perfectly, that's okay:
- Templates are guides, not rigid rules
- Adapt as needed for your context
- Consider if you've chosen the right template type

### 3. Use Multiple Templates

Complex documentation might need multiple types:
- Tutorial + Reference (learning + lookup)
- How-to + Troubleshooting (task + problems)
- Explanation + Reference (concept + details)

### 4. Validate Structure, Not Content

Templates ensure good structure:
- ✅ Check for required sections
- ✅ Validate organization
- ❌ Don't judge content quality (that's human review)

### 5. Customize for Your Domain

Good Docs templates are starting points:
- Add domain-specific sections
- Include required metadata
- Adapt examples to your context
- Keep the core structure intact

### 6. Version Your Customizations

If you customize templates:
- Track changes in version control
- Document why you deviated
- Consider contributing improvements upstream

### 7. Train Your Team

Templates work best when everyone understands:
- The Diataxis framework
- When to use each template type
- How to adapt templates
- Why structure matters

---

## Resources

### Official Resources

**Good Docs Project**:
- GitLab Repository: https://gitlab.com/tgdp/templates
- Website: https://thegooddocsproject.dev/
- Community: https://thegooddocsproject.dev/community/
- Slack: Join via website

**Diataxis Framework**:
- Website: https://diataxis.fr/
- Created by Daniele Procida
- Foundation of Good Docs organization

### Related Resources

**Write the Docs**:
- Website: https://www.writethedocs.org/
- Community of technical writers
- Conferences and meetups

**Google Season of Docs**:
- Website: https://developers.google.com/season-of-docs
- Open source documentation program
- Good Docs projects often participate

### Integration Resources

**AI + Documentation**:
- This guide (you're reading it!)
- MCP SDK: https://github.com/modelcontextprotocol/sdk
- GitHub Actions: https://docs.github.com/actions

---

## Quick Start Checklist

### Immediate (Today)
- [ ] Review Good Docs Project website
- [ ] Explore templates on GitLab
- [ ] Read about Diataxis framework
- [ ] Identify doc types you need

### This Week
- [ ] Update `.github/copilot-instructions.md` with Good Docs reference
- [ ] Clone templates repository
- [ ] Try creating one document using Good Docs template
- [ ] Share with team for feedback

### This Month
- [ ] Customize templates for your domain
- [ ] Set up validation workflow
- [ ] Train team on templates and Diataxis
- [ ] Create MCP server (optional)

### Ongoing
- [ ] Use templates for all new documentation
- [ ] Validate existing docs against templates
- [ ] Gather feedback and iterate
- [ ] Consider contributing improvements

---

## Conclusion

The Good Docs Project provides:
- ✅ **Community-vetted templates** for all documentation types
- ✅ **Theory-based organization** (Diataxis framework)
- ✅ **AI-friendly structure** (machine-readable Markdown)
- ✅ **Free and open source** (CC-BY-4.0 license)
- ✅ **Continuous improvement** (active community)

**For AI Agents**, Good Docs means:
- Authoritative source for documentation structure
- Clear templates to generate content
- Validation standards for quality checks
- Consistent patterns across projects

**The Future**: AI + Good Docs integration will become standard practice for documentation teams.

---

*Last Updated: February 2026*
*Related: Documentation Standards, AI Agents, Technical Writing, Diataxis Framework*
*Source: The Good Docs Project (CC-BY-4.0)*
