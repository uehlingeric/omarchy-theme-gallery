# Omarchy Theme Gallery

25 desktop themes generated entirely by AI using a custom [Claude Code](https://docs.anthropic.com/en/docs/claude-code) slash command. Each theme includes a complete terminal color palette and AI-generated wallpapers — built from nothing but a text description.

![Claude Code Slash Command](https://img.shields.io/badge/Claude_Code_Slash_Command-1E3A8A) ![License: MIT](https://img.shields.io/badge/License-MIT-green)

## How It Works

```
/omarchy-theme cyberpunk neon city --wallpapers 3 --quality 2K --install
```

The `/omarchy-theme` command is a Claude Code [custom slash command](https://docs.anthropic.com/en/docs/claude-code/tutorials/custom-slash-commands) that orchestrates a 5-step pipeline:

1. **Design Lookup** — Queries a design intelligence database ([slim-pro-max](https://github.com/uehlingeric/slim-pro-max)) to find a matching visual style and seed color palette
2. **Color Palette** — Adapts web UI colors into a 23-color terminal palette (ANSI 0–15 + core/selection), enforcing WCAG AA contrast and hue-family correctness
3. **Wallpaper Generation** — Batch-generates desktop wallpapers via [Gemini image generation](https://ai.google.dev/gemini-api/docs/image-generation) with compositional variety (abstract, landscape, macro, panoramic, minimalist)
4. **Quality Verification** — Uses Claude's vision to reject wallpapers containing text, UI artifacts, or watermarks
5. **Theme Assembly** — Packages `colors.toml` + wallpapers into a ready-to-use [Omarchy](https://github.com/getomarchy/omarchy) theme

Every color and every pixel is AI-generated. No manual tweaking.

## Gallery

<!-- Gallery is organized alphabetically. Each theme shows the hero wallpaper preview and accent color. -->

### Amber Ember
> Warm amber tones with glowing ember highlights
<img src="gallery/amber-ember.png" alt="Amber Ember theme preview" width="100%">

`accent: #d97706` `bg: #1a1008` `fg: #f0e0c8`

---

### Arctic Frost
> Cool cyan and ice blue with deep navy depths
<img src="gallery/arctic-frost.png" alt="Arctic Frost theme preview" width="100%">

`accent: #0891b2` `bg: #0a1620` `fg: #e0f0f8`

---

### Brutalist Red
> Pure red on black — raw, uncompromising contrast
<img src="gallery/brutalist-red.png" alt="Brutalist Red theme preview" width="100%">

`accent: #ff0000` `bg: #0a0a0a` `fg: #f0f0f0`

---

### Burgundy Velvet
> Deep wine reds with warm dusty rose undertones
<img src="gallery/burgundy-velvet.png" alt="Burgundy Velvet theme preview" width="100%">

`accent: #b91c1c` `bg: #150808` `fg: #f0d8d0`

---

### Cinematic
> Moody indigo with a subtle silver-screen coolness
<img src="gallery/cinematic.png" alt="Cinematic theme preview" width="100%">

`accent: #5e6ad2` `bg: #020203` `fg: #ededef`

---

### Coffee House
> Rich espresso browns with warm roasted amber
<img src="gallery/coffee-house.png" alt="Coffee House theme preview" width="100%">

`accent: #92400e` `bg: #1a1008` `fg: #f0e0c8`

---

### Deep Space
> Dark indigo void with electric violet highlights
<img src="gallery/deep-space.png" alt="Deep Space theme preview" width="100%">

`accent: #4338ca` `bg: #0f0f23` `fg: #f8fafc`

---

### Dev Dark
> Clean green-on-dark — built for coding
<img src="gallery/dev-dark.png" alt="Dev Dark theme preview" width="100%">

`accent: #22c55e` `bg: #0f172a` `fg: #f8fafc`

---

### Dreamscape
> Ethereal purple haze with soft violet luminance
<img src="gallery/dreamscape.png" alt="Dreamscape theme preview" width="100%">

`accent: #7c3aed` `bg: #0a0a1a` `fg: #e8e0f8`

---

### Firepit
> Blazing orange with deep charred undertones
<img src="gallery/firepit.png" alt="Firepit theme preview" width="100%">

`accent: #f97316` `bg: #151010` `fg: #f8e8e0`

---

### Forest Floor
> Deep woodland greens with earthy moss tones
<img src="gallery/forest-floor.png" alt="Forest Floor theme preview" width="100%">

`accent: #15803d` `bg: #0a1a0a` `fg: #d4e0c8`

---

### Glass Aurora
> Translucent emerald with aurora borealis shimmer
<img src="gallery/glass-aurora.png" alt="Glass Aurora theme preview" width="100%">

`accent: #10b981` `bg: #050a15` `fg: #e0f0f8`

---

### Monochrome
> Pure grayscale with a single blue accent
<img src="gallery/monochrome.png" alt="Monochrome theme preview" width="100%">

`accent: #2563eb` `bg: #09090b` `fg: #e4e4e7`

---

### Neon Pulse
> Hot pink and electric blue on deep midnight
<img src="gallery/neon-pulse.png" alt="Neon Pulse theme preview" width="100%">

`accent: #f43f5e` `bg: #0f0f1a` `fg: #e2e8f0`

---

### Noir Gold
> Dark film noir with warm gold accents
<img src="gallery/noir-gold.png" alt="Noir Gold theme preview" width="100%">

`accent: #a16207` `bg: #0c0a09` `fg: #e7e5e4`

---

### Rose Dawn
> Soft pink dawn with warm magenta highlights
<img src="gallery/rose-dawn.png" alt="Rose Dawn theme preview" width="100%">

`accent: #ec4899` `bg: #1a1015` `fg: #f8e8f0`

---

### Sakura
> Delicate cherry blossom pinks on twilight purple
<img src="gallery/sakura.png" alt="Sakura theme preview" width="100%">

`accent: #e88aaa` `bg: #140a10` `fg: #f8e0ee`

---

### Slate Forge
> Industrial teal-green on cool gunmetal gray
<img src="gallery/slate-forge.png" alt="Slate Forge theme preview" width="100%">

`accent: #059669` `bg: #0f1215` `fg: #d4d8dc`

---

### Solar Flare
> Brilliant yellow with scorching golden warmth
<img src="gallery/solar-flare.png" alt="Solar Flare theme preview" width="100%">

`accent: #eab308` `bg: #0a0a05` `fg: #f8f0d0`

---

### Synthwave
> Retro neon pink and purple on dark midnight
<img src="gallery/synthwave.png" alt="Synthwave theme preview" width="100%">

`accent: #ff71ce` `bg: #1a1a2e` `fg: #f0e6ff`

---

### Terminal Green
> Classic green phosphor terminal aesthetic
<img src="gallery/terminal-green.png" alt="Terminal Green theme preview" width="100%">

`accent: #22c55e` `bg: #020617` `fg: #f0fdf4`

---

### Terracotta
> Warm baked clay with rustic earthy warmth
<img src="gallery/terracotta.png" alt="Terracotta theme preview" width="100%">

`accent: #c67b5c` `bg: #1a1210` `fg: #f0e0d0`

---

### Vintage Film
> Faded sepia tones with warm nostalgic grain
<img src="gallery/vintage-film.png" alt="Vintage Film theme preview" width="100%">

`accent: #d4a574` `bg: #1a1510` `fg: #f5e6c8`

---

### Void
> Pure black and white — absolute zero contrast
<img src="gallery/void.png" alt="Void theme preview" width="100%">

`accent: #f8fafc` `bg: #000000` `fg: #fafafa`

---

### Zen Garden
> Calm emerald green with meditative violet undertones
<img src="gallery/zen-garden.png" alt="Zen Garden theme preview" width="100%">

`accent: #059669` `bg: #0e0a18` `fg: #e8e0f0`

---

## Theme Structure

Each theme produces:

```
~/.config/omarchy/themes/{name}/
├── colors.toml          # 23-color terminal palette
├── backgrounds/
│   ├── wallpaper-0.png  # Hero wallpaper
│   ├── wallpaper-1.png
│   └── wallpaper-2.png
└── preview.png          # Gallery preview
```

### Color Palette Format

```toml
# Core
accent = "#ff71ce"
cursor = "#ff99dd"
foreground = "#f0e6ff"
background = "#1a1a2e"
selection_foreground = "#1a1a2e"
selection_background = "#b967ff"

# ANSI 0-7 (normal)
color0 = "#2a2a4a"    # black
color1 = "#ff006e"    # red
color2 = "#05ffa1"    # green
color3 = "#ffbe0b"    # yellow
color4 = "#5d34d0"    # blue
color5 = "#b967ff"    # magenta
color6 = "#01cdfe"    # cyan
color7 = "#c8b8e0"    # white

# ANSI 8-15 (bright)
color8 = "#3d3d66"    # bright black
color9 = "#ff4d94"    # bright red
color10 = "#3dffc0"   # bright green
color11 = "#ffd43b"   # bright yellow
color12 = "#7c5ce7"   # bright blue
color13 = "#d098ff"   # bright magenta
color14 = "#33ddff"   # bright cyan
color15 = "#f5f0ff"   # bright white
```

## The Command

The full slash command definition is in [`omarchy-theme.md`](omarchy-theme.md). Drop it into `~/.claude/commands/` to use it with Claude Code.

### Dependencies

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) with MCP support
- Image generation MCP server (Gemini via [banana-mcp](https://github.com/uehlingeric/banana-mcp))
- [slim-pro-max](https://github.com/uehlingeric/slim-pro-max) design intelligence skill
- [Omarchy](https://github.com/getomarchy/omarchy) desktop environment (for theme installation)

### Usage

```bash
# Basic — generates 3 wallpapers at 2K, dark mode
/omarchy-theme cozy autumn cabin

# Full options
/omarchy-theme cyberpunk neon city --name neon-pulse --wallpapers 4 --quality 4K --install

# Light mode
/omarchy-theme minimal scandinavian --mode light --install
```

## License

MIT — see [LICENSE](LICENSE).
