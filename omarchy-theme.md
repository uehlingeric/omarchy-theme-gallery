---
description: Generate complete Omarchy desktop themes (color palette + wallpapers) from a text description
argument-hint: <description> [--name slug] [--mode dark|light] [--wallpapers N] [--quality 1K|2K|4K] [--install]
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, mcp__banana__generate_image, mcp__banana__generate_images_batch, mcp__banana__get_settings, mcp__banana__update_settings
---

# /omarchy-theme

Generate a complete Omarchy desktop theme — color palette and wallpapers — from a freeform text description.

**Input:** `$ARGUMENTS`

## Argument Parsing

Parse `$ARGUMENTS` into:

| Flag | Values | Default |
|------|--------|---------|
| (positional) | freeform description text | required |
| `--name` | theme slug | derived from description |
| `--mode` | `dark`, `light` | `dark` |
| `--wallpapers` | number of wallpapers | `3` |
| `--quality` | `1K`, `2K`, `4K` | `2K` |
| `--install` | flag (no value) | do not install |

Example: `/omarchy-theme cyberpunk neon city --wallpapers 4 --quality 4K --install`

## Output Structure

**Working directory** (intermediate artifacts, ABSOLUTE paths):
```
{CWD}/omarchy-themes/{slug}/
├── context/
│   ├── style-lookup.json
│   ├── color-lookup.json
│   └── palette.json
└── backgrounds/
    ├── wallpaper-0-{slug}.png
    ├── wallpaper-1-{slug}.png
    └── wallpaper-2-{slug}.png
```

**Installed theme** (final output):
```
~/.config/omarchy/themes/{slug}/
├── colors.toml
├── backgrounds/
│   ├── wallpaper-0-{slug}.png
│   ├── wallpaper-1-{slug}.png
│   └── wallpaper-2-{slug}.png
├── preview.png
└── light.mode              (only if --mode light)
```

## CRITICAL: Absolute Paths

The banana MCP requires **absolute paths** for `file_name`. Relative paths save to a stray `./output/` directory.

Resolve the working directory first:
```bash
WORK_DIR="$(pwd)/omarchy-themes/{slug}"
```

Use `$WORK_DIR/backgrounds/wallpaper-0-{slug}.png` as `file_name` in all banana calls.

## Output Formatting

### Step Headers

```
● Step {N}: {Step Name}
```

### Step Completion

```
  ✓ {Brief result summary} ({elapsed time})
```

### Timestamps

Record wall-clock time at the start of every step using `date +%s%N`. Compute elapsed at step end:
- Under 60s: `{X.X}s`
- Over 60s: `{M}m {S}s`

### Pipeline Summary

```
──────────────────────────────────────────────
Pipeline Summary
──────────────────────────────────────────────
Step 1: Design Lookup ..................... 1.2s
Step 2: Generate Color Palette ........... 2.4s
Step 3: Generate Wallpapers .............. 45.3s
Step 3b: Verify Wallpapers ............... 8.1s
Step 4: Assemble Theme ................... 0.3s
Step 5: Install Theme .................... 1.8s
──────────────────────────────────────────────
Total ................................... 58.9s
```

## Execution Pipeline

Execute all steps sequentially without pausing for confirmation. Do NOT use `AskUserQuestion` during the pipeline.

---

### Step 1: Design Lookup (slim-pro-max)

**Goal:** Use slim-pro-max to find a matching design style and color palette for the description.

**Fallback:** If `~/.claude/skills/slim-pro-max/` is not installed, skip the searches below and choose the style category, warmth/temperature, and a 17-token seed palette (Primary, Secondary, Accent, Background, Foreground, etc.) yourself from the description, then continue with Step 2 unchanged.

1. **Search for matching style:**
   ```bash
   python3 ~/.claude/skills/slim-pro-max/scripts/search.py "<description keywords>" --domain styles
   ```
   Extract from the top result: style category, warmth/temperature, primary/secondary colors, effects, AI prompt keywords, and design system variables.

2. **Search for matching color palette:**
   ```bash
   python3 ~/.claude/skills/slim-pro-max/scripts/search.py "<description keywords>" --domain colors
   ```
   Extract from the top result: the 17 color tokens (Primary, Secondary, Accent, Background, Foreground, etc.). This is the **seed palette** — web colors that will be adapted to terminal ANSI colors in Step 2.

3. **Derive theme name:**
   - If `--name` is provided, use it as the slug
   - Otherwise, generate a short evocative slug (1-2 words, lowercase, hyphenated) from the description
   - Verify no conflict with existing themes at `~/.local/share/omarchy/themes/` or `~/.config/omarchy/themes/`

4. **Resolve absolute working directory:**
   ```bash
   WORK_DIR="$(pwd)/omarchy-themes/{slug}"
   mkdir -p "$WORK_DIR/context" "$WORK_DIR/backgrounds"
   ```

5. **Write** `style-lookup.json` and `color-lookup.json` to `$WORK_DIR/context/`.

6. **Determine mood properties** from the slim-pro-max results:
   - **warmth**: infer from the style's color temperature (cool/neutral/warm)
   - **saturation**: infer from the style's effects and color intensity (muted/moderate/vivid)
   - **contrast**: infer from the style's accessibility rating and design system variables
   - **dominantHueFamily**: derive from the primary color's hue angle

7. **Print** lookup summary: theme name, matched style, matched product type, warmth, saturation, contrast, mode.

---

### Step 2: Generate Color Palette

**Goal:** Adapt the slim-pro-max web color palette into a 23-color terminal palette for `colors.toml`.

The slim-pro-max seed palette provides 17 web UI tokens. Map them to terminal colors:

| Terminal Key | Derivation |
|---|---|
| `background` | Dark mode: darken slim Background to 5-20% luminance. Light mode: lighten to 90-98%. |
| `foreground` | Dark mode: lighten slim Foreground for contrast. Light mode: darken. WCAG AA (4.5:1) against background. |
| `accent` | Use slim Accent directly (adjust if needed for contrast against background). |
| `cursor` | Lighter/brighter variant of accent. |
| `selection_foreground` | Use background color (inverted selection). |
| `selection_background` | Use accent or a tinted bright variant. |
| `color0` | Slightly lighter than background (dark) or near-black (light). |
| `color1` | **Red** — derive from slim Destructive, ensure recognizably red. |
| `color2` | **Green** — derive from slim Primary/Secondary if green-ish, otherwise generate a green that shares the theme's temperature. |
| `color3` | **Yellow** — warm accent variant, must read as yellow. |
| `color4` | **Blue** — derive from slim Primary/Secondary if blue-ish, otherwise generate a blue that shares the theme's temperature. |
| `color5` | **Magenta** — derive from slim Accent if pink/purple, otherwise generate. |
| `color6` | **Cyan** — generate a cyan that shares the theme's temperature. |
| `color7` | Slightly dimmer than foreground (dark) or near-white (light). |
| `color8`–`color15` | Brighter/more saturated versions of `color0`–`color7`. |

**Rules:**
- color1=red, color2=green, color3=yellow, color4=blue, color5=magenta, color6=cyan — each MUST be recognizable in its hue family.
- Bright variants (color8-15) are lighter/more saturated versions of normal (color0-7).
- All colors share the theme's warmth/temperature from the slim-pro-max style.
- Warm themes: shift blues toward teal, reds toward coral.
- Cool themes: shift reds toward pink, greens toward teal.
- bg/fg contrast: WCAG AA (~4.5:1).

**Self-validate:**
- All 23 keys present and valid 7-character hex (`#rrggbb`)
- Background/foreground have sufficient contrast
- color1-6 are each recognizably their ANSI hue
- If any check fails, adjust and re-validate

**Write** `palette.json` to `$WORK_DIR/context/`.

**Print** the palette:
```
Core:   bg #1a1520  fg #e8d5c4  accent #e09840  cursor #f5c882
Select: fg #1a1520  bg #f5c882
ANSI:   0 #2e2632  1 #d4564e  2 #a3b56a  3 #e0a84e  4 #5a8fbf  5 #c47a98  6 #5eb5a0  7 #d8c8b8
Bright: 8 #5c4f64  9 #e87a70  10 #c4d080  11 #f0c060  12 #78b0d8  13 #d898b0  14 #78d0b8  15 #f2e6d8
```

---

### Step 3: Generate Wallpapers

**Goal:** Generate desktop wallpapers matching the theme's mood and colors using banana MCP.

1. **Build wallpaper prompts.** Each wallpaper gets a different compositional variant for variety. Use the slim-pro-max style's AI Prompt Keywords and the mood properties from Step 1 to construct prompts.

   **Prompt template:**
   ```
   Desktop wallpaper, digital art illustration style (NOT photorealistic): {scene description from style keywords}.
   {compositional variant}. Tones: {background color description} and {accent color description},
   {warmth}, {saturation} saturation. {dark, moody | bright, airy} atmosphere.
   Stylized, painterly, or abstract aesthetic.
   PURE ART ONLY — no text, no labels, no logos, no watermarks, no UI elements, no icons, no buttons,
   no desktop elements, no overlays of any kind.
   ```

   **Compositional variants** (cycle through for each wallpaper):
   - Abstract composition with layered geometric shapes, depth, and subtle parallax effect
   - Atmospheric environment or landscape evoking the mood with natural depth of field
   - Textural macro close-up with organic shapes and rich surface detail
   - Wide panoramic vista with dramatic lighting and sense of scale
   - Minimalist composition with a single focal element and expansive negative space

   The first wallpaper (index 0) is the "hero" — append "Make this striking and dramatic." to its prompt.

2. **Describe colors in natural language** for the prompt. Convert hex to descriptive terms:
   - Check luminance: very dark (<15%), dark (<30%), light (>70%), very light (>85%)
   - Check dominant channel: red, green, blue, yellow, magenta, cyan, neutral gray

3. **Generate wallpapers** via banana MCP batch:
   ```json
   mcp__banana__generate_images_batch({
     "requests": [
       {
         "prompt": "<built prompt>",
         "file_name": "{WORK_DIR}/backgrounds/wallpaper-{index}-{slug}.png",
         "aspect_ratio": "16:9",
         "image_size": "<from --quality flag>",
         "quality": "quality"
       }
     ],
     "max_concurrency": 3
   })
   ```
   **`file_name` MUST be an absolute path.**

4. **Handle failures:** For any wallpaper with `status: "rejected"`, retry ONCE using `mcp__banana__generate_image`. If retry fails, log and continue.

5. **Print** banana-style output:
   ```
   [banana] generate_images_batch ............ 24.1s
     3/3 succeeded
     [0] /absolute/path/wallpaper-0-slug.png
     [1] /absolute/path/wallpaper-1-slug.png
     [2] /absolute/path/wallpaper-2-slug.png
   ```

---

### Step 3b: Verify Wallpapers

**Goal:** Use Claude vision to verify wallpapers are clean and match the theme.

1. **For each generated wallpaper**, use the `Read` tool to view the image, then check:
   - **ARTIFACTS (auto-fail):** Any text, labels, icons, UI elements, buttons, watermarks, desktop elements (recycle bin, folder icons, taskbars), or overlays of ANY kind. These are Gemini hallucinations and MUST be caught.
   - **Color match:** Does the overall color temperature match the palette?
   - **Mood match:** Does the atmosphere match the description?
   - **Composition:** Suitable as desktop wallpaper? (not too busy)

2. **Score** each wallpaper as PASS or FAIL with a brief reason.

3. **For any FAIL**, retry once using `mcp__banana__generate_image` with the same prompt.

4. **Print:**
   ```
   Wallpaper 0: PASS — warm golden tones, clean
   Wallpaper 1: PASS — atmospheric mood, no artifacts
   Wallpaper 2: FAIL → regenerated — had UI icon overlay
   ```

---

### Step 4: Assemble Theme

**Goal:** Package into a valid Omarchy theme directory.

1. **Create theme directory:**
   ```bash
   mkdir -p ~/.config/omarchy/themes/{slug}/backgrounds
   ```

2. **Write `colors.toml`** to `~/.config/omarchy/themes/{slug}/colors.toml`:
   ```toml
   accent = "#hexval"
   cursor = "#hexval"
   foreground = "#hexval"
   background = "#hexval"
   selection_foreground = "#hexval"
   selection_background = "#hexval"

   color0 = "#hexval"
   color1 = "#hexval"
   ...
   color15 = "#hexval"
   ```

3. **Copy wallpapers:**
   ```bash
   cp $WORK_DIR/backgrounds/*.png ~/.config/omarchy/themes/{slug}/backgrounds/
   ```

4. **Copy wallpaper-0 as preview:**
   ```bash
   cp $WORK_DIR/backgrounds/wallpaper-0-{slug}.png ~/.config/omarchy/themes/{slug}/preview.png
   ```

5. **If light mode:**
   ```bash
   touch ~/.config/omarchy/themes/{slug}/light.mode
   ```

6. **Print:**
   ```
   ┌────────────┬───────────────────────────────────┐
   │ Name       │ {slug}                            │
   │ Directory  │ ~/.config/omarchy/themes/{slug}/  │
   │ Wallpapers │ {count}                           │
   │ Mode       │ dark/light                        │
   │ Activate   │ omarchy-theme-set {slug}          │
   └────────────┴───────────────────────────────────┘
   ```

---

### Step 5: Install (Optional)

1. **If `--install` was specified:**
   ```bash
   omarchy-theme-set {slug}
   ```

2. **If not specified:** skip.

3. **Print pipeline summary** (see Output Formatting).

---

## Post-Pipeline Operations

### Single Wallpaper Regeneration

If the user asks to regenerate a wallpaper after the pipeline:

1. Build a new prompt with a different compositional variant
2. Call `mcp__banana__generate_image` with absolute path for `file_name`
3. Copy to theme directory, replacing old one
4. If wallpaper-0 was replaced, also update preview.png
5. If theme is installed, run `omarchy-theme-bg-next` to cycle

### Palette Tweaking

If the user wants to adjust colors after the pipeline:

1. Read current `colors.toml`
2. Apply changes
3. Rewrite `colors.toml`
4. If theme is installed, run `omarchy-theme-refresh`
