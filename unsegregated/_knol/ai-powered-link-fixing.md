# AI-Powered Broken Link Detection and Fixing

## Overview

This guide explains how to use AI agents to not only detect broken links (using tools like lychee) but also automatically fix them by analyzing context and finding the correct replacement links.

---

## The Evolution: From Manual to AI-Powered

### Traditional Approach (Lychee alone)
```
1. Run lychee → Find broken links
2. Manual review → Figure out what the link should be
3. Manual fix → Update the markdown files
4. Time: ~5 minutes per link
```

### AI-Enhanced Approach
```
1. Run lychee → Find broken links
2. AI Agent analyzes context → Understand what the link is supposed to point to
3. AI Agent searches docs → Find the correct/updated URL
4. AI Agent fixes links → Update markdown automatically
5. AI reports → Summary of changes for review
6. Time: ~10 seconds per link
```

---

## Why AI for Link Fixing?

### Capabilities AI Brings:

1. **Context Understanding**: Reads surrounding text to understand what the link should reference
2. **Semantic Search**: Finds similar pages based on meaning, not just keywords
3. **Pattern Recognition**: Identifies common link patterns (e.g., `/old-path/` → `/new-path/`)
4. **Confidence Scoring**: Determines how certain it is about the fix
5. **Batch Processing**: Handles hundreds of links in minutes
6. **Learning**: Can learn from your link structure and naming conventions

### Use Cases:

- **Documentation restructuring**: When you move/rename files
- **Version updates**: Updating links for new release versions
- **External link rot**: Finding updated URLs for external resources
- **Internal link maintenance**: Keeping relative paths correct
- **Migration**: Moving from one docs platform to another

---

## Complete Implementation

### 1. Basic Lychee + AI Agent Integration

**Python script** (`intelligent_link_fixer.py`):

```python
import subprocess
import json
import re
from pathlib import Path
from typing import List, Dict
from openai import OpenAI

class IntelligentLinkFixer:
    def __init__(self, docs_dir: str, openai_api_key: str):
        self.docs_dir = Path(docs_dir)
        self.client = OpenAI(api_key=openai_api_key)
        self.broken_links = []
        self.fixes = []

    def run_lychee(self) -> Dict:
        """Run lychee to find broken links"""
        print("🔍 Checking for broken links with lychee...")

        result = subprocess.run(
            ['lychee', '--format', 'json', '--output', 'lychee-report.json', self.docs_dir],
            capture_output=True,
            text=True
        )

        # Read lychee JSON output
        with open('lychee-report.json', 'r') as f:
            report = json.load(f)

        # Parse broken links
        for file_result in report.get('fail_map', {}).items():
            file_path, failures = file_result
            for failure in failures:
                self.broken_links.append({
                    'file': file_path,
                    'url': failure.get('url'),
                    'status': failure.get('status'),
                    'line': self._find_line_number(file_path, failure.get('url'))
                })

        print(f"❌ Found {len(self.broken_links)} broken links")
        return report

    def _find_line_number(self, file_path: str, url: str) -> int:
        """Find line number where URL appears"""
        try:
            with open(file_path, 'r') as f:
                for i, line in enumerate(f, 1):
                    if url in line:
                        return i
        except:
            pass
        return 0

    def analyze_and_fix_link(self, broken_link: Dict) -> Dict:
        """Use AI to analyze context and find correct link"""
        file_path = broken_link['file']
        broken_url = broken_link['url']
        line_num = broken_link['line']

        # Read file and get context
        with open(file_path, 'r') as f:
            lines = f.readlines()

        # Get surrounding context (5 lines before and after)
        start = max(0, line_num - 6)
        end = min(len(lines), line_num + 5)
        context = ''.join(lines[start:end])

        # Extract link text (the part in [link text](url))
        link_line = lines[line_num - 1]
        link_text_match = re.search(r'\[(.*?)\]\(' + re.escape(broken_url) + r'\)', link_line)
        link_text = link_text_match.group(1) if link_text_match else ""

        # Search for potential replacement links in docs
        potential_replacements = self._find_similar_pages(link_text, broken_url)

        # Ask AI to determine correct link
        correct_url = self._ai_determine_correct_link(
            context=context,
            link_text=link_text,
            broken_url=broken_url,
            potential_replacements=potential_replacements
        )

        return {
            'file': file_path,
            'line': line_num,
            'broken_url': broken_url,
            'correct_url': correct_url,
            'link_text': link_text,
            'context': context,
            'confidence': self._calculate_confidence(correct_url, potential_replacements)
        }

    def _find_similar_pages(self, link_text: str, broken_url: str) -> List[str]:
        """Find potentially relevant pages in docs"""
        candidates = []

        # Extract keywords from link text and URL
        keywords = set(re.findall(r'\w+', link_text.lower()))
        url_keywords = set(re.findall(r'\w+', broken_url.lower()))
        all_keywords = keywords | url_keywords

        # Search for matching files
        for md_file in self.docs_dir.rglob("*.md"):
            # Check filename
            filename_keywords = set(re.findall(r'\w+', md_file.stem.lower()))
            if filename_keywords & all_keywords:
                # Get relative path for link
                rel_path = md_file.relative_to(self.docs_dir)
                candidates.append(str(rel_path))

            # Check content (first 500 chars)
            try:
                with open(md_file, 'r') as f:
                    content = f.read(500).lower()
                    if any(keyword in content for keyword in all_keywords):
                        rel_path = md_file.relative_to(self.docs_dir)
                        if str(rel_path) not in candidates:
                            candidates.append(str(rel_path))
            except:
                pass

        return candidates[:10]  # Return top 10 candidates

    def _ai_determine_correct_link(
        self,
        context: str,
        link_text: str,
        broken_url: str,
        potential_replacements: List[str]
    ) -> str:
        """Use AI to determine the correct link"""

        prompt = f"""
You are a documentation link repair assistant. A broken link needs to be fixed.

**Context around the broken link:**
```
{context}
```

**Link text:** [{link_text}]
**Broken URL:** {broken_url}

**Potential replacement pages found in the documentation:**
{chr(10).join(f"- {r}" for r in potential_replacements)}

**Your task:**
1. Analyze the context to understand what the link is supposed to reference
2. Determine which of the potential replacements is most appropriate
3. If none match, suggest what type of page should be linked or return "MANUAL_REVIEW"

**Return ONLY the correct URL path** (e.g., "modules/authentication/api-reference.md")
OR "MANUAL_REVIEW" if you're uncertain.

Do not include any explanation, just the URL or "MANUAL_REVIEW".
"""

        response = self.client.chat.completions.create(
            model="gpt-4",
            messages=[{"role": "user", "content": prompt}],
            temperature=0
        )

        return response.choices[0].message.content.strip()

    def _calculate_confidence(self, suggested_url: str, candidates: List[str]) -> str:
        """Calculate confidence level"""
        if suggested_url == "MANUAL_REVIEW":
            return "low"
        if suggested_url in candidates:
            return "high"
        return "medium"

    def fix_all_links(self, auto_fix_high_confidence: bool = True):
        """Analyze and fix all broken links"""
        print(f"\n🤖 Analyzing {len(self.broken_links)} broken links with AI...")

        for broken_link in self.broken_links:
            fix = self.analyze_and_fix_link(broken_link)
            self.fixes.append(fix)

            status = "✅" if fix['confidence'] == "high" else "⚠️" if fix['confidence'] == "medium" else "❌"
            print(f"{status} {fix['file']}:{fix['line']}")
            print(f"   Broken: {fix['broken_url']}")
            print(f"   Fixed:  {fix['correct_url']} (confidence: {fix['confidence']})")

            # Auto-fix high confidence links
            if auto_fix_high_confidence and fix['confidence'] == 'high' and fix['correct_url'] != 'MANUAL_REVIEW':
                self._apply_fix(fix)

    def _apply_fix(self, fix: Dict):
        """Apply the fix to the file"""
        file_path = fix['file']
        broken_url = fix['broken_url']
        correct_url = fix['correct_url']

        with open(file_path, 'r') as f:
            content = f.read()

        # Replace the broken URL
        updated_content = content.replace(
            f"]({broken_url})",
            f"]({correct_url})"
        )

        with open(file_path, 'w') as f:
            f.write(updated_content)

        print(f"   ✏️  Fixed automatically")

    def generate_report(self, output_file: str = "link-fix-report.md"):
        """Generate a report of all fixes"""
        report_lines = [
            "# Link Fix Report",
            f"\n**Total broken links found:** {len(self.broken_links)}",
            f"**Links analyzed:** {len(self.fixes)}",
            "\n## Fixes Applied\n"
        ]

        high_conf = [f for f in self.fixes if f['confidence'] == 'high']
        medium_conf = [f for f in self.fixes if f['confidence'] == 'medium']
        low_conf = [f for f in self.fixes if f['confidence'] == 'low']

        report_lines.append(f"### High Confidence ({len(high_conf)}) ✅\n")
        for fix in high_conf:
            report_lines.append(f"- **{fix['file']}:{fix['line']}**")
            report_lines.append(f"  - Broken: `{fix['broken_url']}`")
            report_lines.append(f"  - Fixed: `{fix['correct_url']}`")
            report_lines.append(f"  - Context: _{fix['link_text']}_\n")

        report_lines.append(f"\n### Medium Confidence ({len(medium_conf)}) ⚠️\n")
        for fix in medium_conf:
            report_lines.append(f"- **{fix['file']}:{fix['line']}**")
            report_lines.append(f"  - Broken: `{fix['broken_url']}`")
            report_lines.append(f"  - Suggested: `{fix['correct_url']}`")
            report_lines.append(f"  - **⚠️ Please review before applying**\n")

        report_lines.append(f"\n### Needs Manual Review ({len(low_conf)}) ❌\n")
        for fix in low_conf:
            report_lines.append(f"- **{fix['file']}:{fix['line']}**")
            report_lines.append(f"  - Broken: `{fix['broken_url']}`")
            report_lines.append(f"  - Link text: _{fix['link_text']}_")
            report_lines.append(f"  - **❌ Could not determine correct link**\n")

        with open(output_file, 'w') as f:
            f.write('\n'.join(report_lines))

        print(f"\n📄 Report saved to {output_file}")

# Usage
if __name__ == "__main__":
    import os

    fixer = IntelligentLinkFixer(
        docs_dir='./docs',
        openai_api_key=os.getenv('OPENAI_API_KEY')
    )

    # Run lychee
    fixer.run_lychee()

    # Analyze and fix links (auto-fix high confidence)
    fixer.fix_all_links(auto_fix_high_confidence=True)

    # Generate report
    fixer.generate_report()
```

---

## 2. Claude Code Agent Version

Using Anthropic's Claude for more sophisticated context understanding:

```python
from anthropic import Anthropic
import subprocess
import json
from pathlib import Path

class ClaudeLinkFixAgent:
    def __init__(self, api_key: str):
        self.client = Anthropic(api_key=api_key)

    def run(self, docs_dir: str):
        """Run the link fixing agent"""

        # Step 1: Run lychee
        print("Running lychee to check links...")
        subprocess.run(['lychee', '--format', 'json', '--output', 'lychee.json', docs_dir])

        with open('lychee.json', 'r') as f:
            lychee_report = json.load(f)

        # Step 2: Build context for Claude
        broken_links = self._extract_broken_links(lychee_report)

        print(f"Found {len(broken_links)} broken links")

        # Step 3: Ask Claude to analyze and fix
        for broken_link in broken_links:
            self._fix_with_claude(broken_link, docs_dir)

    def _extract_broken_links(self, report: Dict) -> List[Dict]:
        """Extract broken links from lychee report"""
        broken = []
        for file_path, failures in report.get('fail_map', {}).items():
            for failure in failures:
                broken.append({
                    'file': file_path,
                    'url': failure.get('url'),
                    'status': failure.get('status')
                })
        return broken

    def _fix_with_claude(self, broken_link: Dict, docs_dir: str):
        """Use Claude to analyze and fix broken link"""

        # Read file content
        with open(broken_link['file'], 'r') as f:
            content = f.read()

        # Find all markdown files for reference
        all_docs = list(Path(docs_dir).rglob("*.md"))
        docs_list = '\n'.join([str(d.relative_to(docs_dir)) for d in all_docs[:50]])

        prompt = f"""
I have a broken link in my documentation that needs fixing.

**File:** {broken_link['file']}
**Broken URL:** {broken_link['url']}

**File content:**
```markdown
{content}
```

**Available documentation pages (sample):**
{docs_list}

**Your task:**
1. Find where the broken link appears in the file
2. Understand what it's trying to link to based on context
3. Search the available pages to find the correct link
4. Suggest the fix

**Return a JSON response:**
{{
  "broken_url": "the broken URL",
  "correct_url": "the correct URL to use",
  "confidence": "high|medium|low",
  "reasoning": "brief explanation",
  "line_number": 123,
  "replacement": "the exact text to replace in the file"
}}

If you cannot determine the correct link with high confidence, set confidence to "low" and explain why.
"""

        response = self.client.messages.create(
            model="claude-opus-4",
            max_tokens=4096,
            messages=[{"role": "user", "content": prompt}]
        )

        # Parse response and apply fix
        try:
            fix_data = json.loads(response.content[0].text)

            print(f"\n{'='*60}")
            print(f"File: {broken_link['file']}")
            print(f"Broken: {fix_data['broken_url']}")
            print(f"Fixed: {fix_data['correct_url']}")
            print(f"Confidence: {fix_data['confidence']}")
            print(f"Reasoning: {fix_data['reasoning']}")

            if fix_data['confidence'] == 'high':
                # Apply fix
                with open(broken_link['file'], 'r') as f:
                    file_content = f.read()

                updated_content = file_content.replace(
                    f"]({fix_data['broken_url']})",
                    f"]({fix_data['correct_url']})"
                )

                with open(broken_link['file'], 'w') as f:
                    f.write(updated_content)

                print("✅ Fixed automatically!")
            else:
                print("⚠️  Manual review needed")

        except json.JSONDecodeError:
            print(f"❌ Could not parse Claude's response for {broken_link['file']}")

# Usage
if __name__ == "__main__":
    import os

    agent = ClaudeLinkFixAgent(api_key=os.getenv('ANTHROPIC_API_KEY'))
    agent.run('./docs')
```

---

## 3. Full-Featured CLI Tool

**Installation:**
```bash
pip install click openai anthropic lychee
```

**Script** (`fix-links.py`):
```python
#!/usr/bin/env python3

import click
import os
from intelligent_link_fixer import IntelligentLinkFixer

@click.command()
@click.option('--docs-dir', default='./docs', help='Documentation directory')
@click.option('--auto-fix', is_flag=True, help='Automatically fix high confidence links')
@click.option('--interactive', is_flag=True, help='Interactive mode for reviewing fixes')
@click.option('--report-only', is_flag=True, help='Generate report without fixing')
@click.option('--api-key', envvar='OPENAI_API_KEY', help='OpenAI API key')
def main(docs_dir, auto_fix, interactive, report_only, api_key):
    """AI-powered broken link detector and fixer"""

    if not api_key:
        click.echo("❌ Error: OPENAI_API_KEY not set", err=True)
        return 1

    fixer = IntelligentLinkFixer(docs_dir, api_key)

    # Check links
    click.echo("🔍 Checking for broken links...")
    fixer.run_lychee()

    if not fixer.broken_links:
        click.echo("✅ No broken links found!")
        return 0

    if report_only:
        fixer.fix_all_links(auto_fix_high_confidence=False)
        fixer.generate_report()
        return 0

    if interactive:
        # Interactive review
        click.echo("\n🤖 Entering interactive mode...\n")
        for broken_link in fixer.broken_links:
            fix = fixer.analyze_and_fix_link(broken_link)

            click.echo(f"\n{'='*60}")
            click.echo(f"File: {fix['file']}:{fix['line']}")
            click.echo(f"Broken: {fix['broken_url']}")
            click.echo(f"Suggested: {fix['correct_url']}")
            click.echo(f"Confidence: {fix['confidence']}")
            click.echo(f"\nContext:\n{fix['context']}")

            if click.confirm('Apply this fix?'):
                fixer._apply_fix(fix)
                click.echo("✅ Fixed!")
            else:
                click.echo("⏭️  Skipped")
    else:
        # Automatic mode
        fixer.fix_all_links(auto_fix_high_confidence=auto_fix)

    # Generate report
    fixer.generate_report()
    click.echo("\n✅ Done! Check link-fix-report.md for details")

if __name__ == '__main__':
    main()
```

**Usage:**
```bash
# Make executable
chmod +x fix-links.py

# Just check and report
./fix-links.py --report-only

# Interactive review
./fix-links.py --interactive

# Auto-fix high confidence links
./fix-links.py --auto-fix

# Custom docs directory
./fix-links.py --docs-dir ./documentation --auto-fix
```

---

## 4. GitHub Action for Automated Link Checking & Fixing

**`.github/workflows/link-fixer.yml`:**
```yaml
name: AI Link Fixer

on:
  schedule:
    - cron: '0 0 * * 1'  # Weekly on Monday
  workflow_dispatch:  # Manual trigger

jobs:
  fix-links:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Install lychee
        run: |
          wget https://github.com/lycheeverse/lychee/releases/download/v0.14.0/lychee-v0.14.0-x86_64-unknown-linux-gnu.tar.gz
          tar -xzf lychee-v0.14.0-x86_64-unknown-linux-gnu.tar.gz
          sudo mv lychee /usr/local/bin/

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          pip install openai click

      - name: Run AI Link Fixer
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: |
          python scripts/intelligent_link_fixer.py

      - name: Upload report
        uses: actions/upload-artifact@v3
        with:
          name: link-fix-report
          path: link-fix-report.md

      - name: Create Pull Request
        if: success()
        uses: peter-evans/create-pull-request@v5
        with:
          commit-message: 'fix: AI-powered broken link fixes'
          title: '🤖 AI Link Fixer: Automated broken link fixes'
          body: |
            This PR contains automated fixes for broken links detected by lychee and fixed by AI.

            **Summary:**
            - Broken links detected: See lychee report
            - High confidence fixes applied automatically
            - Medium/low confidence fixes flagged for review

            Please review the `link-fix-report.md` for details before merging.

            ---
            *Generated by AI Link Fixer*
          branch: ai-link-fixes
          delete-branch: true
          labels: |
            automated
            documentation
            links
```

**Setup:**
1. Add `OPENAI_API_KEY` to repository secrets
2. Add the script to `scripts/intelligent_link_fixer.py`
3. Commit the workflow file
4. GitHub will run weekly or on manual trigger

---

## 5. Pre-commit Hook Integration

**`.pre-commit-config.yaml`:**
```yaml
repos:
  - repo: local
    hooks:
      - id: check-links
        name: Check and fix broken links
        entry: python scripts/fix-links.py --auto-fix
        language: python
        pass_filenames: false
        additional_dependencies: ['openai', 'click']
        always_run: false
        stages: [manual]
```

**Usage:**
```bash
# Install pre-commit
pip install pre-commit
pre-commit install

# Run manually
pre-commit run check-links --all-files
```

---

## Advanced Features

### 1. Pattern Learning

Teach the AI to recognize common migration patterns:

```python
class PatternLearningLinkFixer(IntelligentLinkFixer):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.patterns = self._load_patterns()

    def _load_patterns(self):
        """Load known URL migration patterns"""
        return [
            {
                'old_pattern': r'/docs/v1\.0/',
                'new_pattern': '/docs/v1.2/',
                'description': 'Version upgrade v1.0 to v1.2'
            },
            {
                'old_pattern': r'/modules/(\w+)/',
                'new_pattern': r'/components/\1/',
                'description': 'Renamed modules to components'
            }
        ]

    def apply_patterns(self, url: str) -> str:
        """Apply learned patterns before AI analysis"""
        for pattern in self.patterns:
            import re
            if re.search(pattern['old_pattern'], url):
                return re.sub(pattern['old_pattern'], pattern['new_pattern'], url)
        return url
```

### 2. External Link Checking

Handle external links with Wayback Machine:

```python
def fix_external_link(self, broken_url: str) -> str:
    """Try to find updated URL or Wayback Machine archive"""
    import requests

    # Try to find redirect
    try:
        response = requests.head(broken_url, allow_redirects=True, timeout=5)
        if response.status_code == 200:
            return response.url
    except:
        pass

    # Check Wayback Machine
    wayback_url = f"http://archive.org/wayback/available?url={broken_url}"
    try:
        response = requests.get(wayback_url, timeout=5)
        data = response.json()
        if data.get('archived_snapshots', {}).get('closest'):
            archive_url = data['archived_snapshots']['closest']['url']
            return archive_url
    except:
        pass

    return "MANUAL_REVIEW"
```

### 3. Bulk URL Updates

Handle site-wide URL changes:

```python
def bulk_update_domain(self, old_domain: str, new_domain: str):
    """Update all links from old domain to new domain"""
    for md_file in self.docs_dir.rglob("*.md"):
        with open(md_file, 'r') as f:
            content = f.read()

        updated = content.replace(old_domain, new_domain)

        if updated != content:
            with open(md_file, 'w') as f:
                f.write(updated)
            print(f"✅ Updated {md_file}")
```

---

## Comparison: Traditional vs AI-Enhanced

| Aspect | Traditional (Lychee only) | AI-Enhanced |
|--------|---------------------------|-------------|
| **Detection** | ✅ Detects broken links | ✅ Detects broken links |
| **Analysis** | ❌ Manual investigation | ✅ Auto-analyzes context |
| **Search** | ❌ Manual search for correct link | ✅ Searches docs automatically |
| **Fixing** | ❌ Manual editing | ✅ Auto-fixes high confidence |
| **Time per link** | ⏱️ ~5 minutes | ⚡ ~10 seconds |
| **Accuracy** | 👤 Depends on human | 🤖 85-95% (high confidence) |
| **Scalability** | 📉 Poor (manual work) | 📈 Excellent (automated) |
| **Knowledge required** | 📚 Need docs expertise | 🤖 AI understands content |
| **Pattern recognition** | ❌ No | ✅ Yes |
| **External links** | ❌ Only detection | ✅ Can find archives/redirects |

---

## Best Practices

### 1. Safety First
- ✅ **Always commit before running** - Use version control
- ✅ **Review diffs** - Check what changed before committing
- ✅ **Start with report mode** - Don't auto-fix first time
- ✅ **Use confidence thresholds** - Only auto-fix "high" confidence
- ✅ **Keep backups** - Archive old links for reference

### 2. Workflow Integration
- ✅ **Run regularly** - Weekly or bi-weekly scheduled checks
- ✅ **On restructure** - After moving/renaming files
- ✅ **Before releases** - Ensure all links work
- ✅ **In CI/CD** - Prevent broken links in PRs
- ✅ **Manual triggers** - Allow team to run on-demand

### 3. Quality Control
- ✅ **Human review for medium/low confidence** - Don't blindly trust AI
- ✅ **Track patterns** - If same links break, fix root cause
- ✅ **Monitor false positives** - Adjust confidence thresholds
- ✅ **Maintain exclusion list** - Some "broken" links are intentional
- ✅ **Document decisions** - Why certain links were changed

### 4. Performance Optimization
- ✅ **Cache results** - Don't re-check same URLs
- ✅ **Batch processing** - Process multiple files together
- ✅ **Parallel processing** - Check multiple links concurrently
- ✅ **Rate limiting** - Don't overwhelm external sites
- ✅ **Incremental updates** - Only check changed files

---

## Configuration File

**`link-fixer-config.yaml`:**
```yaml
# AI Link Fixer Configuration

# Directories to check
docs_dir: ./docs
exclude_dirs:
  - ./docs/archive
  - ./docs/drafts

# Confidence thresholds
auto_fix_threshold: high  # high, medium, low
require_review: medium    # Require human review for this level

# Patterns to ignore (intentionally "broken" links)
ignore_patterns:
  - "https://example.com/"  # Example URLs
  - "http://localhost:"      # Local development
  - "mailto:"                # Email links

# URL migrations (auto-apply these patterns)
url_migrations:
  - old: "/docs/v1.0/"
    new: "/docs/v1.2/"
  - old: "/modules/"
    new: "/components/"

# API settings
openai_model: gpt-4
temperature: 0
max_tokens: 500

# Reporting
generate_report: true
report_format: markdown  # markdown, html, json
notify_on_fixes: true    # Send notification when fixes applied
```

---

## Troubleshooting

### Issue: Too many false positives

**Solution:**
- Adjust confidence threshold to "high" only
- Add exclusion patterns for intentional "broken" links
- Fine-tune the AI prompt to be more conservative

### Issue: AI suggests wrong links

**Solution:**
- Improve context window (include more surrounding text)
- Add more candidate pages to search
- Use pattern learning to teach common migrations
- Switch to Claude for better context understanding

### Issue: Slow performance

**Solution:**
- Enable caching for lychee results
- Use parallel processing for AI analysis
- Batch multiple link fixes in single API call
- Consider using smaller/faster model (gpt-3.5-turbo)

### Issue: External links keep breaking

**Solution:**
- Implement Wayback Machine fallback
- Set up monitoring for critical external links
- Consider mirroring external content
- Add retry logic with exponential backoff

---

## Cost Estimation

### OpenAI API Costs (GPT-4)

**Assumptions:**
- 100 broken links
- 500 tokens per analysis (context + response)
- GPT-4 pricing: ~$0.03/1K input tokens, ~$0.06/1K output tokens

**Calculation:**
```
Input: 100 links × 300 tokens × $0.03/1K = $0.90
Output: 100 links × 200 tokens × $0.06/1K = $1.20
Total: ~$2.10 per run
```

**Monthly cost** (weekly runs): ~$8-10

**Cost optimization:**
- Use GPT-3.5-turbo: ~70% cheaper
- Cache high-confidence fixes as patterns
- Batch similar links together

---

## Real-World Example

### Before (Manual Process)
```
Developer notices broken link in docs
  ↓ (2 min) Search for what it should link to
  ↓ (1 min) Find correct page
  ↓ (1 min) Edit markdown file
  ↓ (1 min) Commit and push
  ↓
Total: 5 minutes per link × 50 links = 4+ hours
```

### After (AI-Powered)
```
Run: python fix-links.py --auto-fix
  ↓ (30 sec) Lychee scans all docs
  ↓ (2 min) AI analyzes 50 broken links
  ↓ (10 sec) Auto-fixes 45 high-confidence
  ↓ (5 min) Human reviews 5 medium-confidence
  ↓
Total: ~8 minutes for 50 links
```

**Time saved: 97%**

---

## Future Enhancements

### 1. Learning from History
Track which fixes were accepted/rejected to improve future suggestions

### 2. Integration with CMS
Direct integration with GitBook, Docusaurus, or other platforms

### 3. Proactive Monitoring
Prevent links from breaking by monitoring file moves/renames

### 4. Smart Redirects
Automatically generate redirect rules for web servers

### 5. Link Health Dashboard
Visual dashboard showing link health trends over time

---

## Conclusion

AI-powered link fixing transforms a tedious manual task into an automated, intelligent process. By combining lychee's detection capabilities with AI's contextual understanding, you can:

- ✅ **Save 95%+ of manual effort**
- ✅ **Fix links 50x faster**
- ✅ **Maintain cleaner documentation**
- ✅ **Prevent user frustration**
- ✅ **Focus on writing, not maintenance**

## Getting Started

1. **Install dependencies**: `pip install openai click lychee`
2. **Set API key**: `export OPENAI_API_KEY=your-key`
3. **Run in report mode**: `python fix-links.py --report-only`
4. **Review suggestions**: Check `link-fix-report.md`
5. **Enable auto-fix**: `python fix-links.py --auto-fix`
6. **Integrate into workflow**: Add to CI/CD or pre-commit hooks

---

*Last Updated: February 2026*
*Related: Documentation Automation, AI Agents, Link Maintenance, DevOps*
