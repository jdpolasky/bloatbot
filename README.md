# BloatBot

An auditor for your AI workspace. Finds the bloat, proposes the cuts, touches nothing.

BloatBot walks your folder tree and reports what is redundant, dead, or wasting tokens: instruction files that load every session, duplicate notes, dead links, orphan files, overlapping skills, abandoned config. Findings are ranked by payoff. It never edits, deletes, or renames anything without your explicit go, file by file.

Works on Obsidian vaults, Logseq graphs, plain markdown folders, agent workspaces (Claude Code, Codex, etc.), and RAG source folders.

Sibling to [BuildBot](https://github.com/jdpolasky/buildbot). BloatBot prunes the system, BuildBot grows it, and neither grades its own work. Both came out of [chief-of-staff-2](https://github.com/jdpolasky/chief-of-staff-2), an AI Chief of Staff that lives in an Obsidian vault as plain markdown; they are the tools it is kept lean with.

## Install

**Claude Code:**

```bash
# all projects
cp -r bloatbot ~/.claude/skills/bloatbot

# or one project
cp -r bloatbot your-project/.claude/skills/bloatbot
```

New session, then `/bloatbot`.

**Everything else:** `bloatbot/SKILL.md` is plain instructions. Paste it into any model that can read your files and tell it where to look.

## Usage

```
/bloatbot
> Where should I check, and what platform is this?
"My Obsidian vault at ~/vault. Everything. Nothing off-limits."
```

It walks the structure first, then hunts in order of payoff, then reports. Condensed from a real audit of a test vault:

```
1. Dead plugin folder
   .obsidian/plugins/dead-plugin-2023/ is abandoned but still in the
   load cycle. Proposed: delete the folder.

2. Dead link in Projects/Big Project.md
   [[Old Archive Note]] doesn't exist anywhere in the vault.
   Proposed: fix or remove the reference.

3. Missing frontmatter on Daily/2026-07-30.md
   Every other note follows title/created/type. This one has none.
   Proposed: add matching frontmatter.

Top three cuts: delete the dead plugin, fix the archive link,
patch the daily note.
```

Then it stops. Nothing happens to any file unless you name the file and the action.

## What it hunts

- Always-loaded files (CLAUDE.md, AGENTS.md, rules, memory) heavier than their job requires
- Duplicate or overlapping notes and instructions
- Dead files and dead links
- Orphan notes with no metadata and no links
- Skills and commands that overlap or that nothing invokes
- Empty folders, stray files, structure drift
- Slogans and cryptic one-liners sitting where instructions should be

## Rulings

Optionally (with your permission) it keeps one file, `bloatbot-rulings.md`: findings you've ruled "keep" are never re-reported, and a baseline table tracks the size of your always-loaded files across audits so drift is visible.

## Why

Agent setups rot in predictable ways. Rules accumulate and nothing gets deleted. Parts bolted on at different times stop fitting. The same fact ends up in four places. The always-loaded files grow until they crowd out the actual work. BloatBot's job is to find all of it before your system becomes its own project.
