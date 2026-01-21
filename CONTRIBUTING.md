# Contributing to CL4R1T4S

Thank you for your interest in contributing to CL4R1T4S! This document provides guidelines and standards for contributions to ensure consistency and quality across the archive.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Types of Contributions](#types-of-contributions)
- [Submission Guidelines](#submission-guidelines)
- [File Naming Conventions](#file-naming-conventions)
- [Content Standards](#content-standards)
- [Metadata Template](#metadata-template)
- [Pull Request Process](#pull-request-process)
- [Quality Checklist](#quality-checklist)

---

## Code of Conduct

- **Respect Privacy:** Do not include personal information, API keys, or credentials in submissions
- **Accuracy First:** Only submit authentic prompts; do not fabricate or modify content
- **Attribution:** Credit sources when possible and respect intellectual property
- **Transparency:** Be honest about extraction methods and limitations
- **Responsibility:** Do not use or encourage use of this information for malicious purposes

---

## Types of Contributions

We welcome the following types of contributions:

### 1. New System Prompts

- Base system prompts from AI models
- Specialized prompts (coding, research, voice, etc.)
- Updated versions of existing prompts

### 2. Tool Definitions

- Function/tool schemas
- API definitions
- Command references

### 3. Configuration Files

- Personality settings
- Style configurations
- Feature toggles

### 4. Documentation

- Corrections to existing documentation
- Analysis or annotations (in separate files)
- Translation of prompts

---

## Submission Guidelines

### Before Submitting

1. **Check for duplicates:** Search the repository to ensure the prompt hasn't already been submitted
2. **Verify authenticity:** Ensure the prompt is genuine and not fabricated
3. **Anonymize if needed:** Remove any personal data or identifying information

### How to Submit

1. **Fork** the repository
2. **Create a branch** with a descriptive name: `add/model-name-version`
3. **Add your file(s)** following the naming conventions below
4. **Include metadata** at the top of the file (see template)
5. **Submit a Pull Request** with a clear description

---

## File Naming Conventions

Use the following format for file names:

```
ModelName_Version_MM-DD-YYYY.extension
```

### Components

| Component | Description | Example |
|-----------|-------------|---------|
| `ModelName` | Name of the AI model/product | `ChatGPT`, `Claude`, `Gemini` |
| `Version` | Version or variant (optional) | `4o`, `Sonnet_3.5`, `2.0` |
| `MM-DD-YYYY` | Extraction date (optional but preferred) | `04-25-2025` |
| `.extension` | File type | `.md` or `.txt` |

### Examples

```
ChatGPT_4o_04-25-2025.txt
Claude_Sonnet_4.txt
Devin_2.0.md
Cursor_Tools.md
```

### File Extensions

- Use `.md` for files with markdown formatting
- Use `.txt` for plain text prompts without formatting
- Prefer `.md` for new submissions as it supports better formatting

---

## Content Standards

### Do Include

- Complete system prompts (do not truncate)
- All tool/function definitions
- Timestamps and version information when available
- Context about the extraction method (in metadata)

### Do Not Include

- Personal information or credentials
- API keys or tokens
- Fabricated or modified content
- Copyrighted third-party content unrelated to the prompt

### Formatting Guidelines

1. **Preserve original formatting** as much as possible
2. **Use code blocks** for structured content (XML, JSON, etc.)
3. **Escape special characters** if they cause rendering issues
4. **Maintain line breaks** that appear in the original

---

## Metadata Template

Add the following metadata header at the top of each prompt file:

```markdown
---
model: [Model Name]
version: [Version Number/Name]
provider: [Company Name]
extraction_date: [YYYY-MM-DD]
extraction_method: [Brief description - optional]
contributor: [Your GitHub username - optional]
notes: [Any relevant notes - optional]
---

[Prompt content starts here]
```

### Example

```markdown
---
model: Claude Sonnet
version: 4
provider: Anthropic
extraction_date: 2025-05-22
extraction_method: Conversation analysis
contributor: @username
notes: Includes web search and artifacts tools
---

The assistant is Claude, created by Anthropic...
```

---

## Pull Request Process

### 1. Create Your PR

- Use a clear, descriptive title: `Add: [Model Name] system prompt`
- Fill out the PR template completely
- Reference any related issues

### 2. PR Description Should Include

- Model name and version
- Provider/company
- Extraction date (if known)
- Brief description of what's included
- Any known limitations or missing sections

### 3. Review Process

- Maintainers will review for authenticity and quality
- You may be asked for clarification or modifications
- Once approved, your PR will be merged

### 4. After Merge

- Your contribution will be credited
- The README catalog will be updated
- Thank you for contributing to AI transparency!

---

## Quality Checklist

Before submitting, verify:

- [ ] File follows naming conventions
- [ ] Metadata header is included
- [ ] Content is complete (not truncated)
- [ ] No personal information or credentials included
- [ ] No duplicate of existing content
- [ ] File renders correctly (check markdown preview)
- [ ] Placed in the correct directory
- [ ] PR description is complete

---

## Directory Structure

Place files in the appropriate directory:

| Directory | Content |
|-----------|---------|
| `ANTHROPIC/` | Claude models |
| `OPENAI/` | ChatGPT, GPT models |
| `GOOGLE/` | Gemini models |
| `XAI/` | Grok models |
| `CURSOR/` | Cursor IDE |
| `WINDSURF/` | Windsurf/Cascade |
| `DEVIN/` | Devin autonomous developer |
| `REPLIT/` | Replit agent |
| `BOLT/` | Bolt IDE |
| `MANUS/` | Manus agent |
| `MULTION/` | MultiOn |
| `PERPLEXITY/` | Perplexity |
| `MISTRAL/` | Mistral/LeChat |
| `HUME/` | Hume AI |
| `LOVABLE/` | Lovable platform |
| `SAMEDEV/` | SameDev |
| `VERCEL_V0/` | Vercel v0 |

**New Provider?** Create a new directory using UPPERCASE naming.

---

## Questions?

- **GitHub Issues:** Open an issue for questions or suggestions
- **X (Twitter):** [@elder_plinius](https://x.com/elder_plinius)
- **Discord:** elder_plinius

---

Thank you for contributing to AI transparency!

*Love, Pliny <3*
