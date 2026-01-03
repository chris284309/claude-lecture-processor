# /process-transcript - Unified Transcript Processing

Process a transcript using one of two modes: summarize or atomize.

## Usage

```
/process-transcript {file-path-or-content}
```

Or just:
```
/process-transcript
```
Then provide the transcript when prompted.

## Processing Modes

When invoked, Claude will ask which processing mode to use:

### 1. Summary Mode
**Best for:** Quick understanding, sharing key points, meeting notes

Creates a condensed summary note with:
- Overview (2-3 sentences)
- 3-5 key insights
- Main points
- Action items
- Connections to existing notes

**Output:** Same folder as transcript: `Summary - {Title}.md`

**Uses agent:** `transcript-summarizer`

---

### 2. Atomic Mode
**Best for:** Building permanent knowledge, Zettelkasten workflow, deep learning

Extracts individual ideas into separate atomic notes:
- One idea per note
- Written in own words
- Tagged with topics/concepts
- Linked to related notes
- Linked back to source

**Output:** Same folder as transcript in `Atomic/` subfolder (or `Resources/Transcripts/Atomic/` if no clear folder)

**Uses agent:** `atomic-extractor`

---

## Workflow

```
the user: /process-transcript

Claude: Please provide the transcript (file path or paste content).

the user: [provides transcript]

Claude: I've analyzed the transcript: "{Title}" ({duration/length})

How would you like to process it?

1. **Summary** - Condensed overview with key insights
2. **Atomic** - Break into individual linked notes

the user: 2

Claude: [Routes to atomic-extractor agent]
```

## Options

```
/process-transcript --mode summary {path}    # Skip mode selection
/process-transcript --mode atomic {path}
```

## Examples

```
/process-transcript ~/Downloads/lecture-notes.txt
```

```
/process-transcript --mode atomic Resources/raw/seminar-2024-12-28.md
```

```
/process-transcript
> [paste transcript content]
```

## Decision Guide

| If you want to... | Choose |
|-------------------|--------|
| Quickly capture main points | Summary |
| Build reusable knowledge units | Atomic |
| Share key takeaways with others | Summary |
| Connect ideas to existing notes | Atomic |

## Multi-Mode Processing

You can process the same transcript multiple ways:

```
/process-transcript --mode summary lecture.md
/process-transcript --mode atomic lecture.md
```

This creates both a summary and atomic notes from the same source.

## Integration

All modes:
- Check existing vault tags before creating new ones
- Tags are audited by `tag-manager` during weekly review
- Suggest connections via `link-suggester` after processing
- Store in appropriate `Resources/Transcripts/` subfolder
