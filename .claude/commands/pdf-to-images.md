# /pdf-to-images - Convert PDF Slides to Images

Convert each page of a PDF file into individual PNG images. Ideal for lecture slides, presentations, or any multi-page PDF.

## Usage

```
/pdf-to-images {pdf-path}
```

Or from a folder containing a PDF:
```
/pdf-to-images
```
Auto-detects PDF in current directory or `Folien/` subfolder.

## Examples

```
/pdf-to-images Folien/lecture-slides.pdf
```
Creates images in `Folien Bilder/` folder.

```
/pdf-to-images ~/Downloads/presentation.pdf --output ./slides
```
Creates images in specified output folder.

```
/pdf-to-images --scale 3
```
Higher resolution output (default: 2.5).

## Process

1. **Locate PDF** - Use provided path or auto-detect in current folder
2. **Create output folder** - `Folien Bilder/` next to PDF (or custom path)
3. **Convert pages** - Each page becomes a PNG image
4. **Name sequentially** - `Slide_01.png`, `Slide_02.png`, etc.

## Implementation

Use pypdfium2 for conversion (no external dependencies like poppler):

```python
import pypdfium2 as pdfium
import os

def convert_pdf_to_images(pdf_path, output_dir, scale=2.5, max_dimension=1800):
    """Convert PDF pages to PNG images.

    Args:
        max_dimension: Resize images so largest dimension <= this value.
                      Default 1800px ensures compatibility with Claude API
                      multi-image requests (2000px limit).
    """
    os.makedirs(output_dir, exist_ok=True)

    pdf = pdfium.PdfDocument(pdf_path)

    for i in range(len(pdf)):
        page = pdf[i]
        bitmap = page.render(scale=scale)
        pil_image = bitmap.to_pil()

        # Resize if exceeds max_dimension (for Claude API compatibility)
        if max_dimension and max(pil_image.size) > max_dimension:
            pil_image.thumbnail((max_dimension, max_dimension))

        output_path = os.path.join(output_dir, f"Slide_{i+1:02d}.png")
        pil_image.save(output_path, "PNG")

    return len(pdf)
```

**IMPORTANT:** Images are auto-resized to max 1800px to stay under Claude API's 2000px limit for multi-image requests.

## Options

| Option | Default | Description |
|--------|---------|-------------|
| `--output` | `Folien Bilder/` | Output directory for images |
| `--scale` | `2.5` | Render scale (~180 DPI). Higher = better quality |
| `--prefix` | `Slide_` | Filename prefix for output images |
| `--format` | `png` | Output format (png, jpg) |
| `--max-dimension` | `1800` | Max pixel dimension (for Claude API compatibility) |

## Scale Reference

| Scale | Approx DPI | Use Case |
|-------|------------|----------|
| 1.0 | 72 | Quick preview |
| 2.0 | 144 | Screen viewing |
| 2.5 | 180 | Good balance (default) |
| 3.0 | 216 | High quality |
| 4.0 | 288 | Print quality |

## Output

Creates PNG files in the output folder:
```
Folien Bilder/
├── Slide_01.png
├── Slide_02.png
├── Slide_03.png
└── ...
```

Default location: `Folien Bilder/` as sibling to PDF or `Folien/` folder.

## Auto-Detection Logic

When no PDF path is provided:
1. Look for `*.pdf` in current directory
2. Look for `*.pdf` in `Folien/` subfolder
3. If multiple PDFs found, ask user to specify

## Dependencies

Requires `pypdfium2`:
```bash
pip3 install pypdfium2
```

## Integration

Works well with lecture processing workflow:
- After downloading lecture slides
- Before `/process-transcript` for referencing slides
- Creates images that can be embedded in Obsidian notes

## Tips

- Use scale 2.5 for good quality without huge file sizes
- Higher scale (3.0+) for slides with small text
- Output folder is created automatically if it doesn't exist
- Existing files with same names will be overwritten
