# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is an Obsidian markdown notes vault synced via Git. The vault is named **Notes** and lives at `$HOME/Documents/Notes`. The only community plugin installed is **obsidian-git**.

## Obsidian CLI

Use the `obsidian` command to interact with the vault while Obsidian is running:

```bash
# Read/write notes
obsidian read file=<name>
obsidian create path=<path> content=<text>
obsidian append file=<name> content=<text>

# Search
obsidian search query=<text>
obsidian search:context query=<text>

# Inspect vault
obsidian files
obsidian folders
obsidian tags counts
obsidian tasks todo

# Properties (frontmatter)
obsidian property:set name=<key> value=<val> file=<name>
obsidian property:read name=<key> file=<name>

# Daily notes
obsidian daily:read
obsidian daily:append content=<text>
```

File lookup: `file=<name>` resolves by name (like wikilinks); `path=<path>` is exact.

## Git Sync

The obsidian-git plugin is configured to:
- Pull from remote on Obsidian startup (`autoPullOnBoot`)
- Commit and push to remote on Obsidian close (`commitOnClose`, `pushOnClose`)

Manual git operations work normally via the CLI.

## Vault Structure

- Notes live under `AWS/` (currently the only top-level folder)
- Core plugins enabled: file-explorer, search, graph, backlink, canvas, tag-pane, properties, daily-notes, templates, note-composer, bookmarks, outline, bases
- No CSS snippets or custom themes
