---
name: powerpoint-creator
description: "Create professional PowerPoint presentations matching the Talosix brand style. Uses the dark navy theme with teal/cyan/purple/amber accents, consistent footer, and structured slide layouts from the Agentic SDLC deck template."
argument-hint: "[topic-and-audience]"
allowed-tools: Read, Grep, Glob, Bash, Write, Edit
---

# Talosix PowerPoint Creator

Generate professional PowerPoint presentations that exactly match the Talosix brand identity and deck style established in the Agentic SDLC presentation.

## When to Use

- Creating a strategy or vision deck
- Building a client-facing presentation
- Internal team presentations and proposals
- Conference or webinar slide decks
- Board or leadership updates

## Brand Identity

### Color Palette (EXACT hex codes)

```
BACKGROUNDS:
  --slide-bg-primary: #070E1C      /* Near-black navy - primary slide background */
  --slide-bg-secondary: #0A1628    /* Dark navy - card/section backgrounds */
  --slide-bg-tertiary: #111E33     /* Medium navy - content panels, alternating sections */

ACCENT COLORS:
  --accent-teal: #2DD4A8           /* Teal/green - primary accent, success, highlights */
  --accent-cyan: #39BFF0           /* Cyan/blue - secondary accent, links, tech elements */
  --accent-purple: #8B5CF6         /* Purple - tertiary accent, innovation, AI elements */
  --accent-amber: #F59E0B          /* Amber/gold - warnings, stats, call-outs */
  --accent-red: #F43F5E            /* Rose/red - used sparingly for emphasis, old/bad */

TEXT:
  --text-primary: #FFFFFF           /* White - headings, primary text */
  --text-secondary: #CBD5E1         /* Light gray - body text, descriptions */
  --text-muted: #8899B4             /* Muted blue-gray - captions, footnotes, labels */

BORDERS/DIVIDERS:
  --border-subtle: #1A3050          /* Dark blue - card borders, dividers */
```

### Typography

- **Headings**: Bold, large (36-44pt for slide titles, 24-28pt for section headers)
- **Body text**: Regular weight, 16-18pt
- **Stats/numbers**: Extra bold, 48-72pt, colored with accent teal or amber
- **Captions/sources**: 10-12pt, muted blue-gray (#8899B4)
- **Font family**: System sans-serif (Segoe UI, Inter, or similar clean sans-serif)

### Consistent Footer (EVERY SLIDE)

```
TALOSIX  |  [DECK TITLE]  |  CONFIDENTIAL          [slide#] / [total]
```

- Left-aligned, muted text (#8899B4), 10pt
- Appears on every slide except the title slide and closing slide

## Slide Layout Templates

### 1. Title Slide
```
Background: #070E1C with subtle gradient or radial glow
Top: "TALOSIX" wordmark in white
Center: "Embracing the" in light gray (#CBD5E1, 24pt)
         "[Main Title]" in white bold (48-56pt)
Subtitle: Description line in #CBD5E1 (18pt)
Bottom-left: 4 pill badges in a row:
  - Teal (#2DD4A8) badge
  - Cyan (#39BFF0) badge
  - Purple (#8B5CF6) badge
  - Amber (#F59E0B) badge
Bottom-right: "Date  |  Context" in muted (#8899B4)
```

### 2. Stats/Impact Slide (4-up metrics)
```
Background: #0A1628
Title: Bold white, left-aligned (36pt)
Subtitle: #CBD5E1 (16pt)
Four metric cards in a row:
  Each card:
    - Large number: 48-72pt, accent color (alternate teal/amber/cyan/purple)
    - Description: White, 14pt
    - Source: #8899B4, 10pt
Bottom callout bar: Full-width darker strip with key stat
```

### 3. Three-Column Content Slide
```
Background: #070E1C
Title: White bold (36pt)
Subtitle: #8899B4 (14pt)
Three equal columns, each with:
  - Column header with accent-colored left border or icon
  - Bullet points in #CBD5E1 (14pt)
  - Source citation in #8899B4
```

### 4. Comparison Slide (Old vs New / Before vs After)
```
Background: #0A1628
Title: White bold (36pt)
Two-column layout:
  LEFT: "THE OLD WAY" header with red accent (#F43F5E)
    - Items in #CBD5E1
  RIGHT: "THE [NEW] WAY" header with teal accent (#2DD4A8)
    - Items in #CBD5E1
  Arrow (→) or connector between each pair
Source line at bottom in #8899B4
```

### 5. Process/Timeline Slide (Numbered Steps)
```
Background: #070E1C
Title: White bold (36pt)
Horizontal flow of numbered steps:
  Each step:
    - Number in accent-colored circle (01, 02, 03...)
    - Step title in white bold (18pt)
    - Description in #CBD5E1 (12pt)
    - Arrow connector (▶) between steps
  Alternate accent colors: teal, cyan, purple, amber
Key principle callout box at bottom with teal left border
```

### 6. Two-Panel Detail Slide (What AI Does / What Humans Own)
```
Background: #0A1628
Title: White bold (36pt)
Two panels side by side:
  LEFT panel: Dark card (#111E33) with accent-colored header
    - Bullet points in #CBD5E1
  RIGHT panel: Dark card (#111E33) with different accent-colored header
    - Bullet points in #CBD5E1
```

### 7. Four-Card Grid Slide
```
Background: #070E1C
Title: White bold (36pt)
2x2 grid of cards:
  Each card:
    - #111E33 background with subtle border
    - Accent-colored title (18pt bold)
    - Description text in #CBD5E1 (14pt)
    - Each card uses a different accent color for its title
```

### 8. Roadmap/Phase Slide (3 phases)
```
Background: #0A1628
Title: White bold (36pt)
Three equal columns for phases:
  Each phase:
    - Phase header with accent-colored top border
    - Phase label: "WEEKS X-Y" and "PHASE NAME" in bold
    - Bullet list of items in #CBD5E1
    - Each phase uses different accent color (teal, cyan, purple)
```

### 9. Closing Slide
```
Background: #070E1C with gradient
Top: "TALOSIX" wordmark
Center: "Let's [Action]" in large bold white
         "[Together / Forward / etc.]"
Center quote block: Italicized quote in #CBD5E1 with subtle border
Bottom: Three value proposition cards in a row with accent-colored icons
```

### 10. Large Quote/Callout Slide
```
Background: #0A1628
Large quote in white, 28-36pt, with left teal border bar
Attribution below in #8899B4
```

## Slide Design Rules

1. **Dark backgrounds ALWAYS** - never use white or light backgrounds
2. **One idea per slide** - don't overcrowd
3. **Stats are heroes** - large numbers (48-72pt) in accent colors grab attention
4. **Consistent spacing** - 48px padding from slide edges, 24px between elements
5. **Source everything** - citations in #8899B4 at bottom of relevant slides
6. **No stock photos** - use icons, abstract shapes, gradients, or data visualizations
7. **Accent color rotation** - cycle through teal, cyan, purple, amber to keep visual interest
8. **Footer on every content slide** - format: `TALOSIX  |  [TITLE]  |  CONFIDENTIAL    [n] / [total]`
9. **Maximum 6-7 bullets per panel** - if more, split across slides
10. **Bold key phrases** in bullet text for scannability

## Generation Workflow

### Step 1: Understand the Deck

Ask or determine:
- Topic and key message
- Target audience (internal team, leadership, customer, conference)
- Number of slides desired
- Key data points or stats to feature
- Any specific content to include

### Step 2: Create Slide Outline

Generate a slide-by-slide outline:
```
Slide 1: [Title Slide] - Main title + subtitle
Slide 2: [Stats Slide] - Key metrics establishing urgency/context
Slide 3: [Content] - Problem statement or background
...
Slide N: [Closing Slide] - Call to action
```

Map each slide to one of the 10 layout templates above.

### Step 3: Generate python-pptx Script

Generate a Python script using the `python-pptx` library that creates the deck programmatically:

```python
from pptx import Presentation
from pptx.util import Inches, Pt, Emu
from pptx.dml.color import RGBColor
from pptx.enum.text import PP_ALIGN, MSO_ANCHOR
```

Key implementation patterns:
- Set slide dimensions to widescreen (13.333" x 7.5")
- Use `RGBColor` with the exact hex values from the palette
- Create reusable helper functions for common elements (footer, cards, badges)
- Set font to 'Segoe UI' or 'Inter' with fallback
- Use `slide.shapes.add_shape()` for colored rectangles and cards
- Use `slide.shapes.add_textbox()` for positioned text

### Step 4: Execute and Deliver

Run the script to generate the .pptx file. Offer the user:
- The generated .pptx file path
- The Python script for future modifications
- Suggestions for manual polish in PowerPoint (add transitions, fine-tune spacing)

## Output

When generating a deck:
1. First show the slide outline for approval
2. Generate the complete python-pptx script
3. Run the script to create the .pptx file
4. Report the file location

## Dependencies

Requires `python-pptx` library. Install with:
```bash
pip install python-pptx
```
