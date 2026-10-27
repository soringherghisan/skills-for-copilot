# skills-for-copilot

Personal agent skills, packaged for GitHub Copilot and any other agent that
follows the [agentskills.io](https://agentskills.io/specification) open
standard (OpenCode, Claude Code, Codex, Cursor).

## The skills

| Skill | Invoke | What it does |
| --- | --- | --- |
| `show-me` | `/show-me` | Answers with concise visuals — component trees, call stacks, diffs, Mermaid, focused HTML — instead of walls of prose. |
| `learning-opportunities` | `/learning-opportunities` | Offers 10–15 minute evidence-based learning exercises (prediction, teach-it-back, retrieval check-ins) after architectural work. |
| `orient` | `/orient` | Explores an unfamiliar repo strategically and generates an `orientation.md` lesson file for `learning-opportunities orient`. |
| `caveman` | `/caveman` | Terse, low-fluff response voice. Technical payload stays verbatim. |
| `ultracave` | `/ultracave` | Caveman with the grammar stripped: fragments, maximum compression. |
| `megacave` | `/megacave` | Caveman in Classical Chinese (文言文). |
| `writing-for-agents` | `/writing-for-agents` | Reference for writing documents agents consume: skills, AGENTS.md, context pointers. |
| `teach` | `/teach` | Turns a directory into a stateful teaching workspace: mission, vetted resources, HTML lessons, learning records across sessions. |
| `grilling` | `/grilling` | Relentless interview about a plan or design, worked as a decision tree in rounds. |
| `grill-me` | `/grill-me` | Alias that invokes the `grilling` skill. |
| `wait-what` | `/wait-what` | Re-pitches a message that didn't land, in plain English, using the workspace glossary. |

All eleven are **user-invoked**: they appear in the `/` menu but the agent never
loads them on its own (`disable-model-invocation: true`).

---

## Agent install procedure

You are an AI coding agent (Copilot or similar), reading this repository with
the task of installing its skills globally for your user. Follow the steps in
order. Each step ends on a completion criterion; do not move on until it holds.

### Step 1 — Pick the install target

Check these directories, in order, and take the first match:

1. `~/.copilot/skills` — the GitHub Copilot personal skills directory
2. `~/.agents/skills` — the cross-agent shared skills directory
3. Neither exists → create `~/.copilot/skills`

**Criterion:** the install target is a single absolute directory path that
exists on disk.

### Step 2 — Copy the skills

Copy each directory under `skills/` in this repository, whole, into the
install target:

```
cp -r skills/<skill-id> <install-target>/<skill-id>
```

**Criterion:** every skill directory under `skills/` exists at
`<install-target>/<skill-id>/SKILL.md`, including skills with supporting
files (`writing-for-agents/SKILL-MECHANICS.md`, `teach/*-FORMAT.md`,
`learning-opportunities/resources/`, `orient/resources/`).

### Step 3 — Verify

For each installed skill, read the frontmatter of
`<install-target>/<skill-id>/SKILL.md` and confirm the `name:` value matches
the directory name.

**Criterion:** all skills verified, zero mismatches.

### Step 4 — Report

Tell the user:

- The install target used, and why (which of the three rules matched)
- The installed skill IDs (one per skill directory copied)
- That each is invoked with `/skill-id` (for example `/show-me`) and will not
  trigger automatically
- That skills take effect in a new session

**Update path:** if the install target already contains a directory with the
same name as a skill, compare the existing `SKILL.md` with the repository's
version. If they differ, ask the user before overwriting — do not replace
files without confirmation.

### Step 5 — Smoke test

Tell the user to open a new session and check the `/` menu: every installed
skill must appear there, invocable by its ID.

**Criterion:** the user confirms each installed skill shows up in the `/`
menu. Report any skill that does not — that is a surface-specific bug with
the `disable-model-invocation` flag (known in GitHub Copilot CLI and the VS
Code Agents window), not a failed install.

---

## Manual install (for humans)

```sh
git clone https://github.com/<owner>/skills-for-copilot.git
mkdir -p ~/.copilot/skills
cp -r skills-for-copilot/skills/* ~/.copilot/skills/
```

On a machine where `~/.agents/skills` is already the shared skills directory,
copy into that instead — both Copilot and OpenCode read it.

Alternative, if Node is available: `npx skills add <owner>/skills-for-copilot`.

## Notes

- `learning-opportunities` and `orient` work as a pair: run `/orient` in an
  unfamiliar repo, then `/learning-opportunities orient`.
- On OpenCode, the ultracave voice can be made the session default via the
  `instructions` config field; on Copilot, the equivalent is
  `~/.github/copilot-instructions.md`. See ATTRIBUTION.md for sources and
  licenses.
