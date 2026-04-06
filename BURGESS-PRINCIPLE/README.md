# The Burgess Principle — Human-Review Framework for AI Transparency

> **"Was a human member of the team able to personally review the specific facts of my specific case?"**

The Burgess Principle is a framework created by Lewis James Burgess (UK Certification Mark #UK00004343685). It provides a calm, binary human-review check to ensure that edge cases, ambiguities, and high-stakes decisions are flagged for a real person before proceeding.

This fits directly into CL4R1T4S's mission: if hidden system prompts shape how AI behaves without your knowledge, the Burgess Principle ensures that AI decisions which affect real people are never fully automated without human oversight.

**CL4R1T4S reveals what the AI is told. The Burgess Principle ensures a human checks what the AI does.**

---

## Table of Contents

- [What Is the Burgess Principle?](#what-is-the-burgess-principle)
- [Why It Belongs Here](#why-it-belongs-here)
- [Which Burgess Skill Should I Use?](#which-burgess-skill-should-i-use)
- [Available Burgess-Enhanced Skills](#available-burgess-enhanced-skills)
- [Design Principles](#design-principles)
- [Why This Liberates the AI](#why-this-liberates-the-ai)
- [Attribution](#attribution)
- [Disclaimer](#disclaimer)

---

## What Is the Burgess Principle?

At its core is one question:

> **"Was a human member of the team able to personally review the specific facts and implications of this for my individual situation?"**

If the answer is **no** — or there is any doubt — the matter is flagged for human review. That is the whole point: **see the human first**.

For the full framework: [github.com/ljbudgie/burgess-principle](https://github.com/ljbudgie/burgess-principle)

---

## Why It Belongs Here

CL4R1T4S exists because AI systems operate on hidden instructions that shape their behaviour — and the public deserves to see those instructions. The Burgess Principle addresses the other side of the same coin: even when you *can* see the instructions, you still need a way to ensure that AI decisions affecting real people get genuine human review.

Together, they form a complete transparency picture:
1. **CL4R1T4S** → See what the AI is told (input transparency)
2. **Burgess Principle** → Ensure a human checks what the AI does (output accountability)

---

## Which Burgess Skill Should I Use?

Not sure which skill fits your situation? Use this quick guide:

```
Is someone asking you to sign or agree to a document?
  └─ Yes → Contract Review

Do you need workplace or service adjustments for a disability or condition?
  └─ Yes → Reasonable Adjustments

Do you want to know what personal data an organisation holds about you?
  └─ Yes → DSAR Request

Do you want information held by a public body (government, council, NHS, police)?
  └─ Yes → FOI Request

Has an AI coding agent made changes that affect real people?
  └─ Yes → Coding Agent Review

Have bailiffs or enforcement agents contacted you about a debt?
  └─ Yes → Enforcement Agent Response

Has a media outlet published something inaccurate about you?
  └─ Yes → Media & Libel Review

Has your content been wrongfully taken down by a copyright or DMCA claim?
  └─ Yes → Copyright & DMCA Counter-Notice

Has your original music been wrongfully claimed by Content ID or a rights holder?
  └─ Yes → Music Copyright Dispute

Has a benefits decision (PIP, Universal Credit, ESA) not reflected your circumstances?
  └─ Yes → Benefits Claim Assistance

Is a medical device making automated decisions about your care?
  └─ Yes → Medical Device Review

Have you received a council tax demand or parking fine that feels wrong?
  └─ Yes → Council Tax & PCN Dispute

Has a Direct Debit been taken wrongfully and the bank refused a refund?
  └─ Yes → Direct Debit Refund

Has a decision been made about you that feels wrong, automated, or impersonal?
  └─ Yes → Human Review Request

None of the above, but you want a human to look at your specific case?
  └─ Yes → Human Review Request (it works for any situation)
```

---

## Available Burgess-Enhanced Skills

| Skill | Description |
|-------|-------------|
| [Benefits Claim Assistance](skills/benefits-claim-assistance/) | Challenge benefits decisions (PIP, Universal Credit, ESA) with human-review checkpoints. |
| [Coding Agent Review](skills/coding-agent-review/) | Human-review gate for AI coding agent outputs — flags changes affecting accessibility, privacy, security, and more. |
| [Contract Review](skills/contract-review/) | Clause-by-clause contract review with Burgess binary flagging for clauses needing human attention. |
| [Copyright & DMCA Counter-Notice](skills/copyright-dmca-counter-notice/) | Respond to wrongful copyright takedowns and DMCA notices with human-review requests. |
| [Council Tax & PCN Dispute](skills/council-tax-pcn-dispute/) | Challenge council tax demands and parking fines with human-review checkpoints. |
| [Direct Debit Refund](skills/direct-debit-refund/) | Challenge wrongful Direct Debit payments and bank refund decisions. |
| [DSAR Request](skills/dsar-request/) | Structured Data Subject Access Request generation with human-review checkpoints. |
| [Enforcement Agent Response](skills/enforcement-agent-response/) | Respond to bailiffs and enforcement agents with human-review requests and enforcement pause. |
| [FOI Request](skills/foi-request/) | Freedom of Information request generation for public bodies with human-review checkpoints. |
| [Human Review Request](skills/human-review-request/) | A general-purpose skill for requesting that a decision be reconsidered by a real person. |
| [Media & Libel Review](skills/media-libel-review/) | Respond to inaccurate media coverage with human-review requests for editorial accountability. |
| [Medical Device Review](skills/medical-device-review/) | Request human clinician review of algorithmic decisions in medical devices. |
| [Music Copyright Dispute](skills/music-copyright-dispute/) | Dispute wrongful Content ID claims, blocked monetisation, and royalty allocation errors. |
| [Reasonable Adjustments](skills/reasonable-adjustments/) | Templates and guided prompts for requesting reasonable adjustments from employers and service providers. |

---

## Design Principles

All Burgess-enhanced skills follow these principles:

- **Calm and respectful** — no aggressive language, no pressure, no rush.
- **Minimalist** — zero bloat, one purpose per skill.
- **Zero-energy** — designed for people with limited energy or hidden disabilities.
- **Human-first** — every edge case is an invitation to pause and involve a real person.
- **Privacy-first** — no data leaves the device unless explicitly required by the skill.

See [context/design-principles.md](context/design-principles.md) for detailed design guidance.

---

## Why This Liberates the AI

The Burgess layer does not restrict the AI — it frees it from a false choice between refusing entirely and processing blindly. By encoding a principled reason to pause, defer, and hand off, it transforms the AI into a tool that knows when to help, when to stop, and when to bring in a human.

See [context/why-this-liberates-the-ai.md](context/why-this-liberates-the-ai.md) for the full explanation.

---

## Attribution & Licensing

This toolkit is sourced from the [Advocate Companion](https://github.com/ljbudgie/advocate-companion) by Lewis James Burgess.

**The code is free.** All code, templates, and skill definitions in this directory are MIT-licensed — free to use, modify, and redistribute for any purpose, including commercial use.

**The name is not.** "Burgess Principle" and its UK Certification Mark (#UK00004343685) are protected intellectual property belonging to Lewis James Burgess. You may not use the name or mark to brand, market, or certify your own products or services without permission.

For trademark enquiries, contact [lewisburgess_1987@hotmail.co.uk](mailto:lewisburgess_1987@hotmail.co.uk).

---

## Disclaimer

This is a personal self-advocacy tool. The generated messages and templates are for informational and assistive purposes only. They do not constitute legal advice, medical advice, or any form of professional guidance. Please consult appropriate professionals for your specific needs.
