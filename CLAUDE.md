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
│       ├── Connectivity.md
│       ├── Messaging.md
│       ├── missing.md
│       └── update-content.md
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

Use the `obsidian` command to interact with the vault while Obsidian is running.

`file=<name>` resolves by name (wikilink-style). `path=<path>` is exact.

```bash
# Read / write
obsidian read file=<name>
obsidian create path=<path> content=<text>
obsidian append file=<name> content=<text>
obsidian prepend file=<name> content=<text>
obsidian delete file=<name>
obsidian rename file=<name> name=<new-name>
obsidian move file=<name> to=<folder-path>

# Search
obsidian search query=<text>                         # files matching query
obsidian search:context query=<text>                 # matching lines with context
obsidian search query=<text> path=<folder>           # limit to folder

# Inspect vault
obsidian files
obsidian files folder=<path>
obsidian folders
obsidian tags counts
obsidian tasks todo
obsidian tasks verbose                               # grouped by file with line numbers
obsidian outline file=<name>                         # headings tree
obsidian links file=<name>                           # outgoing links
obsidian backlinks file=<name>                       # incoming links
obsidian orphans                                     # files with no incoming links
obsidian deadends                                    # files with no outgoing links
obsidian unresolved                                  # broken wikilinks in vault

# Frontmatter properties
obsidian property:set name=<key> value=<val> file=<name>
obsidian property:read name=<key> file=<name>
obsidian property:remove name=<key> file=<name>
obsidian properties file=<name>                      # list all properties on file

# Daily notes
obsidian daily:read
obsidian daily:append content=<text>
obsidian daily:prepend content=<text>
obsidian daily:path                                  # get path of today's note
```

---

## 5. Note Conventions

- Use wikilinks (`[[File Name]]`) for internal links.
- Every topic folder must have exactly one `Index.md` (capital I).
- **Wikilink path rules:** short names resolve to the file with that name in the **same directory**. Use `[[Index]]`, `[[Pods]]`, `[[Networking]]` for files in the same folder. For files in a different folder, use full path with display text: `[[AWS/Index|Index]]`, `[[Kubernetes/Index|Index]]`.
- Only split a topic into multiple files when the file gets too long or subtopics diverge enough to warrant separate navigation. Don't split preemptively.
- Add example commands alongside concept explanations.

---

## 6. Update Files — Pending Changes Workflow

Fixed filename: `update-content.md`. May exist in multiple dirs simultaneously, each with different content. Claude decides which note(s) in that dir the content belongs in — may split across multiple files.

**When user says "do your work" (or equivalent trigger):**
1. Run `obsidian files` to list vault files and find all `update-content.md` instances. Do NOT use `find` — RTK proxying can silently swallow results.
2. For each: read content, apply to appropriate note(s) in the same directory.
3. Empty `update-content.md` after applying using the `Write` tool with empty content. Do NOT use `obsidian create` — it appends ` 1` to the filename instead of overwriting.
4. Update `Index.md` if new notes were created.
5. Update `Home.md` file tree if new notes were created.

**Rules:**
- User writes; Claude routes and applies.

---

## 7. Missing Topics

Each folder may contain a `missing.md` that tracks topics not yet studied.

**Rules:**
- Don't add content to `missing.md` — the user fills it in when they study the topic.
- When a topic from `missing.md` gets its own note, remove it from `missing.md`.

---

## 8. Frontmatter Schema

All content notes and index files carry YAML frontmatter. Do NOT add frontmatter to `missing.md`, `update-content.md`, `CLAUDE.md`, or `Home.md`.

```yaml
---
domain: aws | kubernetes          # top-level subject area
track: cloud-practitioner | core  # certification / learning track
topic: compute | networking | ... # required on notes; omit on index files (type: index has no topic)
type: note | index
tags:
  - <domain>
  - <track>
  - <topic-specific tags>
---
```

**When adding a new note:** copy the frontmatter from a sibling note and update `topic` and `tags`. Tags should include the domain, track, and key concepts covered in the note.

---

## 9. Cross-Linking

When adding or editing notes, wire up wikilinks between related concepts across files. Do this proactively — don't wait to be asked.

**Rules:**
- Link to a section anchor when the target is a specific heading: `[[File#Section|display text]]`
- Link across topics when concepts are related (e.g. EKS → Kubernetes/Intro, Lambda → Compute#AWS Lambda)
- Link across domains (AWS ↔ Kubernetes) when one concept is the managed version of the other
- Never link to a heading that doesn't exist — verify anchor names match exactly (case-sensitive in Obsidian)
- Prefer inline links; keep display text natural
- Don't over-link: first meaningful mention in a section is enough, not every occurrence
