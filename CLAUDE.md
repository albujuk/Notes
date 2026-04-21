# CLAUDE.md

Claude Code guidance for this repository.

---

## 1. Vault Overview

Obsidian markdown notes vault synced via Git.
- **Path:** `$HOME/Documents/Notes`
- **Community plugins:** obsidian-git only

---

## 2. Vault Structure

```
Notes/
├── Home.md                          ← root index
├── CLAUDE.md
├── AWS/
│   ├── Index.md
│   └── Cloud Practitioner/
│       ├── Index.md
│       ├── Compute.md
│       ├── Containers.md
│       ├── Global Infrastructure.md
│       ├── CloudFormation.md
│       ├── Networking.md
│       └── missing.md
└── Kubernetes/
    ├── Index.md
    ├── Intro.md
    ├── Pods.md
    ├── Workloads.md
    ├── kubectl.md
    └── missing.md
```

---

## 3. Indexes and Reference Docs

Every folder has an `Index.md`. `Home.md` is the root index.

**Always:**
- Check the folder's `Index.md` before adding or editing notes
- Keep index rows in learning/logical order, not alphabetical
- Index row format: `| # | Topic | [[File]] | one-line summary of contents |`

**When content changes:**
- New note → add row to folder's `Index.md`
- New folder → add to parent `Index.md` + `Home.md` Topics table + `Home.md` file tree + Section 2 above
- Note deleted/renamed → update every index that references it
- Do index updates in the same edit pass as the content change — never leave them out of sync

**Files to keep in sync:**

| File | What it tracks |
|------|---------------|
| `Home.md` | All top-level topic folders + file tree |
| `AWS/Index.md` | All certification/track sub-folders under AWS |
| `AWS/Cloud Practitioner/Index.md` | All notes in Cloud Practitioner, in learning order |
| `Kubernetes/Index.md` | All notes in Kubernetes, in learning order |

---

## 4. Obsidian CLI

Use the `obsidian` command to interact with the vault while Obsidian is running:

```bash
# Read / write
obsidian read file=<name>
obsidian create path=<path> content=<text>
obsidian append file=<name> content=<text>

# Search
obsidian search query=<text>
obsidian search:context query=<text>

# Inspect
obsidian files
obsidian folders
obsidian tags counts
obsidian tasks todo

# Frontmatter properties
obsidian property:set name=<key> value=<val> file=<name>
obsidian property:read name=<key> file=<name>
```

`file=<name>` resolves by name (wikilink-style). `path=<path>` is exact.

---

## 5. Note Conventions

- Use wikilinks (`[[File Name]]`) for internal links.
- Index nav footers use: `← [[Parent/Index]] · [[Home]]`
- Only split a topic into multiple files when the file gets too long or subtopics diverge enough to warrant separate navigation. Don't split preemptively.
- Add example commands alongside concept explanations.

---

## 6. Update Files — Pending Changes Workflow

Fixed filename: `update-content.md`. May exist in multiple dirs simultaneously, each with different content. Claude decides which note(s) in that dir the content belongs in — may split across multiple files.

**When user says "do your work" (or equivalent trigger):**
1. Glob `**/update-content.md` to find all instances.
2. For each: read content, apply to appropriate note(s) in the same directory.
3. Empty `update-content.md` after applying (don't delete).
4. Update `Index.md` if new notes were created.

**Rules:**
- User writes; Claude routes and applies.

---

## 7. Missing Topics

Each folder may contain a `missing.md` that tracks topics not yet studied.

**Rules:**
- Don't add content to `missing.md` — the user fills it in when they study the topic.
- When a topic from `missing.md` gets its own note, remove it from `missing.md`.

---

## 8. Cross-Linking

When adding or editing notes, wire up wikilinks between related concepts across files. Do this proactively — don't wait to be asked.

**Rules:**
- Link to a section anchor when the target is a specific heading: `[[File#Section|display text]]`
- Link across topics when concepts are related (e.g. EKS → Kubernetes/Intro, Lambda → Compute#AWS Lambda)
- Link across domains (AWS ↔ Kubernetes) when one concept is the managed version of the other
- Never link to a heading that doesn't exist — verify anchor names match exactly (case-sensitive in Obsidian)
- Prefer inline links; keep display text natural
- Don't over-link: first meaningful mention in a section is enough, not every occurrence
