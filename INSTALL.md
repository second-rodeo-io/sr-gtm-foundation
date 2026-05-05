# Installing & Deploying SR GTM Foundation

This plugin works in **both Claude Cowork and Claude Code**. The plugin format is the same, but the install paths are completely different. This doc covers both, plus how to deploy changes if you're maintaining the plugin.

- **Cowork users** → install a `.plugin` file via drag-drop into chat. See [For end users — Cowork](#for-end-users--cowork).
- **Claude Code users** → install from a GitHub marketplace URL. See [For end users — Claude Code](#for-end-users--claude-code).
- **Maintainers** (Second Rodeo or anyone forking) → see [For maintainers](#for-maintainers) at the bottom.

---

## For end users — Cowork

Cowork is the desktop app for non-developer knowledge work. This plugin's primary audience.

### Quick install

1. Get the `.plugin` file. There are two ways:
   - **Direct download** — go to the [latest GitHub release](https://github.com/second-rodeo-io/sr-gtm-foundation/releases/latest) and download `sr-gtm-foundation.plugin`
   - **From a teammate** — anyone who has the `.plugin` file can send it to you via email, Slack, or a download link

2. **Drag-and-drop the `.plugin` file from Finder (or your Downloads folder) into Cowork chat.** Cowork shows a rich preview of the plugin contents. Click **Accept** to install all six skills in one step.

3. Kick things off by typing:

   > Run sr-gtm-system

   `sr-gtm-system` is the orchestrator — start here every time. It reads your state, figures out where you are in the PMF stack, and routes you to the right next skill. The other five skills are workers it dispatches to:

- `sr-gtm-system` — orchestrator (start here)
- `sr-pmf-diagnostic` — find your PMF level
- `sr-narrative-builder` — draft your narrative
- `sr-icp-builder` — define your ICP and personas
- `sr-distribution-aligner` — align distribution to your level
- `sr-system-diagnostic` — pressure-test your system

To kick things off, just type into Cowork chat:

> I want to use the SR GTM Foundation

Or be more specific:

> Run sr-gtm-system

The orchestrator will take it from there.

### What gets created on your machine

Once installed, the plugin will create a `.sr-gtm/` directory inside whatever folder you've connected to Cowork. That directory holds:

- `state.md` — the single-pane summary the orchestrator reads first
- `pmf-assessment.md` — your PMF diagnostic output
- `narrative.md` — your narrative draft (with a heavy "EDIT ME" header)
- `icp.md` — your ICP and personas
- `distribution.md` — your distribution plan
- `system-check.md` — latest pressure-test findings
- `signal-log.md` — market signal flowing back into the system

State stays on your machine. The plugin doesn't sync any of this anywhere. If you want it in Notion or a shared Drive, copy/paste — markdown travels.

### Updating in Cowork

Cowork plugins do not auto-update. To get a new version:

1. Download the new `.plugin` file from the [GitHub releases page](https://github.com/second-rodeo-io/sr-gtm-foundation/releases/latest)
2. Drag-drop it into Cowork chat — Cowork will detect the existing version and offer to update

Your `.sr-gtm/` state directory is untouched by updates (unless the release notes flag a state schema change — read the changelog for major version bumps).

### Uninstalling in Cowork

Open Cowork settings → Plugins → find `sr-gtm-foundation` → click Remove. Your `.sr-gtm/` state directory in your working folder is untouched. Delete it manually if you want it gone.

### Alternative: install individual skills

If you only want one or two skills (not the full system), you can install them individually as `.skill` files. The release page also includes per-skill `.skill` files alongside the bundled `.plugin`:

- `sr-gtm-system.skill`
- `sr-pmf-diagnostic.skill`
- `sr-narrative-builder.skill`
- `sr-icp-builder.skill`
- `sr-distribution-aligner.skill`
- `sr-system-diagnostic.skill`

Drag-drop each into Cowork chat for individual install. Note: the orchestrator (`sr-gtm-system`) only works as intended when the others are also installed, since it routes between them. Use this path only for testing or partial installs.

---

## For end users — Claude Code

Claude Code is the CLI / IDE tool for developers. The plugin works there too.

### Prerequisites

```bash
claude --version
```

Should return something. If not, install [Claude Code](https://claude.com/claude-code) first.

### Quick install

In any Claude Code session:

```text
/plugin marketplace add https://github.com/second-rodeo-io/sr-gtm-foundation
/plugin install sr-gtm-foundation@sr-gtm-foundation
```

The first command registers the marketplace. The second installs the plugin from it. The two `sr-gtm-foundation` references look redundant but they're not — first is the plugin name, second is the marketplace name. They happen to match.

### Updating in Claude Code

```text
/plugin marketplace update sr-gtm-foundation
/plugin install sr-gtm-foundation@sr-gtm-foundation
```

### Uninstalling in Claude Code

```text
/plugin uninstall sr-gtm-foundation
```

---

## Troubleshooting (both platforms)

**Skills aren't triggering on natural language.**
Try invoking the skill by exact name: "Run sr-gtm-system" or "Use the sr-pmf-diagnostic skill." If the skill is installed but the trigger phrases aren't matching, open an issue on the repo.

**State files in the wrong directory.**
The plugin reads/writes `.sr-gtm/` from whatever folder you've connected to Cowork (or whatever directory you launched Claude Code from). If you want to keep state per portfolio company, give each company its own folder.

**Plugin shows up but skills are missing.**
In Claude Code, run `/reload-plugins`. In Cowork, restart the session.

**"This plugin requires a newer version of Cowork/Claude Code."**
Update to the latest version of the host app.

---

## For maintainers

This section is for Second Rodeo (or anyone forking the repo) who needs to deploy changes, ship a new version, or add a new skill.

### Repo structure

```
sr-gtm-foundation/                ← repo root = plugin root
  .claude-plugin/
    plugin.json                   ← plugin manifest (name, version, description, license, keywords)
    marketplace.json              ← required for Claude Code distribution via /plugin marketplace add
  README.md                       ← public-facing intro
  LICENSE                         ← MIT
  INSTALL.md                      ← this file
  references/                     ← shared knowledge files used by multiple skills
    sr-pmf-levels.md
    sr-narrative-framework.md
    sr-three-together-matrix.md
    sr-tone-and-voice.md
    sr-state-file-schema.md
  skills/                         ← one folder per skill
    sr-gtm-system/SKILL.md
    sr-pmf-diagnostic/SKILL.md
    sr-narrative-builder/SKILL.md
    sr-icp-builder/SKILL.md
    sr-distribution-aligner/SKILL.md
    sr-system-diagnostic/SKILL.md
```

The repo IS the plugin (single plugin at repo root). If you later add more SR plugins to the same marketplace (e.g., `sr-fundraising-foundation`, `sr-hiring-foundation`), restructure to:

```
sr-marketplace/
  .claude-plugin/marketplace.json
  plugins/
    sr-gtm-foundation/
      .claude-plugin/plugin.json
      ...
    sr-fundraising-foundation/
      ...
```

### Local development workflow

#### Validate the plugin

Before anything else, verify the manifest is correct:

```bash
cd /path/to/sr-gtm-foundation
claude plugin validate .claude-plugin/plugin.json
```

Expected output: `✔ Validation passed`

#### Build the .plugin file for Cowork distribution

Cowork installs the full plugin from a single `.plugin` file (a zip of the plugin directory with the `.plugin` extension instead of `.zip`):

```bash
cd /path/to/sr-gtm-foundation
zip -r dist/sr-gtm-foundation.plugin . \
  -x "*.DS_Store" \
  -x "**/.DS_Store" \
  -x ".git/*" \
  -x "dist/*" \
  -x "node_modules/*"
```

To test locally in Cowork: drag `dist/sr-gtm-foundation.plugin` from Finder into a Cowork chat. The rich preview should show the plugin contents — click Accept.

#### (Optional) Build individual .skill files

If you also want to publish each skill as an individually installable `.skill` file (alongside the bundled `.plugin`), each skill folder needs its own copy of the references it uses:

```bash
#!/bin/bash
set -e
SRC=/path/to/sr-gtm-foundation
WORK=/tmp/cowork-skills
DIST=$SRC/dist
rm -rf "$WORK" && mkdir -p "$WORK" "$DIST"

for skill in sr-gtm-system sr-pmf-diagnostic sr-narrative-builder sr-icp-builder sr-distribution-aligner sr-system-diagnostic; do
  mkdir -p "$WORK/$skill/references"
  cp "$SRC/skills/$skill/SKILL.md" "$WORK/$skill/SKILL.md"
  cp "$SRC/references/"*.md "$WORK/$skill/references/"
  cd "$WORK/$skill" && zip -rq "$DIST/$skill.skill" . -x "*.DS_Store"
done

ls -la "$DIST"
```

This produces six `.skill` files in `dist/`. Attach all of them to GitHub releases alongside the `.plugin` so users have both install paths available.

#### Test the plugin in Claude Code

```bash
cd /path/to/sr-gtm-foundation
claude --plugin-dir .
```

This launches Claude Code with the plugin loaded from your working copy. Edits to SKILL.md files are picked up on the next session start (or via `/reload-plugins`).

Or install from a local path inside Claude Code:

```text
/plugin install /absolute/path/to/sr-gtm-foundation
```

#### What to test on every change

A real test pass walks a fake company through the full flow at multiple PMF levels (typically L1 and L4 — they have very different behavior). Things to verify:

- Orchestrator reads `.sr-gtm/state.md` correctly when it exists, creates it cleanly when it doesn't
- PMF diagnostic produces an assessment with evidence + what's missing to advance
- Narrative builder scales section count by level (L1 = sections 1-3, not 12)
- ICP builder labels evidence level explicitly (Hypothesis vs. Recognized vs. Design Partners)
- Distribution aligner refuses wrong-level work with a level-paired alternative (not just "no")
- System diagnostic identifies cross-pillar misalignment correctly
- All output files write to `.sr-gtm/` and update `state.md`

Read 2-3 skill outputs against the use/avoid word list in `references/sr-tone-and-voice.md`. If output uses "transform," "supercharge," "AI-powered," "holistic," "synergies," or any avoid-list words — that's a bug. Fix the SKILL.md prompts to push harder against generic SaaS language.

### Publishing to GitHub

#### First-time publish

```bash
cd /path/to/sr-gtm-foundation
git init
git add .
git commit -m "Initial release of SR GTM Foundation v0.1.0"
git branch -M main
git remote add origin git@github.com:second-rodeo-io/sr-gtm-foundation.git
git push -u origin main
```

The repo must be **public** for Claude Code marketplaces to install from it.

#### Publishing as a GitHub release

This is how Cowork users will get the plugin. Don't skip it.

1. Build the `.plugin` file (see the build section above). Optionally also build the six per-skill `.skill` files as a fallback for partial installs.

2. Tag the release:
   ```bash
   git tag v0.1.0
   git push --tags
   ```

3. On GitHub, go to **Releases → Draft a new release**:
   - Choose the tag you just pushed (e.g., `v0.1.0`)
   - Title: `v0.1.0 — Initial release` (or describe the change)
   - Description: changelog. What's new, what fixed, what's breaking.
   - **Attach `sr-gtm-foundation.plugin` as a release asset** — this is the primary file Cowork users download
   - **Optional:** also attach the six `.skill` files for users who only want individual skills
   - Click **Publish release**

The release page now serves as the canonical install point for both audiences:
- Cowork users: download `.plugin` from the release, drag-drop into Cowork chat
- Claude Code users: their `marketplace add` command pulls from the same repo

### Versioning

Use semantic versioning:

- **PATCH** (0.1.0 → 0.1.1) — typo fixes, voice tweaks, clarifications. No behavior changes.
- **MINOR** (0.1.0 → 0.2.0) — new skill added, new reference file, behavior improvements that aren't breaking.
- **MAJOR** (0.1.0 → 1.0.0) — breaking changes — state schema changes, skill renames, removed reference files.

When bumping a version:

1. Update `.claude-plugin/plugin.json` → `version`
2. Update `.claude-plugin/marketplace.json` → top-level `version` AND the `version` inside the plugin entry (two places, easy to forget the second)
3. Re-build the `.plugin` file
4. Tag in git: `git tag v0.X.Y && git push --tags`
5. Create the GitHub release with the new `.plugin` file attached
6. (Optional) Announce the update — there's no auto-update, so users only get the new version if they know to look

### Adding a new skill

1. Create `skills/your-new-skill/SKILL.md` — use existing skills as templates (frontmatter, tone, structure, related-skills footer)
2. Make the skill description rich with trigger phrases (lots of natural-language variants)
3. Update `README.md` skill table
4. Update `sr-gtm-system/SKILL.md` — orchestrator should know about the new skill and when to route to it
5. Update related skills' "Related skills" sections to cross-link
6. Bump version (MINOR)
7. Test locally
8. Re-build `.plugin` file, push to GitHub, create release

### Adding a new reference file

1. Place it in `references/`
2. Skills that use it should reference it explicitly in their "Reference files used by this skill" section
3. The reference itself should be in SR voice — read `sr-tone-and-voice.md` first
4. Bump version (MINOR if any skill behavior changes; PATCH if purely additive context)

### Changing state schema

State schema changes (modifying `.sr-gtm/state.md` format or any pillar file format) are **breaking**. Bump MAJOR.

When you do this:

1. Document the migration path in `references/sr-state-file-schema.md`
2. Add migration notes to the GitHub release
3. Consider writing a one-time migration skill (e.g., `sr-migrate-state-v1-to-v2`) that reads old format and rewrites in new format

### Things to NOT do

- **Don't add MCP servers without thinking carefully.** This plugin's value is that it's file-only, no external dependencies. Adding an MCP server (Notion, Slack, etc.) makes sense for an SR-internal version but limits the open-source release. If you do add MCP integrations, use the `~~category` placeholder pattern documented in Cowork's plugin spec, and add a `CONNECTORS.md` file explaining which categories the user needs to connect.
- **Don't bake in SR-specific identifiers, URLs, or accounts.** The plugin should work for any founder.
- **Don't soften the refusals to please users.** The plugin's distinctive value is its willingness to tell founders things they don't want to hear. PRs that soften this lose the SR voice.
- **Don't add new pillars.** Narrative / ICP / Distribution is from the deck and intentional. New pillars belong in separate plugins in the same marketplace, not bolted onto this one.

### Shipping a new release: full checklist

- [ ] Local testing on at least 2 PMF levels (L1 and L4 give very different behavior)
- [ ] Tone check: read 2-3 skill outputs against the use/avoid word list
- [ ] `claude plugin validate .claude-plugin/plugin.json` passes
- [ ] Update `version` in `plugin.json`
- [ ] Update `version` in `marketplace.json` (top-level AND inside plugin entry)
- [ ] Update `README.md` if user-facing changes
- [ ] Update `INSTALL.md` if install commands changed
- [ ] Build the `.plugin` file (and optionally the six `.skill` files)
- [ ] `git commit -m "v0.X.Y"`, `git tag v0.X.Y`, `git push --tags`
- [ ] Create GitHub release, attach `.plugin` (and optionally `.skill` files)
- [ ] Test install from GitHub release on a clean machine (both Cowork — drag `.plugin` into chat — and Claude Code — `/plugin marketplace add`)
- [ ] Announce the update (Twitter/LinkedIn/SR newsletter)

---

## Questions or issues

Open an issue on the repo: [github.com/second-rodeo-io/sr-gtm-foundation/issues](https://github.com/second-rodeo-io/sr-gtm-foundation/issues)

Or reach out to Second Rodeo directly: [secondrodeo.io](https://secondrodeo.io)
