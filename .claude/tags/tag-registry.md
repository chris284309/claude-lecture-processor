# Tag Registry

**Single source of truth for all tags in the vault.**

All agents, commands, and documentation MUST reference this file for tag conventions. Do not define tag hierarchies elsewhere.

---

## Design Principles

1. **Tags for discoverability** — Click to find related notes
2. **Frontmatter for metadata** — Queryable with Dataview
3. **No redundancy** — Each piece of info lives in ONE place
4. **Prefixes required** — Every tag has a category prefix

---

## Frontmatter Fields vs Tags

To eliminate redundancy, use this division:

### Use Frontmatter Fields (NOT tags) for:

| Field | Purpose | Values |
|-------|---------|--------|
| `status` | Note lifecycle | `active`, `waiting`, `completed`, `archived`, `inbox` |
| `date` | Creation date | `YYYY-MM-DD` |
| `author` | Speaker/writer | Free text |
| `source` | Link to source | `[[wiki-link]]` |
| `slides` | Slide numbers | `[1, 2, 3]` array |

### Use Tags (NOT frontmatter) for:

Everything else — tags are clickable and visible in note body.

---

## Required Tag Prefixes

Every note should have at least ONE tag from each required category.

### #source/ — Content Origin

Where the content came from.

| Tag | Use For |
|-----|---------|
| `#source/transcript` | Lecture/meeting transcripts |
| `#source/paper` | Academic papers, journal articles |
| `#source/book` | Books, book chapters |
| `#source/article` | Blog posts, news articles |
| `#source/meeting` | Meeting notes |
| `#source/podcast` | Podcast episodes |
| `#source/video` | Video content, webinars |
| `#source/course` | Online courses |
| `#source/conversation` | Discussions, interviews |

### #type/ — Note Type

What kind of note this is.

| Tag | Use For |
|-----|---------|
| `#type/atomic` | Single-idea Zettelkasten notes |
| `#type/summary` | Condensed summaries of longer content |
| `#type/literature-note` | Academic literature notes |
| `#type/idea` | Captured ideas, not yet processed |
| `#type/moc` | Maps of Content |
| `#type/project` | Project notes |
| `#type/daily` | Daily notes |
| `#type/meeting` | Meeting notes |
| `#type/reference` | Reference material |

### #topic/ — Subject Matter

What the note is about. **Supports nesting for specificity.**

**Naming:** Lowercase, hyphens, no abbreviations, singular.

#### Flat Topics (Broad)

```
#topic/machine-learning
#topic/productivity
#topic/philosophy
#topic/qualitative-research
```

#### Nested Topics (Specific)

Use nesting when one topic is clearly a subset of another:

```
#topic/machine-learning/neural-networks
#topic/machine-learning/backpropagation
#topic/philosophy/ethics
#topic/philosophy/categorical-imperative
#topic/productivity/time-blocking
```

**How Obsidian handles nested tags:**
- Click `#topic` → shows ALL notes with any topic tag
- Click `#topic/machine-learning` → shows this AND all children (neural-networks, backpropagation)
- Click `#topic/machine-learning/backpropagation` → shows only this specific tag

#### When to Nest vs Use Separate Tags

**Nest** when one is clearly a subset:
- `#topic/machine-learning/backpropagation` ✓

**Use separate tags** for orthogonal dimensions:
- `#topic/machine-learning` + `#topic/qualitative-research` ✓
- NOT `#topic/qualitative-research/machine-learning` ✗

**Rule:** If unsure, use separate flat tags. MOCs can show relationships.

---

## Recommended Tag Prefixes

Use when applicable. Not required on every note.

### #domain/ — Academic Discipline

Broad academic fields. Use when the note relates to formal scholarship.

| Tag | Field |
|-----|-------|
| `#domain/computer-science` | CS, programming, algorithms |
| `#domain/philosophy` | Philosophy, logic, ethics |
| `#domain/psychology` | Psychology, cognitive science |
| `#domain/economics` | Economics, finance |
| `#domain/medicine` | Medicine, public health |
| `#domain/statistics` | Statistics, data science |
| `#domain/education` | Pedagogy, learning science |

### #author/ — Knowledge Sources

Authors, researchers, thinkers whose work informs the notes. Not personal contacts.

Format: `#author/firstname-lastname`

Examples:
- `#author/niklas-luhmann`
- `#author/andy-matuschak`
- `#author/sönke-ahrens`

---

## Deprecated Prefixes

Do NOT use these. They have been replaced.

| Deprecated | Use Instead |
|------------|-------------|
| `#concept/` | Nested `#topic/` (e.g., `#topic/ml/backpropagation`) |
| `#research/` | `#topic/` (e.g., `#topic/literature-review`) |
| `#method/` | `#topic/` (e.g., `#topic/qualitative-research`) |
| `#field/` | `#domain/` |
| `#status/` | Frontmatter `status:` field |
| `#priority/` | Frontmatter or task syntax |
| `#context/` | Remove or use `#topic/` |
| `#person/` | `#author/` |

---

## Naming Conventions

### Format Rules

| Rule | Correct | Incorrect |
|------|---------|-----------|
| Lowercase always | `#topic/machine-learning` | `#topic/Machine-Learning` |
| Hyphens for multi-word | `#topic/neural-networks` | `#topic/neural_networks` |
| No abbreviations | `#topic/artificial-intelligence` | `#topic/ai` |
| No spaces | `#topic/machine-learning` | `#topic/machine learning` |
| Prefix required | `#topic/productivity` | `#productivity` |
| Use standard term | `#topic/neural-networks` | `#topic/neural-network` |
| Nesting with slashes | `#topic/ml/backpropagation` | `#topic/ml-backpropagation` |

### Exceptions & Clarifications

- **Acronyms as terms:** `#topic/api`, `#topic/sql` (when acronym IS the term)
- **Use standard terminology:** Prefer how the term is commonly used in the field
  - `#topic/neural-networks` ✓ (standard term)
  - `#topic/machine-learning` ✓ (standard term)
  - `#topic/ethics` ✓ (inherently singular)

---

## Tag Selection Guide

### For Atomic Notes

Required:
- 1× `#source/` — where it came from
- 1× `#type/atomic`
- 1× `#topic/` — main subject (can be nested for specificity)

Recommended:
- 1× `#domain/` — if academic content
- 1× `#author/` — if attributable to specific thinker

Example:
```
#source/transcript
#type/atomic
#topic/machine-learning/backpropagation
#domain/computer-science
```

### For Summaries

Required:
- 1× `#source/` — source type
- 1× `#type/summary`
- 1× `#topic/` — main subject

### For Literature Notes

Required:
- 1× `#source/paper` (or book, article)
- 1× `#type/literature-note`
- 1× `#topic/` — main subject
- 1× `#domain/` — academic field

Recommended:
- 1× `#author/` — paper author(s)

### For MOCs

Required:
- 1× `#type/moc`
- 1× `#topic/` — the topic being mapped

---

## Validation Process

### Before Creating Tags

1. **Check this registry** — Is the prefix valid?
2. **Search vault** — Does a similar tag already exist?
3. **Follow conventions** — Lowercase, hyphens, no abbreviations

### Creating New Tags

New tags within established prefixes are fine. Just:
- Follow naming conventions
- Prefer existing synonyms
- Tag-manager will audit weekly

### Weekly Audit (tag-manager)

During weekly review, tag-manager:
1. Scans for convention violations
2. Merges obvious duplicates
3. Flags ambiguous cases for Christian
4. Reports new tags created

---

## Frontmatter Template

Standard frontmatter for most notes:

```yaml
---
date: YYYY-MM-DD
status: active
source: "[[Source Note]]"
---
```

Tags go in the note body or YAML `tags:` array:

```yaml
---
date: 2026-01-01
status: active
tags:
  - source/transcript
  - type/atomic
  - topic/machine-learning/neural-networks
  - domain/computer-science
---
```

---

## Migration Notes

### From Old System

If encountering old tags:
- `#concept/X` → `#topic/parent/X` (nest under appropriate parent topic)
- `#research/X` → `#topic/X` (e.g., `#topic/literature-review`)
- `#method/X` → `#topic/X` (e.g., `#topic/qualitative-research`)
- `#field/X` → `#domain/X`
- `#person/X` → `#author/X`
- `#status/X` → Remove, use frontmatter `status: X`
- `#priority/X` → Remove, use task syntax or frontmatter
- `#context/X` → Evaluate: convert to `#topic/` or remove

### Agent Compatibility

All agents reference this registry. If an agent's documentation conflicts with this file, **this file wins**.

---

*Last Updated: 2026-01-01*
*Registry Version: 2.0 — Simplified to 5 prefixes with nested topic support*
