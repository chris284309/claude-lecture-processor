# Claude Lecture Processor - Project Context

## What This Is
Animated GIF visualization of the `/lecture` command pipeline for the GitHub README.

## Design Specs

| Property | Value |
|----------|-------|
| Dimensions | 800 × 450 px |
| Frame duration | 4 seconds each |
| Total frames | 8 |
| Total loop | 32 seconds |
| Style | Minimalist, professional |

### Color Palette
| Color | Hex | Usage |
|-------|-----|-------|
| Teal | `#0D9488` | Primary (boxes, text, arrows) |
| Light Teal | `#5EEAD4` | Accents, node fills |
| Dark Teal | `#134E4A` | Outlines, emphasis |
| Dark Gray | `#1F2937` | Body text |
| Light Gray | `#F3F4F6` | Input box backgrounds |
| White | `#FFFFFF` | Background |

## Frame Structure

1. **Title** - "Claude Lecture Processor" + tagline + GitHub URL
2. **Raw Lecture** - Transcript (wavy lines) + PDF Slides (grid)
3. **Processing** - Vertical flow: Raw → Clean for both transcript and slides, converging to "Extract Concepts"
4. **Cleaned & Indexed** - Clean text (straight lines) + numbered slide thumbnails
5. **Structured Knowledge** - Summary outline + 4 atomic note cards (A1-A4)
6. **Map of Content** - Section 1, 2, 3 boxes with Concept A-F, connected by arrows
7. **Automatic Tagging** - Tag prefixes (#source/, #type/, #topic/, #domain/) with example values
8. **Complete Transformation** - INPUT (Transcript, PDF Slides) → OUTPUT (Summary, Atomic Notes, MOC, Tags)

## Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| 4s per frame | Tested 1.2s (too fast), 2.5s (still fast) - 4s allows reading |
| Title card first | Better hook when GIF starts |
| Vertical flow in Processing | Shows transformation clearly (what goes in → what comes out) |
| Section 1/2/3 labels | Generic labels work better than specific names (Foundations/Methods/etc.) |
| Larger subtitle text | 18px for readability (was 12-14px) |
| 8 frames total | Added Tags + Overview frames to show complete pipeline |

## Local Files (gitignored)

- `design-philosophy.md` - Visual philosophy document
- `lecture-pipeline-static.png` - Static first frame for fallback

## Regenerating the GIF

The GIF is generated with Python/PIL. Key code pattern:
```python
from PIL import Image, ImageDraw, ImageFont

WIDTH, HEIGHT = 800, 450
TEAL = (13, 148, 136)
# ... create frames as PIL Images ...
frames[0].save("lecture-pipeline.gif", save_all=True,
               append_images=frames[1:], duration=4000, loop=0)
```

To modify: Regenerate all 8 frames with updated content, save as GIF.

## History

- 2026-01-06: Initial creation with 6 frames
- 2026-01-06: Added frames 7 (Tags) and 8 (Overview)
- 2026-01-06: Adjusted timing to 4s per frame
- 2026-01-06: Fixed text overlaps, improved MOC visualization
