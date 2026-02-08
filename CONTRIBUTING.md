# Contributing to Awesome OpenClaw

First off, thank you for considering contributing to Awesome OpenClaw! It's people like you that make the open-source community such a great place to learn and create.

---

## Table of Contents

- [How to Contribute](#how-to-contribute)
- [Development Setup](#development-setup)
- [Adding Resources](#adding-resources)
- [Style Guidelines](#style-guidelines)
- [Pull Request Process](#pull-request-process)
- [Community Guidelines](#community-guidelines)

---

## How to Contribute

There are many ways to contribute:

###  Adding Resources

Found an awesome OpenClaw resource? We'd love to add it!

- **Skills**: Submit a new skill or plugin
- **Guides**: Submit a tutorial, blog post, or video
- **Tools**: Submit utilities, scripts, or integrations
- **Configs**: Share useful configurations

###  Reporting Issues

Found a bug or have a suggestion?

1. Check existing [issues](https://github.com/geekjourneyx/awesome-openclaw/issues)
2. Create a new issue with:
   - Clear title
   - Detailed description
   - Steps to reproduce (for bugs)
   - Expected vs actual behavior

###  Improving Documentation

- Fix typos or grammatical errors
- Add missing information
- Improve clarity of existing content
- Translate content to other languages

###  Translating

Help make OpenClaw accessible to more people:

- Translate README to your language
- Translate documentation
- Create language-specific guides

---

## Development Setup

### 1. Fork the Repository

```bash
# Fork the repository on GitHub
# Then clone your fork
git clone https://github.com/YOUR_USERNAME/awesome-openclaw.git
cd awesome-openclaw
```

### 2. Create a Branch

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bug-fix
```

### 3. Make Your Changes

Edit the relevant files following our [Style Guidelines](#style-guidelines).

### 4. Test Your Changes

```bash
# If you've modified README structure
# Preview the markdown rendering
gem install github-pages
jekyll serve
# View at http://localhost:4000
```

---

## Adding Resources

### Adding a Skill

Add skills to the appropriate category in the README files:

```markdown
#### Category Name

| Skill | Description | Repository |
|-------|-------------|------------|
| **Skill Name** | Brief description of what it does | `github-user/repo-name` |
```

**Required information:**
- Skill name (bold)
- One-line description
- Repository link (GitHub/GitLab)
- Optionally: star count, language, license

### Adding a Guide

Add guides to the appropriate section:

```markdown
| [Guide Title](https://example.com/guide) | Duration | Description |
|----------|----------|-------------|
```

**Required information:**
- Guide title (with link)
- Reading/watching time
- Brief description
- Language (if not English)

### Adding a Tutorial

```markdown
### Tutorial Title

- **Author**: [@author](https://github.com/author)
- **Time**: X min read
- **Level**: Beginner/Intermediate/Advanced
- **Topics**: topic1, topic2, topic3

> Brief description of what readers will learn.
```

---

## Style Guidelines

### Markdown Formatting

- Use ATX-style headings (`#` vs `##`)
- Add a blank line before headings
- Use bullet lists for items
- Use tables for structured data
- Use code blocks with language tags

### Links

```markdown
<!-- External links -->
[Link text](https://example.com)

<!-- Internal links -->
[Link text](#heading-id)

<!-- Reference links -->
[Link text][reference-id]
```

### Code Blocks

```markdown
<!-- Specify language for syntax highlighting -->
```python
def hello():
    print("Hello, OpenClaw!")
```
```

### Resource Descriptions

- Keep descriptions concise (one line when possible)
- Use present tense ("Integrates with", not "Will integrate with")
- Start with action verbs ("Create", "Manage", "Automate")
- Avoid marketing language

---

## Pull Request Process

### 1. Update Your Branch

```bash
git checkout main
git pull upstream main
git checkout your-branch
git rebase main
```

### 2. Commit Your Changes

```bash
git add .
git commit -m "feat: add new skill category for email integrations"
```

**Commit message format:**
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation changes
- `style:` Formatting changes
- `refactor:` Code refactoring
- `test:` Adding tests
- `chore:` Maintenance tasks

### 3. Push and Create PR

```bash
git push origin your-branch
```

Then create a pull request on GitHub with:

- Clear title
- Description of changes
- Reference related issues
- Screenshots if applicable

### 4. PR Review Process

1. Automated checks run
2. Maintainers review your PR
3. Address any feedback
4. PR gets merged!

---

## Community Guidelines

### Our Pledge

We pledge to make participation in our community a harassment-free experience for everyone.

### Our Standards

**Positive behavior includes:**
- Using welcoming and inclusive language
- Being respectful of differing viewpoints
- Gracefully accepting constructive criticism
- Focusing on what is best for the community

**Unacceptable behavior includes:**
- Harassment or discrimination
- Personal insults or derogatory comments
- Public or private harassment
- Publishing private information
- Unprofessional conduct

### Enforcement

Project maintainers reserve the right to remove, edit, or reject comments, commits, code, wiki edits, issues, and other contributions that are not aligned with this Code of Conduct.

---

## Recognition

Contributors are recognized in:
- The [Contributors](https://github.com/geekjourneyx/awesome-openclaw/graphs/contributors) section
- Release notes for significant contributions
- Annual community appreciation posts

---

## Need Help?

-  Read the [documentation](https://docs.openclaw.ai/)
-  Join our [Discord](https://discord.gg/openclaw)
-  Open a [discussion](https://github.com/geekjourneyx/awesome-openclaw/discussions)

---

<div align="center">

**Happy contributing! **

[ Back to Top](#contributing-to-awesome-openclaw)

</div>
