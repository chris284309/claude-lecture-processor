# Expected Folder Structure

The `/lecture` command expects your vault to have a structure similar to this:

```
your-vault/
├── .claude/                          # Claude Code configuration (copy from this repo)
│   ├── commands/
│   ├── agents/
│   ├── skills/
│   ├── rules/
│   └── tags/
│
├── Templates/                        # Note templates (copy from this repo)
│   ├── Lecture MOC Template.md
│   ├── Lecture Atomic Note Template.md
│   ├── MOC Template.md
│   └── Transcript Summary Template.md
│
├── Resources/                        # Optional: organized resources
│   └── Transcripts/
│       └── Atomic/                   # Fallback location for atomic notes
│
└── {Your Lecture Folders}/           # Where you store lecture materials
    └── Lecture 01/
        ├── transcript.md             # Your lecture transcript
        ├── Folien/                   # Optional: PDF slides folder
        │   └── slides.pdf
        ├── Folien Bilder/            # Created by /pdf-to-images
        │   ├── Slide_01.png
        │   ├── Slide_02.png
        │   └── ...
        ├── slide-descriptions.md     # Created by /lecture
        ├── transcript-clean.md       # Created by /lecture
        ├── Summary - Lecture 01.md   # Created by /lecture
        ├── MOC - {Topic}.md          # Created by /lecture
        └── Atomic/                   # Created by /lecture
            ├── {Concept 1}.md
            └── {Concept 2}.md
```

## Folder Naming Conventions

### Slide Folders
- `Folien/` - German for "slides", contains PDF files
- `Folien Bilder/` - German for "slide images", contains PNG conversions

You can use different names by adjusting the commands.

### Lecture Folders
Any structure works. The command detects transcripts and slides automatically.

Examples:
- `Courses/CS101/Lecture 01/`
- `Lectures/Machine Learning/Week 5/`
- `Resources/Transcripts/2024-01-15 AI Lecture/`

## Minimum Required Structure

At minimum, you need:
```
your-vault/
├── .claude/           # From this repo
├── Templates/         # From this repo
└── transcript.md      # Your transcript file
```

The command will create all other folders as needed.
