# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **single-file HTML tool** (`flipcard_generator.html`) that generates standalone flipcard interactives for ASU courses. Users open the generator in a browser, configure their flipcards, and download a self-contained HTML file they can embed in an LMS.

There is no build system, no dependencies, no package manager, and no test suite. The entire application lives in one HTML file with inline CSS and JavaScript.

## How It Works

The generator has a two-stage architecture:

1. **Generator UI** (`flipcard_generator.html`) — A form where users upload a hero image, pick a theme (orange/blue), choose card count (2–8), and enter front/back text for each card.
2. **Generated output** — When the user clicks "Generate HTML", the script constructs a complete HTML document as a template literal string (starting ~line 347), injects the card data and base64-encoded image, and offers it as a blob download.

The generated flipcard HTML is entirely self-contained (no external assets) so it works when embedded in Canvas or other LMS platforms.

## Key Architecture Details

- **Text auto-fit system**: Both the generator (preview warnings) and generated output include font-size shrinking logic. The generator uses `fitTextToBox()` (~line 105) to warn users if text will overflow. The generated output uses `fitElementText()` for runtime fitting.
- **Card layout**: `buildRowsForCount()` maps card counts to row configurations (e.g., 5 cards → rows of [2, 3]; 8 cards → [4, 4]). This function is duplicated in both the generator and the generated output.
- **Theme colors**: Defined in `themeColors` object (~line 343). Currently two themes: orange and blue.
- **HTML escaping**: `escapeHtml()` is defined twice — once in the generator and once in the generated template — to prevent XSS in card text.
- **Viewport handling**: The generated output watches for resize/zoom changes via `watchViewportChanges()` and re-runs layout + text fitting.

## Development

To work on this project, simply open `flipcard_generator.html` in a browser. No server needed — everything runs client-side.

## Planned Iterations

See `iterations.md` for ideas: additional themes (maroon/gold), user-customizable colors, editable landing page text, optional landing page, default graphics, character limits, and uniform card sizing.
