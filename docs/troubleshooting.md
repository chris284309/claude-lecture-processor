# Troubleshooting

## Common Issues

### "Command not found" or "/lecture doesn't work"

**Cause:** Claude Code can't find the command file.

**Solution:**
1. Ensure `.claude/commands/lecture.md` exists in your vault root
2. Make sure you're running Claude Code from the vault directory
3. Try restarting Claude Code

### Slides from wrong lecture appear in atomic notes

**Cause:** Short slide links like `![[Slide_01.png]]` are ambiguous when multiple lectures have similarly-named slides.

**Solution:**
The command uses full paths like `![[Course/Lecture 01/Folien Bilder/Slide_01.png]]`. If you see wrong slides:

1. Check the atomic notes use full paths
2. If not, the slide path format may need updating
3. Re-run the command or manually fix the paths

### pypdfium2 not installed

**Error:** `ModuleNotFoundError: No module named 'pypdfium2'`

**Solution:**
```bash
pip3 install pypdfium2
```

### PDF conversion fails

**Possible causes:**
- Corrupted PDF file
- PDF with unusual encoding
- Missing dependencies

**Solutions:**
1. Try opening the PDF in a viewer to verify it's valid
2. Re-download the PDF if possible
3. Check for error messages in Claude Code output

### Atomic notes have wrong slide numbers

**Cause:** The slide-descriptions.md file may be missing or outdated.

**Solution:**
1. Delete `slide-descriptions.md` if it exists
2. Re-run `/lecture` - it will regenerate descriptions
3. Verify the descriptions match the actual slides

### Too many/few atomic notes

**Cause:** Default count (12-18) doesn't match your lecture.

**Solution:**
Use the `--atomic-count` option:
```
/lecture --atomic-count 8-10 transcript.md   # Fewer notes
/lecture --atomic-count 15-20 transcript.md  # More notes
```

### Summary is too long/short

**Expected:** ~250-300 lines for 90-minute lecture.

If significantly different:
1. Check transcript quality (garbage in = garbage out)
2. Transcript may have unusual structure
3. Re-run with fresh context

### MOC has too many sections

**Expected:** 4-6 sections.

**Solution:**
The MOC is generated from atomic notes. If there are too many sections:
1. Check if atomic notes are too fine-grained
2. Consider using `--atomic-count 8-10` for coarser notes
3. Manually consolidate MOC sections

### Links to non-existent notes

**Cause:** Atomic notes reference each other using wiki-links. If some notes weren't created, links will be broken.

**Solution:**
1. Check all atomic notes were created in `Atomic/` folder
2. Verify note titles match the links exactly
3. Obsidian will show broken links with different styling

### Transcript preprocessing removes too much

**Cause:** The preprocessing step may be too aggressive for some transcripts.

**Solution:**
Use `--no-preprocess` to skip cleaning:
```
/lecture --no-preprocess transcript.md
```

### Command hangs or times out

**Possible causes:**
- Very long transcript (>15,000 words)
- Network issues with Claude API
- Complex PDF with many slides

**Solutions:**
1. For long transcripts, try `--quick` first for faster feedback
2. Split very long transcripts into parts
3. Check your network connection
4. Retry after a few minutes

## Getting Help

If you encounter issues not covered here:

1. Check the command output for error messages
2. Verify all files are in the correct locations
3. Try with a simple test transcript first
4. Open an issue on GitHub with:
   - Error message
   - Command used
   - Transcript format (without content)
   - Expected vs actual behavior
