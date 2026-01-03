# /lecture - End-to-End Lecture Processing

Process a lecture transcript with full pipeline: slides, summary, atomic notes, MOC, and connections.

## Usage

```
/lecture {transcript-path}
```

Or from a lecture folder:
```
/lecture
```
Auto-detects transcript in current directory.

## What It Does

Orchestrates the complete lecture processing workflow:

```
┌─────────────────────────────────────────────────────────────────┐
│ 1. SLIDES       Detect/convert PDF slides                       │
│ 2. DESCRIBE     Generate slide-descriptions.md (MANDATORY)      │
│ 3. PREPROCESS   Clean transcript (remove filler, timestamps)    │
│ 4. CALIBRATE    Read style guide + reference example            │
│ 5. SUMMARY      Create structured outline                       │
│ 6. ATOMIC       Extract 12-18 notes (uses slide-descriptions)   │
│ 7. MOC          Build Map of Content (4-6 sections, ≤15 nodes)  │
│ 8. LINKS        Connect to existing vault notes                 │
│ 9. VERIFY       Check consistency with style guide              │
└─────────────────────────────────────────────────────────────────┘
```

## Workflow

### Step 1: Slide Detection

Check for existing slide images or PDF to convert:

1. Look for `Folien Bilder/` folder with `Slide_*.png` files
2. If not found, look for PDF in `Folien/` folder or current directory
3. If PDF found, offer to run `/pdf-to-images`
4. If no slides available, continue without (webinar mode)

**Output:** Slide count and location, or "No slides detected"

### Step 2: Generate Slide Descriptions (MANDATORY)

**If slides exist, this step is REQUIRED.** The `slide-descriptions.md` file is essential for efficient slide-to-concept matching later. Without it, the atomic-extractor would need to view every slide image repeatedly.

If slides are available, generate `slide-descriptions.md`:

1. **Resize images for API** (see below)
2. **View each slide image** in `Folien Bilder/`
3. Write a brief description (1-2 sentences) of each slide's content
4. Save as `slide-descriptions.md` in the lecture folder

**Model:** Use **Haiku** for this step (sufficient for visual description, ~10x cheaper than Sonnet).

#### Image Resizing for Multi-Image Requests

**CRITICAL:** Haiku has a **2000px max dimension limit** for multi-image requests. Before sending slides to Haiku, resize any image exceeding this limit.

```python
from PIL import Image
import io

def resize_for_api(image_path, max_dim=1900):
    """Resize image if any dimension exceeds API limit.

    Returns PIL Image object (resized or original).
    Does NOT modify the original file.
    """
    img = Image.open(image_path)
    if max(img.size) <= max_dim:
        return img  # No resize needed

    # Calculate new dimensions preserving aspect ratio
    ratio = max_dim / max(img.size)
    new_size = (int(img.width * ratio), int(img.height * ratio))
    return img.resize(new_size, Image.LANCZOS)
```

**Usage:** Apply this to each slide before including in the Haiku request. The original high-resolution images remain unchanged on disk.

**Output format:**
```markdown
# Slide Descriptions

Generated from: {PDF filename}
Slides: {N} total

| Slide | Content |
|-------|---------|
| 01 | Title slide: {Lecture title}, {Course name} |
| 02 | Agenda: {list of topics} |
| 03 | {Main concept}: {brief description of visual content} |
...
```

**Description guidelines:**
- Focus on what the slide teaches, not just what it shows
- Note key terms, formulas, diagrams, matrices
- For example slides (Prisoner's Dilemma matrix), describe the specific content
- Keep each description under 15 words (concise = fewer tokens)

**Skip if:** No slides available (webinar mode)

### Step 3: Transcript Preprocessing

Clean the transcript to reduce token usage before processing:

**Remove:**
- Timestamps: `[00:15:32]`, `(00:15:32)`, `00:15:32`
- Filler words: "um", "uh", "you know", "like", "sort of", "kind of"
- Speaker labels if single speaker: `Professor:`, `Speaker:`
- Repeated words: "the the", "and and"
- Inaudible markers: `[inaudible]`, `[crosstalk]`, `[laughter]`
- Excessive whitespace and blank lines

**Preserve:**
- All substantive content
- Technical terms and formulas
- Speaker labels if multiple speakers (dialogue)
- Meaningful pauses indicated by `...`

**Implementation:**
```
1. Read original transcript
2. Apply cleaning rules
3. Save cleaned version as `transcript-clean.md` (keep original)
4. Use cleaned version for subsequent steps
```

**Expected savings:** 10-20% token reduction on typical lecture transcripts.

**Skip if:** Transcript is already clean or `--no-preprocess` flag is used.

### Step 4: Style Calibration

**Before processing, the agent must:**

1. Read `.claude/skills/lecture-processing-style.md` (contains section format examples)
2. *(Optional)* If available, review a reference MOC from a previously processed lecture for Mermaid style
3. Note targets: **12-18 atomic notes**, 4-6 MOC sections (prose + tables), ≤15 Mermaid nodes

This ensures consistent output across all lectures.

> **Note for new users:** The first time you run `/lecture`, you won't have a reference example. That's fine - the style guide contains all necessary formatting rules. After processing your first lecture, you can optionally set it as a reference for future runs.

### Step 5: Processing Mode Selection

Ask the user:

```
Found: {transcript name} ({word count} words)
Slides: {N} images in Folien Bilder/ (or "None detected")

How should I process this lecture?

1. Summary only - Quick overview with key insights
2. Atomic only - Break into linked knowledge units
3. Full pipeline (recommended) - Summary → Atomic → MOC → Links
4. Custom - Choose which steps to run
```

### Step 6: Execute Pipeline

Based on selection, chain the agents:

#### Summary Phase
- Invoke `transcript-summarizer`
- Apply tags immediately (not left empty)
- Save in same folder as transcript

#### Atomic Phase
- Invoke `atomic-extractor`
- **CRITICAL: Slide Assignment Rules**
  - `slide-descriptions.md` MUST exist (created in Step 2)
  - Read `slide-descriptions.md` to match concepts to slides (efficient: one file read)
  - **NEVER guess from transcript** — lecturer's "this slide" references are unreliable
  - **NEVER view slides individually** — use the descriptions file
  - If descriptions file missing → STOP and create it first (Step 2 was skipped)
- **CRITICAL: Slide Linking Format**
  - **ALWAYS use full relative paths** for slide embeds to avoid cross-lecture conflicts
  - Multiple courses have identically-named slides (Slide_01.png, Slide_02.png, etc.)
  - Format: `![[{Course}/Lecture {NN}/Folien Bilder/Slide_{NN}.png]]`
  - Example: `![[AIL/Lecture 09/Folien Bilder/Slide_34.png]]`
  - **NEVER use short links** like `![[Slide_34.png]]` — Obsidian resolves ambiguously
- the user approves idea list and slide assignments
- Create notes in `Resources/Transcripts/Atomic/` or same folder
- **Note titles must be statements, not topic labels** (enforced by agent)

#### MOC Phase
- Invoke `moc-builder` (Mode 4: Post-Extraction)
- Propose structure from atomic notes
- Include Mermaid diagram if 8+ notes
- Save in same folder as transcript

#### Link Phase
- Invoke `link-suggester` (Mode 1: analyze new notes)
- Show top 5-10 connection opportunities
- Offer to add suggested links

### Step 7: Consistency Verification

Before reporting, verify outputs match style guide:

- [ ] Atomic note count within **12-18** range
- [ ] All atomic titles are statements (not topic labels)
- [ ] **Slide assignments verified** (not guessed from transcript)
- [ ] **Slide links use full paths** (e.g., `![[AIL/Lecture 09/Folien Bilder/Slide_01.png]]`)
- [ ] MOC has 4-6 sections with prose + tables
- [ ] Mermaid diagram is simple (≤15 nodes, no subgraphs)
- [ ] Section format matches style guide examples

If out of range, consolidate or expand as needed before finalizing.

### Step 8: Report Results

```markdown
## Lecture Processing Complete

**Source:** {transcript name}
**Slides:** {N} linked

### Created
- [ ] Summary: [[Summary - {Title}]]
- [ ] Atomic notes: {N} notes created
- [ ] MOC: [[MOC - {Topic}]]

### Connections Made
- {N} links added to new notes
- {N} links suggested for review

### Tags Applied
- New: {list}
- Existing reused: {list}

### Next Steps
- Review atomic notes for accuracy
- Check MOC structure
- Consider additional connections
```

## Style Consistency

**CRITICAL:** Before processing, read the style guide:

1. **Style Guide:** `.claude/skills/lecture-processing-style.md`
2. **Reference Example:** *(Optional)* Your first processed lecture's MOC and Atomic Notes
3. **Templates:** `Templates/Lecture MOC Template.md`, `Templates/Lecture Atomic Note Template.md`

### Default Targets (90-minute lecture)

| Output | Target |
|--------|--------|
| Atomic notes | **12-18 notes** (coarse concepts, not fine definitions) |
| MOC sections | **4-6 sections** (prose intro + table per section) |
| Mermaid diagram | **Simple linear flow** (≤15 nodes, no subgraphs) |
| Summary length | **250-300 lines** (~10:1 compression) |

### Atomic Note Granularity

**Extract coarse concepts.** Combine related sub-ideas into single notes.

| Too Granular (avoid) | Correct Granularity |
|----------------------|---------------------|
| 3 notes on Nash equilibrium variants | 1 note: "Nash equilibrium captures strategic stability" |
| Separate notes for definition + properties | Combined note covering both |

### Title Format

All atomic note titles must be **complete statements**, not topic labels.

---

## Options

```
/lecture --skip-slides          # Don't detect/offer slides
/lecture --skip-summary         # Atomic notes only
/lecture --skip-moc             # No MOC creation
/lecture --skip-links           # No link suggestions
/lecture --quick                # Summary only, no follow-up
/lecture --no-preprocess        # Skip transcript cleaning
/lecture --atomic-count 8-10    # Override atomic note target
/lecture --atomic-count 15-18   # More notes for dense material
```

## Examples

### Full Lecture with Slides
```
/lecture "Lecture Transcript/AIL/Lecture 08/transcript.md"

→ Found 45 slides in Folien Bilder/
→ Full pipeline selected
→ Summary created
→ 12 atomic notes extracted with slide references
→ MOC created with concept flow diagram
→ 8 connections to existing vault notes suggested
```

### Webinar Without Slides
```
/lecture ~/Downloads/webinar-notes.txt

→ No slides detected
→ Full pipeline selected
→ Summary created
→ 7 atomic notes extracted
→ MOC created
→ 5 connections suggested
```

### Quick Summary Only
```
/lecture --quick transcript.md

→ Summary created
→ Done (no atomic/MOC/links)
```

## Integration

This command orchestrates:
- `/pdf-to-images` - Slide conversion
- `transcript-summarizer` - Summary creation
- `atomic-extractor` - Atomic note extraction
- `moc-builder` - MOC creation (Mode 4)
- `link-suggester` - Connection discovery

## Folder Structure Expected

Typical lecture folder:
```
Lecture 08/
├── transcript.md             # Original transcript (preserved)
├── transcript-clean.md       # Preprocessed transcript (used for processing)
├── Folien/
│   └── slides.pdf            # Original PDF (optional)
├── Folien Bilder/            # Converted slide images
│   ├── Slide_01.png
│   ├── Slide_02.png
│   └── ...
├── slide-descriptions.md     # Generated descriptions (via Haiku)
├── Summary - Lecture 08.md   # Created by this command
├── MOC - {Topic}.md          # Created by this command
└── Atomic/                   # Or in Resources/Transcripts/Atomic/
    ├── {Idea 1}.md
    └── {Idea 2}.md
```

## When to Use

| Scenario | Command |
|----------|---------|
| Full lecture processing | `/lecture` |
| Just need a quick summary | `/process-transcript --mode summary` |
| Only want atomic notes | `/process-transcript --mode atomic` |
| Already have summary, want atomic | Use `atomic-extractor` agent directly |

## Comparison with /process-transcript

| Feature | /lecture | /process-transcript |
|---------|----------|---------------------|
| Slide detection | Yes | No |
| Summary | Yes | Yes (mode) |
| Atomic notes | Yes | Yes (mode) |
| MOC creation | Yes | No |
| Link suggestions | Yes | Mentioned but not triggered |
| Chained pipeline | Yes | Single mode only |

## Token Optimization

This command includes optimizations to reduce token usage for long lectures:

| Optimization | Savings | How |
|--------------|---------|-----|
| Haiku for slides | ~60-70% on slide descriptions | Cheaper model, sufficient for visual description |
| Transcript preprocessing | ~10-20% on transcript | Remove filler, timestamps, repeated words |
| Image resizing | Prevents API errors | Resize images >2000px before Haiku request |

**For a 90-minute lecture with 50 slides:**
- Without optimization: ~150k tokens
- With optimization: ~80k tokens (~47% reduction)

**Note:** Multi-image requests to Haiku require images ≤2000px on any dimension. The resize step in Step 2 handles this automatically without modifying original files.
