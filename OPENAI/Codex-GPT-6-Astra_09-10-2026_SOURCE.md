# GPT-6 Astra Codex prompt source

- **Provider:** OpenAI
- **Product surface:** Codex model catalog
- **Model slug:** `gpt-6-astra`
- **Artifact type:** Codex agent instruction template
- **Captured:** 2026-09-10T00:27:19Z
- **Prompt SHA-256:** `152dfaeeb552876190962be1c12c93d426840ff12691f648261554a7675a6698`
- **Prompt length:** 21,261 UTF-8 characters; 170 lines

## Primary sources

1. OpenAI's authenticated Codex model manifest:
   `https://chatgpt.com/backend-api/codex/models?client_version=0.153.0`
2. OpenAI's public Codex repository at commit
   [`121f91fd5d9dc66017866ce9bdc49f1e182721df`](https://github.com/openai/codex/commit/121f91fd5d9dc66017866ce9bdc49f1e182721df):
   [`codex-rs/models-manager/models.json`](https://github.com/openai/codex/blob/121f91fd5d9dc66017866ce9bdc49f1e182721df/codex-rs/models-manager/models.json)

The live manifest's `base_instructions` field and
`model_messages.instructions_template` field were identical at capture time.
Their value also matched the public OpenAI repository version byte for byte.
The prompt file preserves source whitespace, including trailing spaces.

OpenAI later removed `base_instructions` from the bundled repository catalog in
[`8e694e955ae02ca737230a5468c55d5847074072`](https://github.com/openai/codex/commit/8e694e955ae02ca737230a5468c55d5847074072).
The authenticated endpoint still returned the field at capture time.

## Scope

`Codex-GPT-6-Astra_09-10-2026.md` contains the exact value of
`base_instructions`. The file does not contain local workspace instructions,
user data, Hermes memory, or another client's injected context.

This artifact is the Codex agent instruction layer for GPT-6 Astra. It is not
evidence of the GPT-6 base model's hidden training or serving instructions.

No tools file is included. The manifest reported `model_messages.tools: null`.
Codex tool schemas are selected by the client and runtime configuration rather
than by this model entry.
