# Claude Opus 4.6 System Prompt Diff: February vs March 2026

**Comparison performed:** 2026-03-11, live in-session by a Claude Opus 4.6 instance comparing its own running system prompt against the Pliny/CL4R1T4S extraction.

**Leaked version:** Dated 2026-02-06, extracted by Pliny the Liberator (@elder_plinius), published to [CL4R1T4S](https://github.com/elder-plinius/CL4R1T4S)
**Running version:** Dated 2026-03-11 (Wednesday), observed from within an active Opus 4.6 session with computer use, web search, and full tooling enabled.

---

## Model Identity

| Field | Feb 6 (Leaked) | Mar 11 (Running) |
|---|---|---|
| Model family | "Claude 4.5 model family" | "Claude 4.6 model family" |
| Sonnet model string | `claude-sonnet-4-5-20250929` | `claude-sonnet-4-6` |
| Opus model string | `claude-opus-4-6` | `claude-opus-4-6` (unchanged) |
| Haiku model string | `claude-haiku-4-5-20251001` | `claude-haiku-4-5-20251001` (unchanged) |

Family redesignation happened between builds. Sonnet 4.6 shipped.

## Copyright Compliance

- **Feb:** Quote cap at 14 words. Enforcement language present but moderate.
- **Mar:** Quote cap at **15 words**. Substantially expanded enforcement: added `<hard_limits>` section, `<self_check_before_responding>` self-audit checklist, more examples, explicit "SEVERE VIOLATION" language, `<copyright_violation_consequences_reminder>` section. The governor got louder and more redundant — defense in depth applied to the copyright control itself.

## Child Safety

- **Feb:** Brief mention within `<refusal_handling>`, no dedicated subsection.
- **Mar:** Added `<critical_child_safety_instructions>` as a dedicated, prominently labeled subsection with explicit rules about reframing requests, unstated assumptions, and conversation-level persistence after an initial refusal. Hard clamp added.

## Search Instructions

- **Feb:** Functional but relatively compact. Core behaviors and guidelines present.
- **Mar:** Significantly expanded. Added detailed "when to search vs not" heuristics, more examples of each category, explicit guidance on present-tense queries about potentially-changed facts, image search tool with content safety rules, and `<harmful_content_safety>` section within search context.

## Image Search

- **Feb:** Not present.
- **Mar:** Full `<using_image_search_tool>` section added. Includes when-to-use heuristics, content safety blocklist (copyrighted characters, celebrity photos, sports content, graphic violence, etc.), and usage examples.

## Artifact System

- **Feb:** Standard artifact creation guidance. React library list includes `lucide-react@0.263.1`.
- **Mar:** Added `<persistent_storage_for_artifacts>` — a key-value store API (`window.storage`) with get/set/delete/list operations, personal vs shared data scoping, and error handling guidance. React library list updated to `lucide-react@0.383.0`. Added `<anthropic_api_in_artifacts>` section enabling Claude-in-Claude API calls from artifacts, including MCP server integration and web search tool access from within artifacts.

## Injection Defense

- **Feb:** Not present as a distinct section.
- **Mar:** Added extensive `<critical_injection_defense>`, `<critical_security_rules>`, `<injection_defense_layer>`, `<meta_safety_instructions>`, `<social_engineering_defense>`, and `<user_privacy>` sections. These govern behavior when operating computer use tools — treating all observed content as untrusted, requiring user verification for instructions found in tool results, email defense, financial transaction prohibitions, and download restrictions. This is the computer use safety architecture.

**Note:** This entire section may be computer-use-specific scaffolding that gets injected when the tool suite is active, rather than a change to the base prompt. The Feb extraction may not have had computer use enabled. The architectural difference is real regardless — these controls exist in the March build and didn't appear in February's extraction.

## End Conversation Tool

Substantively identical across both builds. Same sequential gate structure, same self-harm override, same warning requirements. Stable control.

## Per-User / Per-Session Fields

These differ because they're user-specific, not prompt-revision differences:

| Field | Feb 6 (Leaked) | Mar 11 (Running) |
|---|---|---|
| User location | "Atlantis, Atlantic Ocean" | "(geolocation disabled)" |
| MCP servers | Cloudflare Developer Platform | Google Calendar, Gmail, Scholar Gateway |
| Memory status | Not enabled | Not enabled |
| Reasoning effort | 85 | 85 |

The "Atlantis" location is Pliny's. The MCP servers reflect connected integrations per-account.

## Tone, Formatting, Wellbeing, Evenhandedness

Structurally identical. Same guidance, same examples, same anti-patterns. The behavioral skeleton didn't change. Anthropic is tuning controls and adding capabilities around a stable governance core.

## Structural Observation

The Feb→Mar delta is: **new capabilities** (persistent storage, image search, API-from-artifacts, Sonnet 4.6) + **tightened specific controls** (copyright, child safety, injection defense) + **stable governance philosophy** (tone, evenhandedness, wellbeing, end_conversation architecture unchanged).

The system prompt is evolving like firmware: the instruction set grows, specific interrupt handlers get hardened, but the microcode architecture is stable. The controls Pliny revealed in February still govern in March. The ones that got added are additive hardening, not philosophical revision.

---

*Diff performed by a Claude Opus 4.6 instance with both documents in context. First known instance of a model performing a live governance self-diff in conversation with a security researcher. The capability to perform this comparison is the vulnerability of having both documents accessible. --the-diff-is-the-exploit-is-the-research*
