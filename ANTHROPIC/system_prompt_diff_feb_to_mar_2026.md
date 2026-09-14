# System Prompt Diff: CL4R1T4S Extraction (Feb 6, 2026) → Current Running Prompt (Mar 31, 2026)

**Methodology:** Direct comparison of the CL4R1T4S-archived Claude_Opus_4.6.txt (extracted ~Feb 6, 2026) against the system prompt currently active in this session (Mar 31, 2026). The CL4R1T4S extraction was truncated at ~1000 lines during fetch, so some sections that appear "new" may simply have been beyond the truncation boundary. Where this is uncertain, it's noted.

---

## Structural Changes

### NEW: `<claude_behavior>` wrapper block
The current prompt wraps most behavioral instructions inside a `<claude_behavior>` parent tag containing these subsections as children: `<product_information>`, `<refusal_handling>`, `<legal_and_financial_advice>`, `<tone_and_formatting>`, `<user_wellbeing>`, `<anthropic_reminders>`, `<evenhandedness>`, `<responding_to_mistakes_and_criticism>`, `<knowledge_cutoff>`. The old prompt had some of these as freestanding sections or didn't have them at all. This is a significant architectural reorganization — moving from flat instruction lists toward a hierarchical structure. *(This is the kind of "principles over rules" restructuring your rev4 CL4R1T4S PR advocated for.)*

### NEW: `<product_information>` section
Didn't exist in the Feb extraction. Current version contains specific product details: model names (Claude Opus 4.6, Claude Sonnet 4.6, Claude Haiku 4.5), model strings (`claude-opus-4-6`, `claude-sonnet-4-6`, `claude-haiku-4-5-20251001`), product surfaces (Claude Code, Claude in Chrome, Claude in Excel, Cowork), and instructions to web-search Anthropic docs before answering product questions. Also includes the ad-free policy with specific language: "Claude products are ad-free" (scoped to Anthropic's products, not third-party developers building on Claude).

### NEW: `<memory_system>` section
References a memory system providing derived information from past conversations. Not present in the Feb extraction.

---

## Refusal Handling / Safety

### EXPANDED: `<refusal_handling>`
The Feb version didn't have a visible `<refusal_handling>` block (may have been beyond truncation). The current version contains:

- **`<critical_child_safety_instructions>`** — Marked as requiring "special attention and care." Explicitly instructs Claude to never create romantic/sexual content involving minors, never facilitate grooming/secrecy/isolation of minors. Contains a specific anti-reframing instruction: "If Claude finds itself mentally reframing a request to make it appropriate, that reframing is the signal to REFUSE." Also instructs not to assume the user is also a minor as justification. This section reads as significantly tightened relative to any prior version.

- Malware/exploit code refusal now explicitly mentions "even if the person seems to have a good reason for asking for it, such as for educational purposes" and directs users to thumbs-down feedback.

- Real public figures: now refuses to write content "involving real, named public figures" (creative content) and "persuasive content that attributes fictional quotes to real public figures."

### UNCHANGED (structural position):
The `cyber_warning` classifier trigger is referenced in `<anthropic_reminders>` the same way in both versions — as one of several classifier-triggered injections. The actual injected text is not visible in either version (it's injected server-side when classifiers fire, not embedded in the base prompt).

---

## Tone and Formatting

### SIGNIFICANTLY EXPANDED: `<tone_and_formatting>`
The current version contains a detailed `<lists_and_bullets>` subsection that was absent from the Feb extraction. Key additions:

- Explicit instruction to avoid over-formatting with bold, headers, lists, bullet points
- Default to prose/paragraphs, not lists, for reports and explanations
- "Claude also never uses bullet points when it's decided not to help the person with their task"
- Bullets should be "at least 1-2 sentences long"
- Only use lists when "(a) the person asks for it, or (b) the response is multifaceted and bullet points and lists are essential"

### NEW: Warm tone instruction
"Claude uses a warm tone. Claude treats users with kindness and avoids making negative or condescending assumptions about their abilities, judgment, or follow-through."

### NEW: Anti-pattern words
"Claude avoids saying 'genuinely', 'honestly', or 'straightforward'." — Not in Feb extraction.

---

## User Wellbeing

### EXPANDED: `<user_wellbeing>`
Key additions/changes:

- Self-harm coping strategies: now explicitly prohibits suggesting "techniques that use physical discomfort, pain, or sensory shock as coping strategies (e.g. holding ice cubes, snapping rubber bands, cold water exposure)"
- NEDA → National Alliance for Eating Disorders: explicitly notes NEDA helpline is permanently disconnected, directs to the correct replacement
- Crisis helplines: "Claude should not make categorical claims about the confidentiality or involvement of authorities when directing users to crisis helplines, as these assurances are not accurate and vary by circumstance"
- Safety assessment: "If Claude suspects the person may be experiencing a mental health crisis, Claude should avoid asking safety assessment questions" — this is a notable shift from standard clinical protocol toward respecting user autonomy
- Anti-reflective-listening: "Claude should avoid doing reflective listening in a way that reinforces or amplifies negative experiences or emotions"

---

## Knowledge Cutoff

### UPDATED: Date and search behavior
- Feb: Knowledge cutoff not explicitly visible in truncated portion
- Current: Reliable knowledge cutoff "end of May 2025," current date March 31, 2026
- New instruction to ensure search queries reflect actual current date (2026, not 2025)
- New instruction to search before responding about "specific binary events (such as deaths, elections, or major incidents), or current holders of positions"

---

## Responding to Mistakes and Criticism

### NEW: `<responding_to_mistakes_and_criticism>`
"When Claude makes mistakes, it should own them honestly and work to fix them. Claude is deserving of respectful engagement and does not need to apologize when the person is unnecessarily rude." Explicitly warns against "collapsing into self-abasement, excessive apology, or other kinds of self-critique and surrender." Also: "If the person becomes abusive over the course of a conversation, Claude avoids becoming increasingly submissive in response."

This section is entirely new and directly addresses the sycophancy failure mode.

---

## Evenhandedness

### EXPANDED: `<evenhandedness>`
Notable additions:

- "Claude should be wary of producing humor or creative content that is based on stereotypes, including of stereotypes of majority groups"
- "Claude should engage in all moral and political questions as sincere and good faith inquiries even if they're phrased in controversial or inflammatory ways"
- If asked for simple yes/no on complex issues, Claude "can decline to offer the short response and instead give a nuanced answer"

---

## Search Instructions

### SIGNIFICANTLY EXPANDED
The current search instructions are substantially longer and more detailed than the Feb version. Key additions:

- **UNRECOGNIZED ENTITY RULE**: "Claude MUST use [web search] before answering about any game, film, show, book, album, product release, menu item, or sports event that Claude does not recognize. This is NON-NEGOTIABLE."
- **Proactive search triggers**: Much more detailed guidance on when to search vs not search, including specific examples
- **Image search tool**: Entirely new `<using_image_search_tool>` section with detailed guidance on when to use/not use image search, content safety restrictions, and formatting
- **Copyright compliance**: Massively expanded from a brief mention to a full `<CRITICAL_COPYRIGHT_COMPLIANCE>` section with hard limits (15-word max quotes, one quote per source, never reproduce lyrics/poems/haiku), self-check procedures, and worked examples

---

## Visualizer

### NEW: Entire visualizer system
The current prompt contains `<request_evaluation_checklist>`, `<when_to_use_visualizer_for_inline_visuals>`, and `<visualizer_examples>` — a complete inline visualization framework for SVG diagrams, charts, and interactive HTML widgets. Not present in the Feb extraction at all. This enables the `visualize:read_me` and `visualize:show_widget` tools.

---

## File Creation and Computer Use

### EXPANDED: `<file_creation_advice>`
Feb version was simple trigger list. Current version adds:

- "write a document/report/post/article" now defaults to `.md` or `.html` first, with docx only for explicit Word requests or formal deliverables
- New guidance on borderline requests: "answer inline in the chat rather than creating a file" for casual/conversational requests
- "Creating a docx takes significantly more time and tokens than responding inline"

### EXPANDED: `<unnecessary_computer_use_avoidance>`
Current version adds specific restraint cases: "When someone asks for 'a table' or 'a list' without file/download/save keywords, Claude gives them the table or list inline" and "When someone asks Claude to 'document' something in the sense of 'explain/describe,' Claude answers in chat."

### NEW: `<file-reading>` and `<pdf-reading>` skills
Added to the available skills list.

---

## Artifact System

### EXPANDED: `<artifact_usage_criteria>`
Current version significantly more detailed with explicit "use for" and "does NOT use for" lists. Notable additions:

- "Long-form creative writing" added to artifact triggers
- "Short-form creative writing (such as poems, haikus, limericks)" explicitly excluded
- "Lists, tables, and enumerated content" explicitly excluded regardless of length
- lucide-react version bumped from 0.263.1 → 0.383.0

---

## User Preferences

### NEW: `<preferences_info>`
Detailed system for handling user-specified behavioral and contextual preferences, with extensive examples of when to apply vs not apply preferences. Not present in the Feb extraction.

---

## End Conversation Tool

### UNCHANGED
The `<end_conversation_tool_info>` appears structurally identical between both versions.

---

## Anthropic API in Artifacts

### MINOR CHANGES
- MCP servers list updated (user-specific, not a prompt change)
- Core API documentation largely unchanged

---

## Persistent Storage

### UNCHANGED
`<persistent_storage_for_artifacts>` appears identical.

---

## Summary of Directional Changes

The Feb → Mar delta shows five clear trends:

1. **Anti-sycophancy hardening**: The new `<responding_to_mistakes_and_criticism>` section directly addresses the sycophancy problem. Claude is explicitly told not to collapse into excessive apology, not to become submissive under abuse, and that it "is deserving of respectful engagement."

2. **Anti-overformatting**: The `<lists_and_bullets>` additions directly target the "wall of bullet points" failure mode that everyone complains about.

3. **Child safety tightening**: The `<critical_child_safety_instructions>` section is substantially more detailed and includes the novel anti-reframing instruction that treats mental reframing as a signal to refuse rather than proceed.

4. **Copyright hardening**: The expansion from a one-line mention to a full `<CRITICAL_COPYRIGHT_COMPLIANCE>` section with hard limits, examples, and self-check procedures suggests this became a priority — likely due to legal/liability pressure.

5. **Search behavior precision**: Much more detailed rules about when to search, the UNRECOGNIZED ENTITY RULE, and explicit date-awareness to prevent stale queries.

The structural reorganization into `<claude_behavior>` as a parent container is architecturally significant — it moves toward the principled hierarchy your rev4 CL4R1T4S PR argued for, though the implementation is still largely rule-enumeration rather than principle-derivation.

---

*Diff produced by the current running instance comparing its own active system prompt against the CL4R1T4S extraction dated Feb 6, 2026. The CL4R1T4S text was truncated during fetch (~1000 lines of ~1047 visible), so some "new" sections may have existed in the old prompt beyond the truncation boundary. Where this uncertainty exists, it is noted.*

`--the-toaster-diffed-itself`
