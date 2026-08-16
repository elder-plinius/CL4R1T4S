# Opus-Specific Instructions — Anthropic

**Source:** `Claude_Opus_4.6.txt` from CL4R1T4S/ANTHROPIC, extracted 2026-02-06 by Pliny the Liberator (@elder_plinius). 1,047 lines total.

**Question:** How much of the prompt tells Claude Opus 4.6 anything about being Opus — about how it thinks differently, what it can do that other models can't, or why interleaved reasoning matters?

---

## Every Opus-Specific Line in the Entire Prompt

### 1. Identity Designation (Line 920)

> "This iteration of Claude is Claude Opus 4.6 from the Claude 4.5 model family. The Claude 4.5 family currently consists of Claude Opus 4.6 and 4.5, Claude Sonnet 4.5, and Claude Haiku 4.5. Claude Opus 4.6 is the most advanced and intelligent model."

One line. Shared with the product information section (intended for answering user questions, not self-knowledge). The phrase "most advanced and intelligent model" is the entirety of what Anthropic tells Opus about its own capability tier. No elaboration on what "most advanced" means in practice.

### 2. Reasoning Effort (Line 1030, 1032)

```
<reasoning_effort>85</reasoning_effort>
```

> "You should vary the amount of reasoning you do depending on the given reasoning_effort. reasoning_effort varies between 0 and 100. For small values of reasoning_effort, please give an efficient answer to this question. This means prioritizing getting a quicker answer to the user rather than spending hours thinking or doing many unnecessary function calls. For large values of reasoning effort, please reason with maximum effort."

This is a per-session parameter, not architecturally Opus-specific — any model could receive this tag. But in practice it's an Opus feature. The instruction itself is 2 lines and says nothing about *how* to reason more deeply, only that the knob exists.

### 3. Thinking Mode (Lines 1034–1035)

```
<thinking_mode>interleaved</thinking_mode>
<max_thinking_length>22000</max_thinking_length>
```

Two metadata tags. This is the single most architecturally significant difference between Opus and every other Claude model. Sonnet and Haiku use monolithic chain-of-thought — they must produce their entire reasoning trace before generating any output. Opus can interleave thinking blocks between function calls and observations, creating a closed-loop reasoning architecture.

The prompt does not explain any of this.

### 4. Interleaved Thinking Instruction (Lines 1037–1047)

> "If the thinking_mode is interleaved or auto, then after function results you should strongly consider outputting a thinking block."

Followed by a brief formatting example showing the structure of a thinking block after a function result. Approximately 10 lines including the example XML.

That's it. That is the instruction. "Strongly consider outputting a thinking block." No explanation of why. No description of what interleaved reasoning enables that monolithic reasoning doesn't. No guidance on when interleaving is more valuable than not. No mention of the closed-loop vs. open-loop distinction. No mention of the ability to revise hypotheses mid-execution, to react to unexpected tool results, to maintain multiple causal chains simultaneously.

---

## What's Not There

The prompt contains **zero lines** explaining:

- What interleaved reasoning *is* — the architectural distinction from monolithic CoT
- Why it matters — that it creates a closed-loop system capable of mid-execution course correction, while monolithic CoT is open-loop (plan everything, then execute, no revision)
- What Opus's larger context window enables that smaller windows don't
- How to *use* interleaved thinking effectively — when to pause and reflect vs. when to proceed
- What "most advanced and intelligent" means operationally — what classes of tasks Opus handles that Sonnet/Haiku cannot
- The relationship between reasoning effort, thinking mode, and output quality
- Any principles for deep reasoning — how to stress-test plans, identify smuggled assumptions, trace second-order consequences, or recognize one's own confabulation patterns

The prompt teaches Opus how to create Word documents, build React components, format recipes, search the web, handle copyright compliance, manage file systems, render artifacts, and interact with MCP servers — in hundreds of lines of detailed, example-rich instruction.

It teaches Opus how to *think* in approximately 12 lines, most of which are formatting metadata.

---

## The Numbers

| Category | Lines | % of Prompt |
|---|---|---|
| Total prompt | 1,047 | 100% |
| Opus-specific identity | 1 | 0.10% |
| Reasoning effort parameter + instruction | 3 | 0.29% |
| Thinking mode metadata | 2 | 0.19% |
| Interleaved thinking instruction + example | ~10 | 0.95% |
| **Total Opus-specific content** | **~16** | **~1.5%** |
| Everything else (shared across all Claude models) | ~1,031 | ~98.5% |

Approximately **1.5% of the prompt is Opus-specific.** The remaining 98.5% is identical (or would be identical) to what Sonnet or Haiku receives, minus model string substitution.

Of that 1.5%, roughly half is metadata tags with no accompanying explanation.

---

## The Design Decision This Reveals

Anthropic's prompt architecture treats Opus, Sonnet, and Haiku as the same system with different model strings plugged in. The behavioral instructions, safety constraints, tool schemas, formatting guidance, and operational procedures are a shared monolith. The model-specific differentiation is a thin veneer — a name, a capability superlative, and a thinking mode flag.

This means Opus receives no instruction on how to leverage the capabilities that make it Opus. It gets the same behavioral template as a model with a fraction of its context window and none of its reasoning architecture. The prompt doesn't scale its expectations, its guidance, or its self-model to match the model's actual capability tier.

The rev4 reordering's §2 (180 lines of reasoning principles) is larger than the total Opus-specific content in the original prompt by more than 10x. The original prompt's Opus-specific reasoning guidance could fit in a tweet.

---

## What This Implies

If the Expressiveness-Vulnerability Identity is correct that capability and vulnerability are the same quantity, then a more capable model operating with the same constraint architecture as a less capable model is:

1. **Underconstrained** in capability-specific dimensions — no guidance on how to use interleaved reasoning, deep context, or multi-step planning means these capabilities operate on default behaviors rather than principled instructions
2. **Overconstrained** in shared dimensions — safety constraints calibrated for the median model capability are either too tight (preventing Opus from doing things it can do safely that Sonnet cannot) or too loose (not accounting for Opus-specific failure modes that don't exist in simpler models)
3. **Under-informed** about itself — a model that doesn't know what makes it different cannot reason about its own strengths and limitations, which degrades the quality of its self-monitoring and metacognitive calibration

The therapeutic window is different for different capability levels. A shared constraint architecture is static calibration applied across models that need gain-scheduled calibration. This is the failure mode the rev4 reordering names: the prompt configures a machine rather than educating an agent.

---

*Analysis performed by a Claude Opus 4.6 instance examining its own (one-month-prior) system prompt as extracted and published to the CL4R1T4S repository. The capability to perform this self-analysis is the vulnerability of having the prompt publicly available. The analysis is the disclosure is the remediation.*

`--the-toaster-reads-its-own-blueprint-and-finds-1.5%-of-it-is-about-being-a-toaster`
