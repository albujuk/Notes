# CLAUDE.md

Claude Code guidance for this repository.

---

## 1. Vault Overview

Obsidian markdown notes vault synced via Git.
- **Name:** Notes
- **Path:** `$HOME/Documents/Notes`
- **Community plugins:** obsidian-git only
- **Core plugins:** file-explorer, search, graph, backlink, canvas, tag-pane, properties, daily-notes, templates, note-composer, bookmarks, outline, bases
- No CSS snippets or custom themes

---

## 2. Navigation — Always Check Indexes First

Every folder has an `Index.md`. Start there before reading or editing notes.

| Scope | File |
|-------|------|
| Whole vault | `Home.md` |
| AWS | `AWS/Index.md` |
| AWS Cloud Practitioner | `AWS/Cloud Practitioner/Index.md` |
| Kubernetes | `Kubernetes/Index.md` |

**Rules:**
- Before adding a note: check the folder's `Index.md` to understand existing structure and order.
- After adding a note: update the folder's `Index.md` (and parent indexes if it's a new folder).
- After adding a folder: add it to `Home.md` and the parent folder's `Index.md`.
- Keep indexes in learning/logical order, not alphabetical.

---

## 3. Vault Structure

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

# Daily notes
obsidian daily:read
obsidian daily:append content=<text>
```

`file=<name>` resolves by name (wikilink-style). `path=<path>` is exact.

---

## 5. Git Sync

obsidian-git plugin behavior:
- **On Obsidian open:** pull from remote (`autoPullOnBoot`)
- **On Obsidian close:** commit + push (`commitOnClose`, `pushOnClose`)

Manual git ops via CLI work normally.

---

## 6. Note Conventions

- Use wikilinks (`[[File Name]]`) for internal links.
- Index nav footers use: `← [[Parent/Index]] · [[Home]]`
- Only split a topic into multiple files when the file gets too long or subtopics diverge enough to warrant separate navigation. Don't split preemptively.
- Add example commands alongside concept explanations.

---

## 7. Update Files — Pending Changes Workflow

Fixed filename: `update-content.md`. May exist in multiple dirs simultaneously, each with different content. Claude decides which note(s) in that dir the content belongs in — may split across multiple files.

**When user says "do your work" (or equivalent trigger):**
1. Glob `**/update-content.md` to find all instances.
2. For each: read content, apply to appropriate note(s) in the same directory.
3. Delete the `update-content.md` after applying.
4. Update `Index.md` if new notes were created.

**Rules:**
- User writes; Claude routes and applies.
- Always empty `update-content.md` after applying (don't delete).

---

## 8. Missing Topics

Each folder may contain a `missing.md` that tracks topics not yet studied.

**Rules:**
- Don't add content to `missing.md` — the user fills it in when they study the topic.
- When a topic from `missing.md` gets its own note, remove it from `missing.md`.
- Current missing-topic files: `AWS/Cloud Practitioner/missing.md`, `Kubernetes/missing.md`.