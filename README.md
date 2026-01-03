# Claude Lecture Processor

Transform lecture transcripts into structured knowledge using Claude Code.

## What It Does

The `/lecture` command processes lecture transcripts through a complete pipeline:

```
┌─────────────────────────────────────────────────────────────────┐
│ 1. SLIDES       Detect/convert PDF slides to images            │
│ 2. DESCRIBE     Generate slide descriptions (efficient Haiku)  │
│ 3. PREPROCESS   Clean transcript (remove filler, timestamps)   │
│ 4. SUMMARY      Create structured outline (250-300 lines)      │
│ 5. ATOMIC       Extract 12-18 concept notes                    │
│ 6. MOC          Build Map of Content with Mermaid diagram      │
│ 7. LINKS        Suggest connections to existing vault notes    │
└─────────────────────────────────────────────────────────────────┘
```

## Quick Start

### 1. Install Dependencies

```bash
pip3 install pypdfium2
```

### 2. Copy Files to Your Vault

```bash
# Copy the .claude folder to your vault root
cp -r .claude/ /path/to/your-vault/.claude/

# Copy templates
cp Templates/* /path/to/your-vault/Templates/
```

### 3. Run the Command

```bash
cd /path/to/your-vault
claude

# Then in Claude Code:
/lecture path/to/transcript.md
```

## Usage

### Basic Usage

```
/lecture path/to/transcript.md
```

Or from a lecture folder (auto-detects transcript):
```
/lecture
```

### Options

| Option | Description |
|--------|-------------|
| `--quick` | Summary only, no atomic notes or MOC |
| `--skip-slides` | Don't detect/convert slides |
| `--skip-summary` | Atomic notes only |
| `--skip-moc` | No MOC creation |
| `--skip-links` | No connection suggestions |
| `--atomic-count 8-10` | Override atomic note count (default: 12-18) |

### Examples

```bash
# Full processing with slides
/lecture Lectures/CS101/Lecture05/transcript.md

# Quick summary only
/lecture --quick transcript.md

# Fewer, coarser atomic notes
/lecture --atomic-count 8-10 transcript.md
```

## Output

For a lecture on "Machine Learning Basics", you'll get:

```
Lecture 01/
├── transcript.md                          # Your original
├── transcript-clean.md                    # Cleaned version
├── slide-descriptions.md                  # Slide content index
├── Summary - Lecture 01 Machine Learning.md
├── MOC - Machine Learning Basics.md
└── Atomic/
    ├── Neural networks learn hierarchical features.md
    ├── Backpropagation enables efficient gradient computation.md
    └── ... (10-16 more concept notes)
```

## Requirements

- **Claude Code** - The Claude CLI tool
- **Obsidian** - For viewing and linking notes
- **pypdfium2** - For PDF to image conversion (`pip3 install pypdfium2`)

## Folder Structure

See [examples/expected-folder-structure.md](examples/expected-folder-structure.md) for full details.

Minimum required:
```
your-vault/
├── .claude/           # From this repo
├── Templates/         # From this repo
└── your-transcript.md
```

## Customization

### Atomic Note Count

Default: 12-18 notes for a 90-minute lecture.

| Lecture Length | Recommended Count |
|----------------|-------------------|
| 45 minutes | 8-10 |
| 60 minutes | 10-12 |
| 90 minutes | 12-18 |
| 120 minutes | 16-22 |

Override with `--atomic-count`.

### Slide Folder Names

Default assumes German naming:
- `Folien/` for PDF files
- `Folien Bilder/` for images

Edit `.claude/commands/pdf-to-images.md` to change.

### Tags

Edit `.claude/tags/tag-registry.md` to customize the tagging system.

Default prefixes:
- `#source/` - Content origin (transcript, paper, book)
- `#type/` - Note type (atomic, summary, moc)
- `#topic/` - Subject matter
- `#domain/` - Academic field

## How It Works

### Agents

| Agent | Purpose |
|-------|---------|
| `transcript-summarizer` | Creates hierarchical outline summaries |
| `atomic-extractor` | Extracts standalone concept notes |
| `moc-builder` | Organizes notes into navigable structure |
| `link-suggester` | Finds connections to existing vault content |

### Skills

| Skill | Purpose |
|-------|---------|
| `lecture-processing-style` | Consistency rules and targets |
| `obsidian-vault-ops` | File operations and wiki-links |
| `research-workflow` | Citation and tagging standards |

## Examples

See the [examples/](examples/) folder for:
- Sample transcript input
- Sample summary output
- Sample atomic note
- Sample MOC

## Troubleshooting

### "Command not found"

Make sure Claude Code is installed and you're in a directory with a `.claude/` folder.

### Slides from wrong lecture

The command now uses full paths for slide embeds. If you see this issue:
1. Check that slide links use format: `![[Course/Lecture 01/Folien Bilder/Slide_01.png]]`
2. Not short format: `![[Slide_01.png]]`

### pypdfium2 not found

```bash
pip3 install pypdfium2
```

## License

MIT License - See [LICENSE](LICENSE)

## Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request

## Acknowledgments

Built for use with:
- [Claude Code](https://claude.ai/code) by Anthropic
- [Obsidian](https://obsidian.md) for note-taking
