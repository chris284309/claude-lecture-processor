# Configuration Guide

## Command Options

### /lecture Options

| Option | Default | Description |
|--------|---------|-------------|
| `--quick` | false | Summary only, skip atomic/MOC/links |
| `--skip-slides` | false | Don't detect or offer slide conversion |
| `--skip-summary` | false | Atomic notes only |
| `--skip-moc` | false | No MOC creation |
| `--skip-links` | false | No connection suggestions |
| `--no-preprocess` | false | Skip transcript cleaning |
| `--atomic-count` | 12-18 | Override atomic note target range |

### /pdf-to-images Options

| Option | Default | Description |
|--------|---------|-------------|
| `--output` | `Folien Bilder/` | Output directory |
| `--scale` | 2.5 | Render scale (higher = better quality) |
| `--prefix` | `Slide_` | Filename prefix |
| `--format` | png | Output format (png, jpg) |

## Style Guide Configuration

Edit `.claude/skills/lecture-processing-style.md` to customize:

### Atomic Note Targets

```markdown
| Lecture Length | Target Atomic Notes |
|----------------|---------------------|
| 45 minutes | 8-10 notes |
| 60 minutes | 10-12 notes |
| **90 minutes** | **12-18 notes** |
| 120 minutes | 16-22 notes |
```

### Summary Length

```markdown
| Lecture Length | Summary Target |
|----------------|----------------|
| 45 minutes | 150-200 lines |
| 60 minutes | 200-250 lines |
| **90 minutes** | **250-300 lines** |
| 120 minutes | 300-350 lines |
```

### Mermaid Diagram

- Maximum **15 nodes**
- No subgraphs (keep flat)
- Short labels (2-4 words)

### MOC Sections

- Target: **4-6 sections** per MOC
- Each section: prose intro + table

## Tag System

Edit `.claude/tags/tag-registry.md` to customize tags.

### Default Prefixes

| Prefix | Purpose | Examples |
|--------|---------|----------|
| `#source/` | Content origin | transcript, paper, book |
| `#type/` | Note type | atomic, summary, moc |
| `#topic/` | Subject matter | machine-learning, logic |
| `#domain/` | Academic field | computer-science, philosophy |
| `#author/` | Knowledge source | andy-matuschak |

### Adding New Tags

1. Add to `tag-registry.md` under appropriate prefix
2. Use lowercase, hyphens between words
3. Nested topics: `#topic/ml/backpropagation`

## Folder Naming

### Slide Folders

Default (German naming):
- `Folien/` - PDF slides
- `Folien Bilder/` - Converted images

To change, edit `.claude/commands/pdf-to-images.md`:
- Change `Folien Bilder/` to your preferred name
- Update the detection logic in `/lecture` command

### Output Locations

Atomic notes are saved in:
1. `{lecture-folder}/Atomic/` (preferred)
2. `Resources/Transcripts/Atomic/` (fallback)

To change, edit `.claude/agents/atomic-extractor.md`.

## Reference Example (Optional)

After processing your first lecture, you can set it as a reference:

1. Edit `.claude/skills/lecture-processing-style.md`
2. Update the Reference Sources table:

```markdown
| Aspect | Reference |
|--------|-----------|
| Atomic note granularity | `YourCourse/Lecture 01/Atomic/` |
| Mermaid diagram style | `YourCourse/Lecture 01/MOC - Topic.md` |
```

This is optional - the style guide contains all necessary formatting rules.
