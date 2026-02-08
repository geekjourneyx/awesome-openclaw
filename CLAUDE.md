# awesome-openclaw - Project Memory

> **Project**: awesome-openclaw - OpenClaw 资源、指南和最佳实践精选合集
>
> **Author**: geekjourneyx
>
> **X (Twitter)**: https://x.com/seekjourney
>
> **WeChat Official Account**: 极客杰尼

---

## Project Overview

**awesome-openclaw** is a curated collection of OpenClaw resources, guides, skills, and best practices. OpenClaw is an open-source AI agent platform that runs on your machine and integrates with chat apps like WhatsApp, Telegram, Discord, etc.

### Key Information

| Item | Value |
|------|-------|
| **Repository** | openclaw-tools/awesome-openclaw |
| **License** | MIT |
| **Languages** | English (primary), Chinese (simplified) |
| **Target Audience** | Global developers, Chinese AI/LLM enthusiasts |
| **SEO Focus** | OpenClaw, AI agent, chatbot, WhatsApp, Telegram, Claude, GPT |

---

## Project Structure

```
awesome-openclaw/
├── README.md                    # English main documentation
├── README_zh.md                 # Chinese (简体中文) documentation
├── CHANGELOG.md                 # Version history (Keep a Changelog format)
├── CONTRIBUTING.md              # Contribution guidelines
├── CODE_OF_CONDUCT.md           # Community guidelines
├── SECURITY.md                  # Security policy
├── LICENSE                      # MIT License
├── CLAUDE.md                    # This file - project memory
├── .markdownlint.json           # Markdown linting rules
├── .gitignore                   # Git ignore rules
└── docs/
    ├── web-tools-guide.md       # Web Tools guide (English)
    ├── web-tools-guide-zh.md    # Web Tools 指南 (中文)
    ├── browser-guide.md         # Browser Tool guide (English)
    └── browser-guide-zh.md      # Browser 工具指南 (中文)
```

---

## Pre-Commit Workflow (CRITICAL)

**BEFORE every commit**, execute this workflow:

### Step 1: Documentation Quality Check

#### Markdown Formatting Standards

- [PASS] Use ATX-style headings (`#` vs `##`)
- [PASS] Add blank line before headings
- [PASS] Use bullet lists for items
- [PASS] Use tables for structured data
- [PASS] Use code blocks with language tags
- [PASS] Line length ≤ 120 characters (except tables/code blocks)
- [PASS] No trailing whitespace
- [PASS] Consistent formatting across files

#### Reading Experience Standards

- [PASS] Clear table of contents at the top
- [PASS] Logical section ordering
- [PASS] Consistent heading hierarchy
- [PASS] Use `<details>` for expandable content
- [PASS] Use `<div align="center">` for centered content
- [FAIL] NO EMOJIS in documentation (strictly prohibited)
- [PASS] Clear visual separation with `---` between major sections

#### SEO Best Practices

- [PASS] Semantic heading structure (H1 → H2 → H3)
- [PASS] Descriptive link text (not "click here")
- [PASS] Keywords naturally distributed
- [PASS] Internal anchor links working
- [PASS] External links open in new tab when appropriate

### Step 2: Bilingual Consistency Check

#### For Each Documentation File:

| Check | English | Chinese |
|-------|---------|---------|
| Structure matches | [PASS] | [PASS] |
| Sections aligned | [PASS] | [PASS] |
| Code examples same | [PASS] | [PASS] |
| Tables match | [PASS] | [PASS] |
| Links work | [PASS] | [PASS] |

#### Translation Quality

- [PASS] Technical terms correctly translated (or kept in English where appropriate)
- [PASS] Tone and style consistent
- [PASS] No machine translation artifacts
- [PASS] Cultural adaptation where needed
- [PASS] Proper Chinese punctuation (full-width vs half-width)

#### Common Terms Reference

| English | Chinese |
|---------|---------|
| Agent | 代理 / 智能体 |
| Channel | 渠道 / 频道 |
| Skill | 技能 / 插件 |
| Model | 模型 |
| Provider | 提供商 |
| API Key | API 密钥 |
| Configuration | 配置 |
| Snapshot | 快照 |
| Screenshot | 截图 |
| Profile | 配置文件 / 档案 |

### Step 3: Content Verification

- [PASS] All code examples are accurate
- [PASS] All commands are tested
- [PASS] All links are valid
- [PASS] No placeholder content left in
- [PASS] No TODO comments in final content
- [PASS] Dates and versions are current

### Step 4: Automated Checks

```bash
# Run markdown linter
npx markdownlint '**/*.md'

# Check for broken links
npx lychee '**/*.md'

# Validate JSON files
cat .markdownlint.json | jq .
```

### Step 5: Update CHANGELOG.md

After all checks pass:

1. Add entry to `[Unreleased]` section
2. Use proper format:
   ```markdown
   ### Added
   - New feature description

   ### Changed
   - Changed feature description

   ### Fixed
   - Bug fix description
   ```

### Step 6: Version Release (when ready)

1. Update version in `CHANGELOG.md`
2. Create new version section
3. Commit with message: `chore: release v0.x.y`
4. Create Git tag: `git tag v0.x.y`
5. Push tag: `git push origin v0.x.y`

### Step 7: User Confirmation

**STOP and ask user:**

```
[PASS] All checks passed!

Changes to be committed:
- [List of files]

Updated CHANGELOG.md:
- [Summary of changes]

Ready to commit? Type "yes" to proceed.
```

**Only commit after user confirms!**

---

## Commit Message Rules

- DO NOT mention AI assistant name or email in commits
- Use conventional commit format: `type: description`
- Types: feat, fix, docs, style, refactor, test, chore
- Keep subject line under 72 characters
- Use body for detailed explanation when needed

---

## Documentation Style Guide

### Headings

```markdown
# H1 - Main title (once per file)

## H2 - Major sections

### H3 - Sub-sections

#### H4 - Nested sub-sections (avoid going deeper)
```

### Tables

```markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |
```

### Code Blocks

````markdown
```bash
# Shell commands
command_here
```

```python
# Python code
def function():
    pass
```
````

### Links

```markdown
[External link](https://example.com)
[Internal link](#section-id)
[Reference link][ref-id]

[ref-id]: https://example.com
```

### Emphasis

```markdown
*italic* or _italic_
**bold** or __bold__
***bold italic*** or ___bold italic___
`code`
```

### Lists

```markdown
- Unordered item
- Another item

1. Ordered item
2. Another item

- [x] Completed task
- [ ] Incomplete task
```

---

## File-Specific Guidelines

### README.md / README_zh.md

- **Language switch at top**: `[English](README.md) | [简体中文](README_zh.md)`
- **TOC after intro**: Complete table of contents
- **Badge section**: Project badges
- **Installation section**: Clear step-by-step
- **Contributing section**: How to contribute
- **License section**: MIT License reference

### docs/*.md

- **Back link**: `[← Back to Guides](../README.md)`
- **Language switch**: At top of each guide
- **Code examples**: Tested and working
- **Screenshots**: When helpful (use relative paths)
- **Date/version**: Last updated date

### CHANGELOG.md

- **Format**: Keep a Changelog
- **Sections**: Added, Changed, Deprecated, Removed, Fixed, Security
- **Links**: Version comparison links at bottom
- **Dates**: ISO 8601 format (YYYY-MM-DD)

---

## SEO Guidelines

### Keywords (Primary)

- OpenClaw
- AI agent
- Chatbot automation
- WhatsApp bot
- Telegram bot
- Claude AI
- GPT integration
- Self-hosted AI

### Meta Tags (for GitHub)

```
Description: A curated collection of OpenClaw resources, guides, skills, and best practices.

Topics: openclaw, ai-agent, chatbot, whatsapp, telegram, discord, llm, claude, gpt, awesome-list, open-source, self-hosted
```

### Heading Structure

```
H1: Main title (includes project name + description)
├── H2: Table of Contents
├── H2: Overview
├── H2: Getting Started
│   ├── H3: What is...
│   └── H3: Key Features
├── H2: Installation
│   ├── H3: Requirements
│   └── H3: Methods
└── H2: ...
```

---

## Common Tasks

### Adding a New Guide

1. Create `docs/guide-name.md` (English)
2. Create `docs/guide-name-zh.md` (Chinese)
3. Add to README tutorial section (both languages)
4. Update CHANGELOG.md
5. Run pre-commit workflow
6. Get user confirmation
7. Commit

### Updating Existing Content

1. Update both language versions
2. Ensure consistency
3. Update CHANGELOG.md
4. Run pre-commit workflow
5. Get user confirmation
6. Commit

### Adding New Resources

1. Update both README files
2. Verify links work
3. Update CHANGELOG.md
4. Run pre-commit workflow
5. Get user confirmation
6. Commit

---

## Quick Reference Commands

```bash
# Markdown linting
npx markdownlint '**/*.md' --fix

# Link checking
npx lychee '**/*.md' --verbose

# Check for trailing whitespace
grep -rn "[[:space:]]$" --include="*.md" .

# Check line length
grep -rn ".\{121,\}" --include="*.md" .

# Validate JSON
jq < .markdownlint.json
jq < package.json
```

---

## Project Goals

1. **Comprehensive**: Cover all aspects of OpenClaw
2. **Bilingual**: Maintain high-quality English and Chinese content
3. **SEO-Optimized**: Easy to discover via search
4. **User-Friendly**: Clear, well-organized documentation
5. **Up-to-Date**: Keep pace with OpenClaw developments
6. **Community-Driven**: Welcome contributions

---

## Contact & Social

- **Author**: geekjourneyx
- **X (Twitter)**: https://x.com/seekjourney
- **WeChat**: 极客杰尼
- **Discord**: https://discord.gg/openclaw
- **GitHub Issues**: https://github.com/openclaw-tools/awesome-openclaw/issues

---

## Notes

- This file should be updated as the project evolves
- Keep it in sync with actual project state
- Review and update monthly
- Archive old workflows to separate file if needed

---

**Last Updated**: 2025-02-08
**Version**: 0.1.0
