# CLAUDE.md

Claude Code guidance for this repository.

---

## 1. Vault Overview

Obsidian markdown notes vault synced via Git.
- **Path:** `$HOME/Documents/Notes`
- **Community plugins:** obsidian-git only

---

## 2. Vault Structure

Numbered top-level buckets (PARA-ish). Buckets are pre-reserved but only created when their first note lands. **Do not pre-create empty buckets.**

```
Notes/
├── README.md                        ← root index
├── CLAUDE.md
├── 000 - Inbox/                     ← quick captures, unsorted
│   └── README.md
├── 100 - Cloud/
│   └── AWS/
│       ├── README.md
│       └── Cloud Practitioner/
│           ├── README.md
│           ├── (notes)
│           └── missing.md
├── 200 - DevOps/                    ← reserved (CI-CD, Linux, Monitoring)
├── 300 - Containers/
│   └── Kubernetes/
│       ├── README.md
│       ├── (notes)
│       ├── kubectl.md
│       └── missing.md
├── 400 - IaC/                       ← reserved (Terraform, Ansible)
├── 500 - Networking/                ← reserved
├── 600 - Security/                  ← reserved
├── 700 - Projects/                  ← reserved
├── 800 - References/                ← reserved (Cheatsheets, Bookmarks, Glossary)
└── 900 - Meta/
    ├── README.md
    └── MOCs/
        ├── AWS MOC.md
        └── Kubernetes MOC.md
```

**Subfolder rule of thumb:** only create a subfolder when you already have **10+ notes** on that topic and they feel cluttered. Don't pre-create empty subfolders for what might exist someday. Folders = coarse buckets. Tags + links = fine-grained organization.

---

## 3. Indexes vs MOCs

Two distinct artifacts:

- **`README.md`: folder index. Mechanical.** Lists files in that folder. Answers "what's in this folder?". Plain link list, no commentary needed.
- **`<Topic> MOC.md`: concept map. Intellectual.** Groups notes by relationship, not file location. Can link across folders, add context, show learning order, cluster ideas thematically. Answers "how do these ideas connect?". Lives in `900 - Meta/MOCs/`.

`README.md` is the root index. It links to top-level READMEs and to MOCs.

**Always:**
- Check the folder's `README.md` before adding or editing notes.
- Keep `README.md` rows in learning/logical order, not alphabetical.
- Index row format (when richer than a bare list): `| # | [[File]] | one-line summary |`

**When content changes:**
- New note → add row to folder's `README.md`. Update relevant MOC if it changes the concept map.
- New folder → add to parent `README.md` + root `README.md` (Topics table + file tree) + Section 2 above.
- Note deleted/renamed → update every index/MOC that references it.
- Do index/MOC updates in the same edit pass as the content change. Never out of sync.

**Files to keep in sync:**

| File | Tracks |
|------|--------|
| `README.md` | Top-level buckets, MOCs, file tree |
| `100 - Cloud/AWS/README.md` | Cert/track sub-folders under AWS |
| `100 - Cloud/AWS/Cloud Practitioner/README.md` | Notes in Cloud Practitioner, learning order |
| `300 - Containers/Kubernetes/README.md` | Notes in Kubernetes, learning order |
| `900 - Meta/MOCs/<Topic> MOC.md` | Concept map for that topic |

---

## 4. Obsidian CLI

**Default tool for vault operations.** Prefer `obsidian` over filesystem tools (`Read`, `Edit`, `Write`, `find`, `grep`, `sed`, `ls`). Fall back to filesystem tools only when the CLI has no equivalent (e.g. surgical mid-file edits, bulk regex replace, git operations).

Acceptable fallbacks:
- Mid-file surgical edits → `Edit` (CLI has no equivalent, only `read`/`append`/`prepend`/`create`)
- Bulk multi-file regex → `sed` via `Bash`
- Clearing inbox or update files → `Write` with empty content (see Section 6)
- Renames/moves → prefer `obsidian rename` / `obsidian move` over `mv`
- Git → always `Bash`

Obsidian must be running for the CLI to work. If a CLI call fails with a connection error, surface it before falling back.

`file=<name>` resolves by name (wikilink-style). `path=<path>` is exact. Most commands default to the active file when `file`/`path` omitted. Quote values with spaces. Use `\n` for newline in `content`.

**Discoverability (run before guessing):**

```bash
obsidian help                       # full command list
obsidian help <command>             # detailed flags for one command
obsidian commands                   # Obsidian app command IDs (for `obsidian command id=...`)
```

**Commonly used:**

```bash
# Read / write
obsidian read file=<name>
obsidian create path=<path> content=<text>           # add `overwrite` flag to replace existing
obsidian append file=<name> content=<text>           # `inline` to skip newline
obsidian prepend file=<name> content=<text>
obsidian delete file=<name>                          # `permanent` to skip trash
obsidian rename file=<name> name=<new-name>
obsidian move file=<name> to=<folder-path>
obsidian open file=<name>                            # open in Obsidian UI

# Search
obsidian search query=<text>                         # files matching query
obsidian search:context query=<text>                 # matching lines with context
obsidian search query=<text> path=<folder>           # limit to folder
obsidian search query=<text> case                    # case-sensitive

# Inspect vault
obsidian vault                                       # vault info
obsidian files                                       # `folder=<path>`, `ext=<ext>` to filter
obsidian folders
obsidian file file=<name>                            # single file info
obsidian folder path=<path>                          # single folder info
obsidian wordcount file=<name>
obsidian recents

# Links & navigation
obsidian links file=<name>                           # outgoing links
obsidian backlinks file=<name>                       # incoming links
obsidian orphans                                     # files with no incoming links
obsidian deadends                                    # files with no outgoing links
obsidian unresolved                                  # broken wikilinks in vault
obsidian outline file=<name>                         # headings tree

# Tags & aliases
obsidian tags counts
obsidian tag name=<tag> verbose                      # files using a tag
obsidian aliases verbose

# Frontmatter properties
obsidian property:set name=<key> value=<val> file=<name>
obsidian property:read name=<key> file=<name>
obsidian property:remove name=<key> file=<name>
obsidian properties file=<name>                      # list all properties on file

# Tasks
obsidian tasks todo                                  # incomplete tasks vault-wide
obsidian tasks verbose                               # grouped by file with line numbers
obsidian task ref=<path:line> done                   # mark task done

# Daily notes
obsidian daily:read
obsidian daily:append content=<text>
obsidian daily:prepend content=<text>
obsidian daily:path                                  # get path of today's note

# History (file versions)
obsidian history file=<name>                         # list versions
obsidian history:read file=<name> version=<n>
obsidian diff file=<name> from=<n> to=<n>
```

Other namespaces: `bookmark*`, `template*`, `plugin*`, `theme*`, `snippet*`, `tab*`, `workspace`, `random*`, `base*`, `dev:*`. See `obsidian help`.

---

## 5. Note Conventions

- Use wikilinks (`[[File Name]]`) for internal links.
- Every topic folder must have exactly one `README.md` (uppercase, GitHub-style; renders by default in folder views).
- **Wikilink path rules:** bare `[[Name]]` is fine when the target filename is unique vault-wide. When duplicates exist (multiple `README.md`, multiple `missing.md`), use full path with display text: `[[100 - Cloud/AWS/README|AWS]]`, `[[300 - Containers/Kubernetes/README|Kubernetes]]`.
- Only split a topic into multiple files when the file gets too long or subtopics diverge enough. Don't split preemptively.
- Add example commands alongside concept explanations.
- Do NOT use em dashes (—). Use commas, semicolons, colons, or separate sentences instead.

---

## 6. Inbox: Pending Changes Workflow

Quick captures land in `000 - Inbox/`. Files there are unsorted; Claude routes them into the right topic folder(s).

**When user says "do your work" (or equivalent trigger):**
1. Run `obsidian files folder="000 - Inbox"` to list inbox files. Do NOT use `find`; RTK proxying can silently swallow results.
2. For each file (skip `README.md`): read content, decide which existing note(s) it belongs in, apply.
3. After applying, delete the inbox file with `obsidian delete file=<name>`. (May go to trash; use `permanent` flag to skip.)
4. If the content is a brand new topic, create a new note in the correct bucket folder, add a row to that folder's `README.md`, and update the relevant MOC.
5. Update `README.md` file tree if a new top-level bucket or folder was created.

**Rules:**
- User writes; Claude routes and applies.
- Inbox should be empty (except `README.md`) after a "do your work" pass.

---

## 7. Missing Topics

Each topic folder may contain a `missing.md` tracking topics not yet studied.

**Rules:**
- Don't add content to `missing.md`; the user fills it in when they study the topic.
- **Checkbox semantics:** `[x]` means the topic is covered at an intro/awareness level (e.g. a foundational cert or track) but not yet at the current track's depth. Unchecked `[ ]` means not covered at any level.
- When a topic is fully covered at the current track's depth (has its own dedicated note at the right level), remove it from `missing.md` entirely.

---

## 8. Frontmatter Schema

All content notes and index files carry YAML frontmatter. Do NOT add frontmatter to `missing.md`, inbox files, `CLAUDE.md`, or `README.md`.

```yaml
---
domain: aws | kubernetes | ...    # top-level subject area; expand as new buckets fill
track: cloud-practitioner | core  # certification / learning track (omit if N/A)
topic: compute | networking | ... # required on notes; omit on index files
type: note | index | moc
tags:
  - <domain>
  - <track>
  - <topic-specific tags>
---
```

**When adding a new note:** copy the frontmatter from a sibling note and update `topic` and `tags`. Tags should include the domain, track, and key concepts covered.

**MOC files** (`900 - Meta/MOCs/<Topic> MOC.md`) use `type: moc` with a `moc` tag plus the topic tag.

---

## 9. Cross-Linking

When adding or editing notes, wire up wikilinks between related concepts across files. Do this proactively; don't wait to be asked.

**Link hierarchy:**
- Root `README.md` → folder READMEs only (not individual notes)
- Folder READMEs → individual notes + link up to root `README.md`
- Individual notes → sibling notes + their folder README (NOT root `README.md`)
- Use display text on full-path links: `[[100 - Cloud/AWS/Cloud Practitioner/README|Cloud Practitioner]]`

**Rules:**
- Link to a section anchor when the target is a specific heading: `[[File#Section|display text]]`
- Link across topics when concepts are related (e.g. EKS → [[Cluster]], Lambda → [[Compute#AWS Lambda]])
- Link across domains (AWS ↔ Kubernetes) when one concept is the managed version of the other, and surface the connection in the relevant MOC.
- Never link to a heading that doesn't exist; verify anchor names match exactly (case-sensitive in Obsidian)
- Prefer inline links; keep display text natural
- Don't over-link: first meaningful mention in a section is enough, not every occurrence
