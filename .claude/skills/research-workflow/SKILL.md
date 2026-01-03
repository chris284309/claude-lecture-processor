---
name: research-workflow
description: Standards for academic research in the vault. Citation formats, literature note structure, linking patterns, and research organization. Use when processing papers, creating citations, or working with academic content.
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Research Workflow Skill

Standards and conventions for academic research within the Obsidian vault.

## Folder Structure

```
Resources/
├── Literature/          # Literature notes (one per paper/book)
├── Transcripts/
│   ├── Summaries/      # Condensed transcript summaries
│   └── Atomic/         # Extracted atomic notes
└── Ideas/              # Captured research ideas
```

## Literature Note Standards

### File Naming

```
{AuthorLastName}{Year} - {ShortTitle}.md
```

Examples:
- `Smith2023 - Machine Learning Ethics.md`
- `Johnson2022 - Qualitative Methods.md`

### Required Frontmatter

```yaml
---
date: YYYY-MM-DD
tags: [source/paper, research/literature]
status: active
type: literature-note
authors: [Last, First; Last, First]
year: YYYY
---
```

### Citation Format

Use APA-style citations in notes:

**In-text:** (Author, Year)
**Full citation block:**
```
Author, A. A., & Author, B. B. (Year). Title of article.
Journal Name, Volume(Issue), pages. https://doi.org/xxx
```

## Linking Patterns

### Connecting Literature Notes

- Link to related papers: `[[Author2023 - Title]]`
- Link to concepts: `[[Concept Name]]`
- Use MOC (Map of Content) notes for topics: `[[MOC - Topic Name]]`

### Tag Hierarchy for Research

**Reference:** See `.claude/tags/tag-registry.md` for complete conventions.

```
#source/
  paper          # Academic papers
  book           # Books and chapters
  article        # Articles and essays
  transcript     # Lecture/seminar transcripts

#type/
  literature-note  # Literature notes
  summary          # Summaries
  atomic           # Atomic notes

#topic/
  methodology      # Methods-related
  literature-review # Literature review work

#domain/
  {discipline}     # e.g., #domain/psychology
```

**Note:** `#research/` and `#field/` prefixes are deprecated. Use `#type/`, `#topic/`, and `#domain/` instead.

## Processing Workflow

### For Research Papers

1. Create literature note in `Resources/Literature/`
2. Fill in citation information
3. Summarize key arguments (not the abstract)
4. Note methodology
5. Extract relevance to current research
6. Link to related notes
7. Add to relevant MOC

### For Transcripts

1. Determine processing type (summary, atomic, or tagged)
2. Create note in appropriate subfolder
3. Tag with `#source/transcript`
4. Link to original source
5. Extract actionable insights

## Quality Standards

### Good Literature Notes

- Focus on *your* understanding, not just summarizing
- Highlight relevance to *your* research
- Include direct quotes with page numbers
- Note limitations and critiques
- Identify gaps for future exploration

### Avoid

- Copying abstracts verbatim
- Notes without connections to other notes
- Missing citations
- Orphan notes (no links in or out)

## Templates

Use templates from `Templates/` folder:
- `Literature Note Template.md`
- `Transcript Summary Template.md`
- `Idea Template.md`

## Integration

Works with agents:
- `literature-processor` - Creates literature notes
- `transcript-summarizer` - Summarizes transcripts
- `atomic-extractor` - Creates atomic notes
- `idea-capture` - Captures research ideas
