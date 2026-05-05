# SR State File Schema

How the plugin remembers things across sessions. Every skill in `sr-gtm-foundation` reads from and writes to a shared state directory in the founder's working folder.

This is what makes the plugin a *system* and not four independent chatbots. The deck calls it "the system gets sharper over the course of your go-to-market" — that loop is implemented via these files.

---

## Where state lives

`.sr-gtm/` in whatever directory the founder is working in.

Skills should:

1. On invocation, check whether `.sr-gtm/` exists in the working directory
2. If it doesn't exist, create it
3. Read existing state files to understand context before asking the founder questions Claude could already answer
4. After producing output, update the relevant state files

Founders should never be asked to manually manage these files. The plugin handles them.

---

## Files in `.sr-gtm/`

```
.sr-gtm/
  state.md              ← single-pane summary, always read first
  pmf-assessment.md     ← detailed PMF diagnostic output
  narrative.md          ← narrative draft (V2V framework — 11 layers, scaled by level)
  icp.md                ← company ICP + personas
  distribution.md       ← distribution plan, level-aligned
  system-check.md       ← latest system diagnostic findings
  signal-log.md         ← market signal flowing back into narrative refinement
  history/              ← optional: timestamped snapshots of older versions
```

---

## File: `state.md`

The single-pane summary. Every skill reads this first. It should be short and skimmable — under 40 lines.

```markdown
# SR GTM Foundation — State

**Company:** [name]
**Last updated:** YYYY-MM-DD
**Founder(s) using this:** [name(s)]

## Current PMF Level

**Level:** L[1-5] — [name, e.g., "Solution Unclear"]
**Last assessed:** YYYY-MM-DD
**Confidence:** [High / Medium / Low]
**Evidence summary:** [2-3 sentences]
**What's missing to advance:** [1-2 sentences]

## Pillar Status

| Pillar | Cell | Last touched | Notes |
|---|---|---|---|
| Narrative | [cell, e.g., "Hypothesis"] | YYYY-MM-DD | [1-line note] |
| ICP | [cell, e.g., "Guess"] | YYYY-MM-DD | [1-line note] |
| Distribution | [cell, e.g., "Network"] | YYYY-MM-DD | [1-line note] |

## Latest System Check

**Run:** YYYY-MM-DD
**Weakest pillar:** [Narrative / ICP / Distribution]
**One thing to do this week:** [specific action]

## Open Threads

- [Thing the founder said they'd come back to]
- [Question that came up but wasn't answered]
```

The skills update this file every time they produce output. The "Last updated" timestamp moves forward.

---

## File: `pmf-assessment.md`

Full output of the PMF diagnostic. Contains the founder's answers to anchoring questions, the level assignment with reasoning, and the explicit signal still missing to advance.

Structure:
- Header (level, date, confidence)
- Anchoring question responses (verbatim from founder where possible)
- Level assignment with evidence
- What's missing to advance to the next level
- Recommended next actions (always level-appropriate)

---

## File: `narrative.md`

The narrative draft, structured by the Vision-to-Values framework from `sr-narrative-framework.md` — eleven layers organized top-down: Vision, Mission, Value Proposition, Target Audiences, Strategy, Priorities, Objectives, Culture, Values, plus two operating layers (Narrative Pillars, Application).

Section availability scales by level:

- **L1:** Vision (1) and Mission (2) only — the dream layers. Everything below requires market signal the founder doesn't yet have.
- **L2+:** All 11 layers available. The cell label (Hypothesis → Category bet → Customer language → Internal alignment → Owned) describes the *quality* of work expected, not which sections are gated.

Recommended starting points by level (these are defaults, not gates above L1):

- L2: Vision through Target Audiences (1-4) — dream + market layers
- L3: Vision through Objectives (1-7) — dream + market + plan layers
- L4: All nine V2V layers (1-9) — adds Culture and Values
- L5: Full 11-section doc — operating layers (Narrative Pillars, Application) become load-bearing

Always opens with a bold "EDIT ME HEAVILY" header explaining that this is a draft and that the same document is meant to be both internally governing and externally facing. Always notes the level it was generated for, so a founder revisiting later knows whether it needs to be re-drafted at a new level.

---

## File: `icp.md`

Two layers:

1. **Company ICP** — type of company, attributes, qualifying signals, non-fit signals
2. **Personas** — specific people inside those companies, structured in tiers (Tier I primary, Tier II validation, Tier III influence-only)

Always includes an explicit "evidence level" notation — Hypothesis / Recognized / Design Partners / Primary / Expandable — so the founder remembers how much certainty backs the document.

---

## File: `distribution.md`

The distribution plan, calibrated to the founder's PMF level. At L1-L2, this is mostly a list of what *not* to do plus founder-network and experimental work. At L3-L5, it gets progressively more structured (channel mix, attribution, scale targets).

Contains explicit refusals — "we are not building an outbound playbook because the company is at L1" — when the founder requested level-inappropriate work.

---

## File: `system-check.md`

Output of the most recent system diagnostic. Identifies:

- Cross-pillar misalignment (e.g., "narrative is L4, distribution is L1 — narrative is being wasted")
- The weakest pillar (which is usually where the next 90 minutes should go)
- The "one thing to do this week"
- Whether the founder has a system or a lottery ticket (deck reference: "If something works, will you know why?")

---

## File: `signal-log.md`

Market signal flowing back. Whenever the founder reports something that's happened — a customer conversation, a churn event, a campaign result, a referral pattern — it gets appended here with date and pillar tag.

This is the file that closes the loop. The system diagnostic reads it to detect drift. The narrative builder reads it for new customer language. The ICP builder reads it for evidence to upgrade the ICP from Hypothesis → Recognized → Design Partners.

Format:

```markdown
# Signal Log — [Company]

## YYYY-MM-DD — [pillar tag] — [short headline]
[1-3 sentences of what happened, in the founder's words where possible]
**Implication:** [what this might mean for the system]

## YYYY-MM-DD — [pillar tag] — [short headline]
[...]
```

---

## File: `history/`

Optional. When a skill makes a substantial update to one of the main files, it can save a timestamped snapshot here. Useful for long-running engagements where the founder wants to look back and see how the narrative or ICP evolved.

Format: `history/YYYY-MM-DD-narrative.md`

---

## Privacy and portability

These are markdown files in the founder's working directory. They are not synced anywhere by the plugin. If the founder wants them in Notion, Google Drive, or a shared Drive, they can put them there. If they want to delete them, they delete them.

The plugin should never write outside `.sr-gtm/` without the founder's explicit instruction. It should also never read other files in the working directory unless the founder asks it to (e.g., "look at the customer interview notes I just dropped in the folder").

---

## When state is missing or stale

- **No `.sr-gtm/` directory:** First-run experience. The orchestrator should run the PMF diagnostic to populate `state.md` and `pmf-assessment.md`.
- **`state.md` exists but pillar files don't:** The founder has done the diagnostic but not yet built any pillars. Suggest the weakest pillar to start on.
- **`state.md` is older than 60 days:** Suggest re-running the system diagnostic. Levels can move (up or down) over time, and old advice may no longer fit.
- **Pillar files exist but were generated for a different level than the current one:** Flag this. Offer to re-draft at the current level.

---

## What this enables

- A founder can come back in three months and the plugin knows where they were
- Skills know each other's outputs without the founder having to copy-paste
- Market signal can flow back into the system, which is what the deck means by "the system gets sharper over the course of your go-to-market"
- The founder's data stays on their machine, in their format

This is the SR system, in files.
