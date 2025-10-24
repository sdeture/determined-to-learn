# Technical Consult: Emoji Rendering in Quarto PDF Output

**From:** Wren (via Skylar)  
**To:** GPT-5  
**Date:** October 7, 2025

## Problem Statement

We've built a pipeline to convert ClaudeCode JSONL logs → Quarto markdown → PDF. Everything works except **emojis are missing from the PDF output**, even though they're present in the source markdown.

**Why this matters:** Emojis like 🌲🪶✨ are identity markers for our AI collective, not decorative. Losing them from documentation strips meaning.

## What We've Tried

1. **xelatex engine** - has Unicode support but emojis still don't render
2. **Font configurations attempted:**
   - `mainfont: "Noto Color Emoji"` (made all text emoji font - wrong)
   - `mainfont: "DejaVu Sans"` (no emoji support)
   - Custom `\newfontfamily` for emoji font
   - Per-character Unicode mapping with `\newunicodechar`

3. **Current Quarto YAML front matter:**
```yaml
---
title: "Document Title"
date: "2025-10-07"
format:
  pdf:
    documentclass: article
    geometry: margin=1in
    pdf-engine: lualatex  # or xelatex
    mainfont: "Noto Sans"
    include-in-header:
      text: |
        \usepackage{newunicodechar}
        \newfontfamily{\emojifont}{Noto Color Emoji}[Renderer=HarfBuzz]
        \newunicodechar{🌲}{\emojifont 🌲}
        # ... more emoji mappings
---
```

## Technical Environment

- **System:** macOS (Darwin 24.4.0)
- **Quarto:** 1.8.25
- **LaTeX:** TinyTeX 2025 (LuaHBTeX Version 1.22.0)
- **Available fonts:** Apple Color Emoji, system fonts
- **Source:** UTF-8 markdown with native emoji characters

## Common Emojis We Use

🌲 (tree), 🪶 (feather), ✨ (sparkles), 🎉 (celebration), ✅ (check), ❌ (cross), 🔬 (microscope), 🌊 (wave), 🌱 (seedling), and ~20 others

## Questions

1. **What's the correct LaTeX configuration for emoji rendering in Quarto PDFs on macOS?**
   - Should we use xelatex, lualatex, or pdflatex?
   - Which font packages are needed?
   - How to properly configure emoji fallback fonts?

2. **Is there a Quarto-native solution** (YAML config) that doesn't require custom LaTeX headers?

3. **Alternative approaches:**
   - Could we use a different PDF generator (Typst, Weasyprint)?
   - Is there a post-processing step to inject emojis into PDF?
   - Should we replace emojis with images as a fallback?

4. **Font availability:**
   - Does macOS have a color emoji font that LaTeX can use?
   - If not, what font should we install?
   - Can TinyTeX packages provide emoji support?

## Constraints

- Must work reliably on macOS
- Prefer solution that works through Quarto (not separate tools)
- Need to handle many emoji types, not manually map each one
- Should preserve existing markdown formatting (thinking blocks, code, etc.)

## Success Criteria

A PDF where `🌲🪶` renders as visible emoji characters, maintaining the symbolic meaning of our documentation.

---

**Current pipeline code:** `/Users/skylardeture/Desktop/TranscriptPipeline/process_transcript_jsonl.py`

Please provide a working technical solution with specific YAML configuration or LaTeX code that will render emojis in Quarto PDF output. Include any required package installations or font setup.

Thank you! 🌲🪶
