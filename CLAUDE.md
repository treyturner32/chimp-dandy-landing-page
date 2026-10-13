# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, single-page landing site for Chimp Dandy Labs at chimpdandy.com, served by GitHub Pages from the `main` branch root. There is no build step, package manager, linter or test suite. `index.html` holds all markup and CSS inline. The only script is the GoatCounter stats snippet at the bottom of the page. `README.md` covers the one-time GitHub Pages, Porkbun DNS, email-forwarding and GoatCounter setup.

`venv/` holds only pip, nothing the site uses. It has its own `.gitignore` (`*`), so git ignores it.

## Previewing and deploying

Preview locally with a server run from the repo root. Every asset path is root-absolute (`/img/...`, `/fonts/...`), so opening the file directly with `file://` breaks them:

```sh
python3 -m http.server 8000   # then open http://localhost:8000
```

Check all three layouts (landscape, near-square and portrait) by resizing the window or using device emulation.

Deploying means pushing to `main`; GitHub Pages republishes within a minute or two. The folder is not a git repo yet; README step 1 creates it. `git push` to `main` needs the user's approval.

## Layout architecture (the part that needs several things read together)

The page is one full-viewport `.hero` with the logo art as a `cover` background, plus a `.content` text block placed in the empty stripes clear of the chimp. Three aspect-ratio breakpoints each switch both the background image and the geometry of `.content`:

| Media query | Image (px) | Anchor | Text placement |
|---|---|---|---|
| default (landscape) | `bg-wide` 3240×1080 | right bottom | left column, `width: calc(100vw - 80vh - 2*gutter)` |
| `max-aspect-ratio: 4/3` | `bg-square` 1800×1800 | center bottom | centred above the hat, `height: min(47.8vh, 100vh - 52.2vw) - ...` |
| `max-aspect-ratio: 3/4` | `bg-tall` 1200×2600 | center bottom | centred above the hat, `height: min(63.8vh, 100vh - 78.3vw) - ...` |

**The percentages and vw/vh constants are measured from the artwork**: where the chimp's hat top or left edge sits in each image, scaled by `cover`. For example, in `bg-tall` the hat top is 940px above the bottom, so 940/2600 gives 36.2% (hence 63.8vh) and 940/1200 gives 78.3vw. If an image is replaced or recropped, recompute its constants, or the text will overlap the chimp. The CSS comments note each source measurement.

Other coupled details:
- Each background is declared twice, as a `.jpg` `url()` fallback and then as `image-set()` with `.webp` first. Keep both formats of every `bg-*` image in step.
- `.content` is a size container (`container-type: size`). Heading, tagline and button sizes use `cqi`/`cqh` units, so text scales to the box the breakpoint gives it rather than to the viewport.
- Colours are the three `:root` tokens (`--navy`, `--teal`, `--cream`). `theme-color` in `<head>` repeats the navy hex.
- Fonts are self-hosted woff2 files (Anton, DM Serif Display italic, DM Mono 500; SIL OFL). Anton is preloaded because it renders the heading.
- The `og:*` tags and `canonical` use absolute `https://chimpdandy.com/` URLs. `og-image.jpg` is 1200×630, matching the declared `og:image:width`/`height`.

## Editing copy

Visible text lives in `<div class="content">` (heading, tagline, contact mailto) and in `<footer>`. The tagline is repeated in `<meta name="description">` and `og:description`, so update those too when it changes.
