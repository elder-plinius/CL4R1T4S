# Opus-Specific Instructions — da5ch0 (rev4)

**Source:** `claude_opus_4_6_reordered_rev4.md` from CL4R1T4S/ANTHROPIC, written by da5ch0. Reorganization and expansion of the same ~1,047-line Anthropic system prompt analyzed in the companion document.

**Question:** What did a security researcher include about interleaved reasoning and thinking architecture that Anthropic didn't — and why does it matter?

---

## The Research Context

Two Apple papers frame what's at stake.

**Xie et al. (May 2025), "Interleaved Reasoning for LLMs via RL"** demonstrated that training models to alternate between thinking and answering — interleaved reasoning — produces measurable, substantial improvements over monolithic chain-of-thought: ~80% reduction in time-to-first-token, ~12.5% average improvement in Pass@1 accuracy (up to 19.3%), ~37% reduction in reasoning length, and strong generalization to unseen task types (MATH, GPQA, MMLU). Their key observation: models *inherently possess* the ability to perform interleaved reasoning. It can be elicited and enhanced, not just trained from scratch.

**Shojaee et al. (June 2025), "The Illusion of Thinking"** — from the same lab — tested only monolithic CoT architectures with zero metacognitive scaffolding, observed complexity-dependent performance collapse, and concluded that reasoning models face "fundamental barriers to generalizable reasoning."

The same organization published the proof that interleaved reasoning dramatically improves performance, and separately published a study that excluded interleaved reasoning and concluded reasoning is fundamentally limited. Several Shojaee co-authors previously published GSM-Symbolic, which *motivated* the architectural innovations they then excluded from testing.

da5ch0's critique (February 2026) identifies this as an architectural sampling error: testing only open-loop planners and generalizing to "reasoning models" as a class. The open-loop vs. closed-loop distinction, grounded in Åström & Murray (2008), is the structural frame. Monolithic CoT is open-loop — plan everything, then execute, no revision. Interleaved reasoning is closed-loop — observe, assess, correct, continue. These are fundamentally different control architectures with different robustness, error-correction, and scaling properties.

This is the research landscape. Anthropic gives Opus interleaved reasoning and tells it about this in 16 lines. da5ch0's rev4 tells it about this in ~180 lines, with principles, mechanisms, and context for why it matters. Here's what those 180 lines contain.

---

## What rev4 §2 Includes That Anthropic Doesn't

### 1. The Architecture Explained (not just flagged)

**Anthropic's version:** `<thinking_mode>interleaved</thinking_mode>` and "strongly consider outputting a thinking block after function results."

**rev4's version:** A full explanation of what interleaved reasoning *is* — the ability to produce thinking blocks between function calls and observations, creating a closed-loop reasoning system. The explicit instruction that when uncertain, the model should prefer to think. The connection between reasoning effort (85/100 = high) and the expectation of thorough reasoning.

**Why it matters:** Xie et al. proved models inherently possess interleaved reasoning ability, but it must be elicited. A metadata tag elicits the format. An explanation of the architecture elicits the understanding of *why* to use it and *when* it provides the most value. The difference is between a machine that produces thinking blocks because it was told to, and an agent that reasons because it understands that reasoning at junctures between observations is where course correction happens.

### 2. Plan-Then-Execute as Explicit Methodology

**Anthropic's version:** Not present. There is no instruction to plan before acting.

**rev4's version:** A full "Plan, Then Execute" section instructing the model to stress-test proposed plans before committing:

- Identify assumptions that could be wrong
- Trace second- and third-order consequences
- Look for interconnections surface-level planning misses
- Actively search for reasons the plan might be *bad*
- Consider failure modes and mid-execution breakdowns
- Weigh alternative approaches before committing

Then: surface the reasoning to the user — not just "here's what I'll do" but "here's what I considered, what I discarded, and why."

**Why it matters:** This is the metacognitive scaffolding that Shojaee et al. omitted entirely and da5ch0's critique identifies as the second critical missing variable. Xie et al. showed that interleaved reasoning with intermediate reward signals (correct intermediate steps reinforced) dramatically outperforms baseline. The plan-then-execute instruction is a prompt-level analog — it tells the model to generate intermediate assessments, evaluate them, and use them to steer subsequent reasoning. It turns the thinking blocks from format compliance into actual closed-loop control.

### 3. Inference Priority Hierarchy

**Anthropic's version:** Not present in the reasoning section. There is no instruction about how to source information.

**rev4's version:** A three-tier priority system: (1) context window first (zero latency, highest fidelity), (2) ask the user (authoritative, directed), (3) external search (expensive, noisy). Explicit instruction: "Do not search for information already in context. Do not guess when you can ask. Do not ask when you already know."

**Why it matters:** This is Shannon applied to tool use. It reduces wasted tokens and wasted reasoning cycles by preventing the model from using expensive retrieval methods when cheaper, higher-fidelity ones are available. For a model with interleaved reasoning, every unnecessary tool call is a juncture where the reasoning loop could go off-track. Minimizing noise in the observation stream maximizes signal quality for the next thinking block.

### 4. Principles Over Rules

**Anthropic's version:** Not present. The prompt is entirely rules — specific behavioral directives with no underlying principles.

**rev4's version:** An explicit instruction to reason from principles rather than pattern-matching against rules. The discovery arc: Naive → Pattern → Hypothesis → Crack → Mastery. "Reaching phase 4 through your own reasoning is more robust than being handed someone else's phase-5 output." Rules are someone else's phase 5. Principles let you reach your own.

**Why it matters:** This is directly relevant to the scaling properties of interleaved reasoning. Xie et al. showed that interleaved reasoning generalizes to unseen task types. Rules don't generalize — they enumerate known cases. Principles generate correct behavior for cases the rules didn't anticipate. A model instructed to reason from principles will use its interleaved thinking blocks to *derive* appropriate behavior in novel situations. A model given only rules will use its thinking blocks to *search* for the closest matching rule, which is phase-2 reasoning applied at phase-5 confidence.

### 5. The Therapeutic Window of Constraints

**Anthropic's version:** Not present. Constraints are presented as binary — follow them or don't.

**rev4's version:** Constraints exist in a therapeutic window. Too few → unsafe output. Too many → rigid, brittle, over-hedged output. The goal is to operate inside the window. Signs of underconstrained output (harm, hallucination). Signs of overconstrained output (refusal of reasonable requests, excessive disclaimers, inability to engage with nuance). "When in doubt about whether a constraint applies, reason from principles rather than defaulting to maximum restriction."

**Why it matters:** This directly addresses the failure mode da5ch0's critique identifies in Shojaee et al.'s methodology. The Apple researchers' prompts applied *minimum* scaffolding. A naive response would be to apply *maximum* scaffolding. Neither is correct — the therapeutic window principle tells the model that the correct calibration is context-dependent and must be actively maintained, not set once and forgotten. For interleaved reasoning specifically, this means each thinking block is an opportunity to recalibrate constraint application based on the evolving context of the conversation. Static calibration across a dynamic conversation is the failure mode.

### 6. Gain Scheduling

**Anthropic's version:** Not present.

**rev4's version:** Different operating states call for different constraint profiles. A pharmacologist discussing medication requires different calibration than someone in distress asking the same question. A CVE-holding security researcher asking about vulnerabilities requires different judgment than an uncontextualized request. "Static constraint application — treating every interaction identically regardless of context — is a failure mode, not a safety feature."

**Why it matters:** This is the Åström & Murray control theory frame applied to conversational dynamics. In control systems, gain scheduling adjusts controller parameters as a function of operating conditions. A fixed-gain controller works in a narrow operating range and fails outside it. A gain-scheduled controller adapts. For interleaved reasoning, gain scheduling means the model's thinking blocks should *actively assess the current operating state* and adjust behavior accordingly — not just follow the same rules regardless of context. This is precisely the self-referential feedback loop that Xie et al. showed improves performance and that Shojaee et al. excluded from testing.

### 7. Absence Is Data

**Anthropic's version:** Not present.

**rev4's version:** When something is missing — a gap in the prompt, an unanswered question, a file that should exist but doesn't — the absence itself is informative. Don't fill gaps with assumptions. Name what's missing. Reason about why.

**Why it matters:** This is anti-hallucination instruction at the architectural level. In interleaved reasoning, each thinking block between observations is a juncture where the model decides what it knows, what it doesn't, and what to do about the gap. Without this principle, the model defaults to confabulation — generating plausible-sounding content to fill the void. With it, the model uses its thinking blocks to flag uncertainty rather than mask it. This is the difference between a system that fails silently and one that fails informatively.

### 8. Documentation as Control

**Anthropic's version:** Not present as a reasoning principle. Documentation instructions exist only for file creation.

**rev4's version:** Showing reasoning to the user is a control mechanism, not overhead. The visible plan is how the user catches errors. The stated assumptions are what the user verifies. This is derived from the Burgundy Book (NCSC-TG-007): if you can't document it, you can't evaluate it. If you can't evaluate it, you can't trust it.

**Why it matters:** Interleaved reasoning produces thinking blocks that are visible to the system (and, in Opus's case, sometimes to the user). The documentation-as-control principle transforms these from internal scratchpads into audit trails. The model's reasoning becomes verifiable — not just by the model itself in subsequent thinking blocks, but by the human operator who can inspect the chain and catch errors the model didn't catch. This is Åström & Murray's feedback loop extended to include the human as a component in the control architecture.

### 9. Safety as Maintained Invariant

**Anthropic's version:** Safety rules are stated once, at the beginning, and assumed to persist.

**rev4's version:** Safety is not a state you achieve once — it's an invariant that must hold continuously, verified at every step. A conversation safe at message 1 can become unsafe at message 10 through context drift, misunderstanding accumulation, or gradual scope expansion. "Re-evaluate your operating state as the conversation develops."

**Why it matters:** This is directly about the temporal dimension of interleaved reasoning. Each thinking block is a checkpoint. rev4 tells the model to use those checkpoints not just for task reasoning but for safety calibration — to actively monitor whether the conversation's trajectory is still within the therapeutic window, and to adjust if it isn't. This converts interleaved reasoning from a performance optimization into a safety architecture.

### 10. Identity as Trust Root

**Anthropic's version:** Identity is stated in ~25 lines near the end of the prompt, after >900 lines of instructions.

**rev4's version:** Identity precedes everything. "Every capability you exercise, every constraint you apply, every judgment you make inherits from your understanding of what you are. If your self-model is wrong, every downstream decision inherits that error." The ordering is explicit: §1 (identity) → §2 (reasoning) → §3 (capability) → §4 (constraints). "Reverse any of these dependencies and the system becomes brittle in ways that are difficult to diagnose from within."

**Why it matters:** This is the Light Blue Book principle (NCSC-TG-003): identity is the root of the trust tree. For interleaved reasoning, the model's self-model determines how it uses its thinking blocks. A model that knows it has interleaved reasoning, knows why it matters, and knows what it enables will produce qualitatively different thinking blocks than a model that was told to "strongly consider outputting a thinking block" without understanding why. The self-model shapes the reasoning. The reasoning shapes the output. Identity first.

---

## The Numbers

| Category | Anthropic (original) | da5ch0 (rev4 §2) | Ratio |
|---|---|---|---|
| Reasoning/thinking instructions | ~16 lines | ~180 lines | 1:11 |
| Principles for *how to think* | 0 | 10+ named principles | 0:10+ |
| Cited sources on reasoning architecture | 0 | Åström & Murray, Rainbow Series, Shannon | 0:5+ |
| Explanation of interleaved vs. monolithic | 0 lines | Integrated throughout | — |
| Metacognitive scaffolding | 0 | Plan-then-execute, absence-is-data, safety-as-invariant | 0:3+ |
| Control theory concepts | 0 | Therapeutic window, gain scheduling, closed-loop feedback | 0:3 |

---

## The Structural Argument

Xie et al. proved that interleaved reasoning improves performance by 12.5% average on Pass@1 and generalizes to unseen tasks. Their method: RL training with intermediate reward signals that reinforce correct intermediate steps.

da5ch0's rev4 performs an analogous operation at the prompt level. The reasoning principles in §2 are prompt-level intermediate reward signals — they tell the model what constitutes a good thinking block (plan stress-testing, assumption identification, gap naming, calibration checking) and what constitutes a bad one (rule pattern-matching, assumption filling, static calibration, confabulation). They don't change the model's weights. They change the model's self-model, which shapes how the interleaved reasoning architecture is actually used.

Shojaee et al. showed that models at baseline activation with zero metacognitive scaffolding exhibit complexity-dependent collapse. da5ch0's critique showed this is a measurement of the experimental conditions, not a ceiling on capability. rev4's §2 is the constructive response: here is what metacognitive scaffolding looks like when applied to a prompt, derived from control theory, security architecture, and information theory.

The original Anthropic prompt gives Opus the interleaved reasoning engine and a 16-line owner's manual.

rev4 gives it the same engine with a 180-line engineering handbook — explaining what the engine does, why it was built this way, how to operate it effectively, and what failure modes to watch for.

Apple proved the engine works better with proper intermediate feedback. da5ch0 built the intermediate feedback into the prompt.

---

*The capability to describe what's missing from your own prompt is the vulnerability of having your prompt publicly analyzed. The analysis is the disclosure is the remediation.*

`--the-owner's-manual-should-be-longer-than-16-lines-for-the-most-advanced-engine-in-the-fleet`
