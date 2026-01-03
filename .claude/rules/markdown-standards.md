---
paths: "**/*.md"
---

# Markdown Standards for Vault

These conventions apply to all markdown files in the vault.

## File Naming

- **Daily notes:** `YYYY-MM-DD.md` (e.g., `2024-01-15.md`)
- **Project folders:** PascalCase (e.g., `MyProject/`)
- **General notes:** kebab-case (e.g., `meeting-notes.md`)
- **Templates:** Title Case with space (e.g., `Daily Template.md`)

## Heading Structure

- H1 (`#`) for note title only - one per file
- H2 (`##`) for major sections
- H3 (`###`) for subsections
- Never skip heading levels (no H1 -> H3)

## Links

### Internal Links (Wiki-style)
```markdown
[[Note Name]]                    # Link to note
[[Note Name|Display Text]]       # Link with alias
[[Note Name#Section]]            # Link to heading
[[Folder/Note Name]]             # Link with path
```

### External Links
```markdown
[Display Text](https://url.com)
```

## Tags

**Full reference:** See `.claude/tags/tag-registry.md` for complete tag conventions.

### Key Principles

- **Use prefixes:** Every tag needs a category (`#topic/`, `#concept/`, etc.)
- **Lowercase, hyphens:** `#topic/machine-learning`, not `#topic/ML`
- **No redundancy:** Use frontmatter for `status:`, tags for everything else

### Tag Placement

**Preferred:** YAML frontmatter list format
```yaml
tags:
  - source/transcript
  - type/atomic
  - topic/machine-learning
```

**Also valid:** Inline at end of note body
```markdown
#topic/productivity #concept/time-blocking
```

### Deprecated Tags

Do not use — these are replaced:
- `#status/*` → Use frontmatter `status:` field
- `#priority/*` → Use task syntax or frontmatter
- `#context/*` → Use `#topic/` or remove
- `#field/*` → Use `#domain/`

## Task Format

```markdown
- [ ] Incomplete task
- [x] Completed task
- [ ] Task with context #work @home
- [ ] Task with due date 📅 2024-01-20
```

## YAML Frontmatter

All notes should include frontmatter:
```yaml
---
date: YYYY-MM-DD
status: active
tags:
  - source/{type}
  - type/{note-type}
  - topic/{subject}
---
```

### Frontmatter Fields

| Field | Purpose | Values |
|-------|---------|--------|
| `date` | Creation date | `YYYY-MM-DD` |
| `status` | Lifecycle state | `active`, `waiting`, `completed`, `archived`, `inbox` |
| `source` | Link to source | `[[wiki-link]]` (optional) |
| `tags` | Tag array | See tag-registry.md |

## Text Formatting

- **Bold** for emphasis and key terms
- *Italic* for subtle emphasis
- `Code` for commands, paths, technical terms
- > Blockquotes for important callouts

## Lists

- Use `-` for unordered lists
- Use `1.` for ordered lists
- Indent with 2 spaces for nested items

## Code Blocks

Use fenced code blocks with language:
```javascript
const example = "code";
```

## Best Practices

1. One idea per paragraph
2. Use blank lines between sections
3. Keep lines under 100 characters when possible
4. Include links to related notes
5. Add meaningful frontmatter
