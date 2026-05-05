# SR GTM Foundation

A Claude plugin that helps founders build the three things that compound in early-stage GTM — narrative, ICP, and distribution — by first locating where they actually are in the PMF stack.

Built by [Second Rodeo](https://secondrodeo.io). Open source under MIT.

## The thesis

Founders don't have a GTM problem. They have a sequence problem.

Most early-stage GTM advice is generic — "build outbound," "do content," "post on LinkedIn." It's the right work at the wrong time, then founders blame the work. The fix isn't more tactics. It's a sequence.

This plugin encodes Second Rodeo's framework: a five-level PMF stack, the three foundational pillars (narrative, ICP, distribution), and the matrix of what each pillar should look like at each level. The same question — "how should I do distribution?" — gets a different answer at L1 (use your founder network) than at L4 (build repeatable channels). The plugin routes accordingly.

## What's in the box

Six skills that work together as one system:

| Skill | What it does |
|---|---|
| **`sr-gtm-system`** ← **start here** | Orchestrator. Front door for the whole flow — reads your state, tells you where to focus. Always invoke this first. |
| `sr-pmf-diagnostic` | Finds your PMF level (1-5) through guided conversation, with evidence and what's missing to advance. |
| `sr-narrative-builder` | Drafts your narrative using the 12-section Andy Raskin / April Dunford / manifesto framework, scaled to your level. |
| `sr-icp-builder` | Builds your company ICP and personas-at-the-company, anchored to the evidence you actually have. |
| `sr-distribution-aligner` | Aligns distribution work to your PMF stage. Will refuse to build an outbound playbook for an L1 founder. |
| `sr-system-diagnostic` | Capstone. Pressure-tests whether you have a system or a lottery ticket. |

Once installed, just type into chat:

> Run sr-gtm-system

The orchestrator takes it from there — it figures out where you are and routes to the right next skill.

All output is markdown. Edit it, push it to Notion, paste it in Google Docs — it's yours.

## How it works

When you invoke any skill, the plugin reads from a shared state directory (`.sr-gtm/`) in your working folder. That directory holds your current PMF assessment, narrative draft, ICP, distribution plan, and a single-pane summary of where everything stands. Skills read it, update it, and feed each other.

This means:
- You can come back in a month and the plugin remembers where you were
- Each skill knows your PMF level and gives advice appropriate to it
- Market signal flows back into narrative — the loop the deck calls "the system gets sharper over the course of your go to market"

## Important: this produces drafts

Every output from this plugin is a draft. The narrative, the ICP, the distribution plan — none of it is done when Claude hands it to you. The work that makes it real is the editing, the customer conversations, the saying it out loud and noticing what doesn't sound like you. The plugin will tell you this in red text every time.

A founder who treats Claude's output as final will end up with a competent-looking version of someone else's company. The point of the plugin is to give you the structure and the questions, not to replace the work.

## Installation

Works in both **Claude Cowork** and **Claude Code**.

**In Cowork:** download `sr-gtm-foundation.plugin` from the [latest release](https://github.com/second-rodeo-io/sr-gtm-foundation/releases/latest) and drag-drop it into Cowork chat. Click Accept on the rich preview. All six skills install in one step.

**In Claude Code:**
```text
/plugin marketplace add https://github.com/second-rodeo-io/sr-gtm-foundation
/plugin install sr-gtm-foundation@sr-gtm-foundation
```

After installing, kick things off with:

> Run sr-gtm-system

For full instructions (both environments), troubleshooting, deployment, and version management, see [INSTALL.md](./INSTALL.md).

## Usage

Start with the orchestrator:

```
/sr-gtm-system
```

Or jump directly into any skill:

```
/sr-pmf-diagnostic           # Where am I in the stack?
/sr-narrative-builder        # Draft my story
/sr-icp-builder              # Define my customer
/sr-distribution-aligner     # How should I be reaching them?
/sr-system-diagnostic        # Pressure-test the whole thing
```

## The framework

### The five PMF levels

| Level | Name | Test |
|---|---|---|
| L1 | Problem Unclear | A specific group names the same problem unprompted |
| L2 | Solution Unclear | Defensible POV on the type of solution this problem calls for |
| L3 | Not Yet Inevitable | 40% would be genuinely disappointed if it disappeared |
| L4 | PMF Emerging | A repeatable way to reach and serve the right users |
| L5 | Fit Is Durable | Grow while keeping coherence and trust |

### The three pillars across levels

|  | L1 | L2 | L3 | L4 | L5 |
|---|---|---|---|---|---|
| **Narrative** | Hypothesis | Category bet | Customer language | Internal alignment | Owned |
| **ICP** | Guess | Recognized | Design Partners | Primary ICP | Expandable |
| **Distribution** | Network | Experiments | Concentrated | Repeatable | Compounding |

The plugin uses this matrix to keep advice level-appropriate.

## Credits

Framework by Second Rodeo. The narrative scaffolding draws on Andy Raskin's strategic narrative work, April Dunford's positioning framework (*Obviously Awesome*), and the manifesto tradition used by Linear, Apple, and Patagonia.

## License

MIT — use it, fork it, learn from it. If you build on it, we'd love to know what you make.
