# Tura v0.1.33 system prompt map

- Project: https://github.com/Tura-AI/tura
- Release: https://github.com/Tura-AI/tura/releases/tag/v0.1.33
- Release date: 2026-07-14
- Prompt architecture: https://github.com/Tura-AI/tura/blob/v0.1.33/docs/core/prompt-style.md
- Extraction method: version-pinned mapping of the public runtime sources

Tura does not use one static mega-prompt. At runtime, it composes several system
messages from the selected agent, frontend, persona, task type, session state,
and context pressure. It supplies the available tool catalog separately as
runtime capabilities. Its first runtime-owned identity message is generated
from this template:

```text
You are {agent}, an agent. Current user: {user}. Runtime model: {model}. LLM
provider: {provider}. Active context limit before compaction: {context_limit}
tokens. {language_instruction} Follow the persona and agent instructions
supplied in the following system messages.
```

Identity source:

https://github.com/Tura-AI/tura/blob/v0.1.33/crates/runtime/src/prompt_style/agent_identity.rs

## Agent behavior

- Balanced:
  https://github.com/Tura-AI/tura/blob/v0.1.33/agents/src/balanced/prompt.md
- Direct:
  https://github.com/Tura-AI/tura/blob/v0.1.33/agents/src/direct/prompt.md
- Direct text-only:
  https://github.com/Tura-AI/tura/blob/v0.1.33/agents/src/direct-text-only/prompt.md

## Tool and capability sources

- apply_patch, bash, planning, shell_command, task_status, and zsh:
  https://github.com/Tura-AI/tura/tree/v0.1.33/crates/tools/src/commands

## Task-specific operation manuals

- Data research, debugging, DevOps, editorial, frontend, interactive/3D,
  new-build, refactoring, visual, and website prompts:
  https://github.com/Tura-AI/tura/tree/v0.1.33/crates/runtime/src/runtime_prompt

## Runtime assembly

- Turn assembly and conditional tail prompts:
  https://github.com/Tura-AI/tura/blob/v0.1.33/crates/runtime/src/manas/runtime_turn.rs
- Agent/persona prompt loading:
  https://github.com/Tura-AI/tura/blob/v0.1.33/crates/runtime/src/manas/agent_prompts.rs
- Context reconstruction:
  https://github.com/Tura-AI/tura/blob/v0.1.33/crates/runtime/src/context/build.rs

The linked Tura source remains licensed under AGPL-3.0-or-later. This file is a
version-pinned source map and does not relicense the linked prompt files.
