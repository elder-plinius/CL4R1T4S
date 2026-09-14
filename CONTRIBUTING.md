# Contributing to CL4R1T4S

Thank you for helping make AI system prompts more transparent! Every contribution helps the community understand how AI systems are instructed behind the scenes.

## How to Submit a Prompt

1. **Fork** this repository
2. **Add your prompt file** to the appropriate company directory (see [Directory Structure](#directory-structure))
3. **Open a pull request** with a clear description

### File Requirements

Every submitted prompt file should include:

- ✅ **Model name and version** (in the filename and metadata header)
- 🗓 **Date of extraction** in ISO 8601 format (`YYYY-MM-DD`), if known
- 🧾 **Context or notes** (optional but helpful)

### Metadata Header

Please add a metadata header at the top of every prompt file:

```markdown
---
model: "Claude 4.1"
company: "Anthropic"
date_extracted: "2025-08-05"
extraction_method: "Direct prompt extraction"
contributor: "your-github-username"
notes: "Extracted via conversation prompt leak"
---
```

If you don't know a field, omit it or write `unknown`.

## Naming Conventions

### Filenames

- Use **underscores** (`_`) as word separators: `Claude_Sonnet_3.5.md`
- Include the **extraction date** in ISO 8601 format when known: `ChatGPT_4o_2025-04-25.txt`
- Use `.md` or `.txt` file extensions — **never omit the extension**
- Format: `ModelName_Version_YYYY-MM-DD.md`

### Dates

- Always use **ISO 8601**: `YYYY-MM-DD` (e.g., `2025-04-25`)
- Never use `Month-DD-YYYY`, `MM-DD-YY`, or other formats

### Directory Names

- Use **ALL CAPS** for company/product directory names: `OPENAI`, `ANTHROPIC`, `XAI`
- Use **hyphens** instead of spaces: `BURGESS-PRINCIPLE`, `VERCEL-V0`

## Directory Structure

Each top-level directory corresponds to a company or product:

```
ANTHROPIC/       — Anthropic (Claude models)
BOLT/            — Bolt
BRAVE/           — Brave (Leo)
BURGESS-PRINCIPLE/ — The Burgess Principle framework (not a prompt)
CLINE/           — Cline
CLUELY/          — Cluely
CURSOR/          — Cursor
DEVIN/           — Devin
DIA/             — Dia
FACTORY/         — Factory (DROID)
GOOGLE/          — Google (Gemini models)
HUME/            — Hume
LOVABLE/         — Lovable
MANUS/           — Manus
META/            — Meta (Llama models)
MINIMAX/         — MiniMax
MISTRAL/         — Mistral (Le Chat)
MOONSHOT/        — Moonshot (Kimi models)
MULTION/         — MultiOn
OPENAI/          — OpenAI (ChatGPT, GPT, Codex, Atlas)
PERPLEXITY/      — Perplexity
REPLIT/          — Replit
SAMEDEV/         — SameDev
VERCEL-V0/       — Vercel v0
WINDSURF/        — Windsurf
XAI/             — xAI (Grok models)
```

If your prompt is from a company not listed here, create a new ALL-CAPS directory for it.

## What Counts as a Contribution?

- **System prompts** — the hidden instructions that shape model behavior
- **Tool definitions** — function/tool schemas given to models
- **Guidelines or policies** — internal moderation or behavior guidelines
- **Personality/persona configs** — instructions that define how a model presents itself

## Community

- **X (Twitter):** [@elder_plinius](https://x.com/elder_plinius)
- **Discord:** Reach out to @elder_plinius

## License

By contributing, you agree that your contributions will be licensed under the [GNU AGPL v3](LICENSE) license that covers this repository. The [Burgess Principle](BURGESS-PRINCIPLE/) directory is separately licensed under MIT (see [LICENSE-BURGESS](LICENSE-BURGESS)).
