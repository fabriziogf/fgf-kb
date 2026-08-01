# fgf-knowledge-base

A version-controlled markdown knowledge base, built from handwritten notes by
[NoteKB](https://github.com/fabriziogf/noteKB).

## Layout

| Path | Purpose |
|---|---|
| `index.md` | Entry point; links every topic file |
| `topics/` | Topic files — the knowledge itself |
| `inbox/` | Drop note images here to be incorporated |
| `archive/` | Processed source images |
| `archive/manifest.jsonl` | Append-only record: one line per processed image |

## Provenance

Every topic file carries its sources in frontmatter, so any fact traces back to
the image and date it came from:

```yaml
---
title: Retrieval evaluation
updated: 2026-07-31
sources:
  - image: archive/2026-07-31-note-0012.jpg
    added: 2026-07-31
    summary: one-line description of what this note contributed
---
```

## Review gate

Nothing auto-merges. NoteKB opens a pull request; a human approves it.
