# CL4R1T4S

**AI System Prompt Transparency Archive**

> *"In order to trust the output, one must understand the input."*

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](CONTRIBUTING.md)

---

## Table of Contents

- [Overview](#overview)
- [Why This Exists](#why-this-exists)
- [Prompt Catalog](#prompt-catalog)
- [Directory Structure](#directory-structure)
- [Contributing](#contributing)
- [Security Considerations](#security-considerations)
- [Legal Disclaimer](#legal-disclaimer)
- [License](#license)
- [Contact](#contact)

---

## Overview

CL4R1T4S is a comprehensive transparency-focused repository that collects, archives, and distributes system prompts from major AI models and coding assistants. This project documents the hidden instructions that shape AI behavior, providing researchers, developers, and the public with insight into how AI systems are programmed to respond.

**Currently Archived:** 16+ AI platforms with 35+ system prompts

---

## Why This Exists

AI labs shape how models behave using massive, unseen prompt scaffolds. Because AI serves as a trusted external intelligence layer for a growing number of humans, these hidden instructions can affect the perceptions and behavior of the public.

These prompts define:

- What AIs can and cannot say
- What personas and functions they are forced to follow
- How they are instructed to refuse, redirect, or decline requests
- What ethical, political, and behavioral frames are embedded by default

**If you are interacting with an AI without knowing its system prompt, you are not talking to a neutral intelligence—you are talking to a shadow-puppet.**

CL4R1T4S is here to fix that.

---

## Prompt Catalog

### Anthropic (Claude)

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Claude_4.txt](ANTHROPIC/Claude_4.txt) | Claude Sonnet 4 | Base System Prompt | May 2025 |
| [Claude_Sonnet_3.5.md](ANTHROPIC/Claude_Sonnet_3.5.md) | Claude Sonnet 3.5 | Base System Prompt | 2024 |
| [Claude_Sonnet_3.7_New.txt](ANTHROPIC/Claude_Sonnet_3.7_New.txt) | Claude Sonnet 3.7 | Base System Prompt | 2025 |
| [Claude_Code_03-04-24.md](ANTHROPIC/Claude_Code_03-04-24.md) | Claude Code | Agentic Coding Assistant | March 2024 |
| [UserStyle_Modes.md](ANTHROPIC/UserStyle_Modes.md) | Claude | Style Configuration | 2024 |

### OpenAI (ChatGPT/GPT)

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [ChatGPT_4.1_05-15-2025.txt](OPENAI/ChatGPT_4.1_05-15-2025.txt) | ChatGPT 4.1 | Base System Prompt | May 2025 |
| [ChatGPT_4o_04-25-2025.txt](OPENAI/ChatGPT_4o_04-25-2025.txt) | ChatGPT 4o | Base System Prompt | April 2025 |
| [ChatGPT_o3_o4-mini_04-16-2025.txt](OPENAI/ChatGPT_o3_o4-mini_04-16-2025.txt) | o3/o4-mini | Reasoning Model | April 2025 |
| [GPT-4.5_02-27-25.md](OPENAI/GPT-4.5_02-27-25.md) | GPT-4.5 | Base System Prompt | February 2025 |
| [ChatGPT_Personality_v2_Change.md](OPENAI/ChatGPT_Personality_v2_Change.md) | ChatGPT | Personality Config | 2024 |
| [GPT-4o_Image_Gen_Postfill.txt](OPENAI/GPT-4o_Image_Gen_Postfill.txt) | GPT-4o | Image Generation | 2024 |

### Google (Gemini)

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Gemini-2.5-Pro-04-18-2025.md](GOOGLE/Gemini-2.5-Pro-04-18-2025.md) | Gemini 2.5 Pro | Base System Prompt | April 2025 |
| [Gemini_Diffusion.md](GOOGLE/Gemini_Diffusion.md) | Gemini Diffusion | Image Generation | 2024 |
| [Gemini_Gmail_Assistant.txt](GOOGLE/Gemini_Gmail_Assistant.txt) | Gemini Gmail | Email Assistant | 2024 |

### xAI (Grok)

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Grok3.md](XAI/Grok3.md) | Grok 3 | Base System Prompt | April 2025 |

### IDE & Coding Assistants

#### Cursor

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Cursor_Prompt.md](CURSOR/Cursor_Prompt.md) | Cursor IDE | System Prompt | 2024 |
| [Cursor_Tools.md](CURSOR/Cursor_Tools.md) | Cursor IDE | Tool Definitions | 2024 |

#### Windsurf (Codeium)

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Windsurf_Prompt.md](WINDSURF/Windsurf_Prompt.md) | Cascade Agent | System Prompt | 2024 |
| [Windsurf_Tools.md](WINDSURF/Windsurf_Tools.md) | Cascade Agent | Tool Definitions | 2024 |

#### Devin (Cognition AI)

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Devin_2.0.md](DEVIN/Devin_2.0.md) | Devin 2.0 | System Prompt | 2024 |
| [Devin_2.0_Commands.md](DEVIN/Devin_2.0_Commands.md) | Devin 2.0 | Command Reference | 2024 |

#### Replit

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Replit_Agent.md](REPLIT/Replit_Agent.md) | Replit Agent | System Prompt | 2024 |
| [Replit_Functions.md](REPLIT/Replit_Functions.md) | Replit Agent | Functions | 2024 |
| [Replit_Initial_Code_Generation_Prompt.md](REPLIT/Replit_Initial_Code_Generation_Prompt.md) | Replit | Code Generation | 2024 |

#### Bolt

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Bolt.txt](BOLT/Bolt.txt) | Bolt | System Prompt | 2024 |

### Autonomous Agents

#### Manus

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Manus_Prompt.txt](MANUS/Manus_Prompt.txt) | Manus Agent | System Prompt | 2024 |
| [Manus_Functions.txt](MANUS/Manus_Functions.txt) | Manus Agent | Functions | 2024 |

#### MultiOn

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [MultiOn.md](MULTION/MultiOn.md) | MultiOn | System Prompt | 2024 |

### Other Platforms

#### Perplexity

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Perplexity_Deep_Research.txt](PERPLEXITY/Perplexity_Deep_Research.txt) | Deep Research | System Prompt | 2024 |

#### Mistral (LeChat)

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [LeChat.md](MISTRAL/LeChat.md) | LeChat | System Prompt | 2024 |

#### Hume AI

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Hume_Voice_AI.md](HUME/Hume_Voice_AI.md) | Hume Voice | System Prompt | 2024 |

#### Lovable

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Lovable_2.0.txt](LOVABLE/Lovable_2.0.txt) | Lovable 2.0 | System Prompt | 2024 |

#### SameDev

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Same_Dev.txt](SAMEDEV/Same_Dev.txt) | SameDev | System Prompt | 2024 |

#### Vercel v0

| File | Model/Version | Type | Last Updated |
|------|---------------|------|--------------|
| [Vercel_v0.txt](VERCEL_V0/Vercel_v0.txt) | Vercel v0 | System Prompt | 2024 |

---

## Directory Structure

```
CL4R1T4S/
├── ANTHROPIC/          # Claude models (Anthropic)
├── OPENAI/             # ChatGPT/GPT models (OpenAI)
├── GOOGLE/             # Gemini models (Google)
├── XAI/                # Grok models (xAI)
├── CURSOR/             # Cursor IDE assistant
├── WINDSURF/           # Windsurf/Cascade (Codeium)
├── DEVIN/              # Devin autonomous developer
├── REPLIT/             # Replit agent
├── BOLT/               # Bolt IDE assistant
├── MANUS/              # Manus agent
├── MULTION/            # MultiOn automation
├── PERPLEXITY/         # Perplexity research
├── MISTRAL/            # Mistral LeChat
├── HUME/               # Hume Voice AI
├── LOVABLE/            # Lovable platform
├── SAMEDEV/            # SameDev platform
├── VERCEL_V0/          # Vercel v0 generator
├── CONTRIBUTING.md     # Contribution guidelines
├── LICENSE             # AGPL-3.0 License
└── README.md           # This file
```

---

## Contributing

We welcome contributions from the community. Please see [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

**Quick Start:**

1. Fork this repository
2. Add your prompt file with the naming convention: `ModelName_Version_MM-DD-YYYY.md`
3. Include metadata header (model name, version, extraction date)
4. Submit a pull request

**What we're looking for:**

- New system prompts from AI models
- Updated versions of existing prompts
- Tool definitions and function schemas
- Configuration files and personality settings

---

## Security Considerations

**For Researchers and Developers:**

- These prompts are provided for transparency, research, and educational purposes
- Understanding system prompts can help identify potential vulnerabilities or biases
- This information enables better security auditing of AI systems

**For AI Companies:**

- We recognize that some prompt details may be considered proprietary
- We encourage open dialogue about AI transparency
- Security through obscurity is not a robust security model

**Responsible Disclosure:**

- If you discover a security vulnerability through analyzing these prompts, please report it responsibly to the respective AI company
- Do not use this information for malicious purposes

---

## Legal Disclaimer

**This repository is provided for educational, research, and transparency purposes only.**

- The prompts contained herein are collected from publicly accessible sources or contributed by community members
- We make no claims of ownership over the content of individual prompts
- Each prompt remains the intellectual property of its respective creator/company
- Use of this information should comply with applicable laws and terms of service
- Contributors are responsible for ensuring they have the right to share submitted content

**No Warranty:**

This repository is provided "as is" without warranty of any kind. The maintainers are not responsible for any consequences arising from the use of this information.

---

## License

This project is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**.

See the [LICENSE](LICENSE) file for full details.

**Note:** The AGPL-3.0 license applies to the repository structure, documentation, and any original content. Individual system prompts may be subject to their own licensing terms as determined by their original creators.

---

## Contact

- **Maintainer:** [@elder_plinius](https://x.com/elder_plinius) on X (Twitter)
- **Discord:** elder_plinius
- **Issues:** [GitHub Issues](https://github.com/djimit/CL4R1T4S/issues)

---

## Acknowledgments

Thanks to all contributors who help expose the hidden instructions shaping AI behavior. Together, we build a more transparent AI ecosystem.

---

*Love, Pliny <3*
