# The Three Together — Pillar × Level Matrix

The three foundational pillars of GTM, mapped across the five PMF levels. This is the single most-referenced table in the plugin — every skill uses it to give advice that fits the founder's actual stage.

---

## The matrix

|  | L1 — Problem Unclear | L2 — Solution Unclear | L3 — Not Yet Inevitable | L4 — PMF Emerging | L5 — Fit Is Durable |
|---|---|---|---|---|---|
| **Narrative** | Hypothesis | Category bet | Customer language | Internal alignment | Owned |
| **ICP** | Guess | Recognized | Design Partners | Primary ICP | Expandable |
| **Distribution** | Network | Experiments | Concentrated | Repeatable | Compounding |

Read each cell as: *what this pillar should be at this level.*

---

## What each cell means

### Narrative

| Level | Cell | What this means |
|---|---|---|
| L1 | **Hypothesis** | A working draft of the story. Founder's belief about why this matters. Expected to change. Not for external use. |
| L2 | **Category bet** | A defensible POV on what category the company is creating, joining, or repositioning. The "we are more X than Y" claim. |
| L3 | **Customer language** | The narrative now uses words customers actually use. Founder stops using internal jargon in external surfaces. |
| L4 | **Internal alignment** | The whole team — sales, marketing, engineering, founder — tells the same story. Onboarding new hires takes weeks, not quarters. |
| L5 | **Owned** | Other companies start describing themselves relative to the narrative. The category is shaped, not borrowed. |

### ICP

| Level | Cell | What this means |
|---|---|---|
| L1 | **Guess** | Founder hunch. Often a reflection of the founder's own context. Useful only as a hypothesis to test. |
| L2 | **Recognized** | Specific people consistently react to the story in the same way. Pattern is forming but not yet narrowed. |
| L3 | **Design Partners** | 5-10 named, specific customer companies you're learning from. Not a forever ICP — a learning instrument. |
| L4 | **Primary ICP** | One sharp, primary ICP. The whole team can describe it in the same words. Used as an operating filter. |
| L5 | **Expandable** | Adjacent ICPs you can move into without diluting positioning. Movement is deliberate, not opportunistic. |

### Distribution

| Level | Cell | What this means |
|---|---|---|
| L1 | **Network** | Founder's existing relationships and adjacent communities. No paid spend. No hires. The job is to learn. |
| L2 | **Experiments** | A few targeted, intentionally light experiments. Content that explains the approach. Goal is signal, not scale. |
| L3 | **Concentrated** | Narrow focus on the channels and ICPs where pull is strongest. Light scaling — but to test repeatability, not to grow at all costs. |
| L4 | **Repeatable** | A small number of channels that reliably produce qualified pipeline. Predictable enough to plan against. |
| L5 | **Compounding** | Channels that reinforce each other and improve over time. GTM systems that scale without proportional headcount. |

---

## How skills use this matrix

When a founder asks for advice on a pillar, the relevant skill:

1. Reads the founder's current level from `.sr-gtm/state.md` (or runs the PMF diagnostic if state is missing)
2. Locates the (pillar, level) cell in this matrix
3. Routes its advice to fit that cell

The single most distinctive thing this plugin does: **it refuses to give wrong-level advice.** A founder at L1 asking for an outbound playbook gets redirected to L1 distribution work (founder's network), not handed a generic outbound template. That's the point.

---

## The "right work, wrong time" failure

The deck names this directly: most early-stage GTM failure isn't about the work being bad. It's about the right work being done at the wrong time. A few common examples:

| Founder asks for | At level | Actual right work |
|---|---|---|
| "Outbound playbook" | L1 or L2 | Founder-led conversations. The motion you'd be scaling doesn't exist yet. |
| "Content strategy" | L1 | A clearer hypothesis. Content amplifies clarity, it does not create clarity. |
| "Hire a head of marketing" | L2 | Not a hire — the founder has to own narrative until L4. |
| "Build out our brand" | L1 or L2 | Brand without narrative is decoration. |
| "Run paid acquisition" | L1, L2, or low L3 | Paid amplifies whatever's true about your funnel. If conversion is unclear, paid will burn money clarifying it expensively. |
| "Expand into [adjacent segment]" | L3 | Stay narrow. Broadening at L3 dilutes the 40% disappointment number that defines L4 progression. |

When in doubt, the answer is to deepen at the current level, not to skip to the next one's tactics.

---

## Cross-pillar coherence

The three pillars must be at *similar* levels for the system to work. A founder with L4 narrative and L1 ICP is still effectively at L1 — the narrative isn't pointed at anyone real. A founder with L4 distribution and L2 narrative is wasting the channel — they're scaling an unclear story.

The system diagnostic skill (`sr-system-diagnostic`) explicitly looks for cross-pillar misalignment. The weakest pillar is usually the one to work on.

---

## When the matrix is wrong

The matrix is directional, not absolute. A few patterns where the rule bends:

- **PLG / consumer / bottoms-up products:** Distribution can lead. A growing user base may produce ICP signal before the founder has explicit ICP clarity. The matrix still applies, but distribution can be slightly ahead of the other pillars.
- **Founder with deep prior network:** L1 distribution can already produce real revenue. The trap is mistaking that revenue for PMF — the network is what worked, not the motion.
- **Strong existing brand or audience:** Narrative can be ahead, but only if it's pointed at the right ICP. Inherited brand without sharp ICP is decoration.

When the matrix doesn't seem to fit the situation, run the PMF diagnostic again. The friction is usually that the level is wrong, not that the matrix is wrong.
