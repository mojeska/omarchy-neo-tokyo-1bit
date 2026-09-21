# Neo Tokyo 1bit

A strict 1-bit (black and white) [Omarchy](https://omarchy.org/) theme with
a 1980s Tokyo vibe -- dithered nighttime cityscapes instead of photos, and
a UI palette that never uses more than two colors.

![preview](preview.png)

## Install

```bash
omarchy theme install https://github.com/mojeska/omarchy-neo-tokyo-1bit.git
omarchy theme set "Neo Tokyo 1bit"
```

## What's in it

- **`colors.toml`** -- every UI role (background, foreground, accent,
  selection) is exactly `#000000` or `#ffffff`. No third color, no
  in-between grays.
- **`backgrounds/`** -- three original dithered scenes, procedurally
  generated (no photos), cycle with `omarchy theme bg next`:
  1. `1-shinjuku-sunrise.png` -- a striped retro sun rising behind a city
     skyline and a Tokyo-Tower-style spire, halftone-dithered.
  2. `2-neon-alley.png` -- a signage alley lit with real kanji (夜・光・酒・
     夢・恋・雨・猫・電), reflected in a hazy wet-pavement smear below.
  3. `3-fuji-torii.png` -- Mount Fuji and a torii gate against the same
     retro sun, over a synthwave perspective grid.

  All three are genuinely 2-color images -- dithering (not gradients or
  extra shades) is what implies depth and light.

## The one restrained exception

The UI chrome (bar, panels, background/foreground/accent/selection) stays
strictly black and white. The terminal's ANSI palette bends the rule in
exactly one place: `red`/`bright_red` and `green`/`bright_green` are real
colors (`#ff2d2d`/`#ff5555` and `#00e676`/`#33ff8c`), because they're the
two roles with the most everyday payoff -- `git diff` additions/removals,
test pass/fail, error output. Every other terminal color (yellow, orange,
cyan, blue, magenta, brown) stays white. It reads like a single deliberate
color pop against an otherwise monochrome frame, not a full palette.

## How the shipped backgrounds were made

The three background images were built with ImageMagick shape/text drawing
plus a final dither pass -- no photos, no AI image generation, no external
assets. There's no build script checked in; the drawing part is
scene-specific, but the final dither pass (below) is generic and is the
same step used to convert a real photo.

## Adding your own backgrounds

Any photo can be converted to match this theme with a single ImageMagick
command -- it's the same dither pass used to finish the three shipped
scenes, just pointed at a real image instead of drawn shapes:

```bash
magick your-photo.jpg -colorspace Gray -dither FloydSteinberg -colors 2 -type bilevel your-photo-1bit.png
```

That gives a soft, photographic dither -- good for photos with real
gradients (sky, glow, smooth light falloff). For the more mechanical,
halftone-grid look instead (what `2-neon-alley.png` uses):

```bash
magick your-photo.jpg -colorspace Gray -ordered-dither o8x8 your-photo-1bit.png
```

Either way, confirm the result is genuinely 2-color before using it:

```bash
magick your-photo-1bit.png -format '%k colors' info:   # should print "2 colors"
```

A high-contrast source photo (bright lights against dark sky, strong
silhouettes) dithers far better than a flat, evenly-lit one -- the dither
pattern is doing the work a gradient would otherwise do, so it needs real
tonal range to work with.

## Where converted backgrounds go

Omarchy looks in two places for this theme's backgrounds, both keyed by
its slug, `neo-tokyo-1bit`:

- `~/.config/omarchy/themes/neo-tokyo-1bit/backgrounds/` -- the theme's own
  shipped backgrounds (this repo's `backgrounds/`, once installed).
- `~/.config/omarchy/backgrounds/neo-tokyo-1bit/` -- **the right place for
  your own additions.** It exists for exactly this: extra backgrounds for
  any theme (stock or custom) without editing the theme itself, so
  re-running `omarchy theme install` on this repo later won't wipe out
  your own photos.

```bash
mkdir -p ~/.config/omarchy/backgrounds/neo-tokyo-1bit
cp your-photo-1bit.png ~/.config/omarchy/backgrounds/neo-tokyo-1bit/
omarchy theme bg next   # cycle to it
```
