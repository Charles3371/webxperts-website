# Portfolio Images — Capture & Drop-in Guide

Everything needed to replace the placeholder imagery with real project work.
All dimensions below were measured from the actual rendered slots (at 2× for retina).

## What you need to produce

| # | Asset | Count | Export size | Aspect |
|---|-------|-------|-------------|--------|
| 1 | Project images (one per case study) | 9 | **1600 × 1200 px** | 4:3 |
| 2 | Studio / team photos | 2 | **1250 × 1560 px** | 4:5 portrait |

**One image per project is enough.** The same file feeds all three places it appears:

- Home "Selected Work" card — renders ~460 × 345 (4:3)
- Portfolio grid card — renders ~400 × 300 (4:3)
- Case-study modal — renders ~780 × 440 (**16:9 center-crop** of your 4:3 image)

⚠ Because the modal center-crops to 16:9, keep the important content (the hero of the
screenshot, the logo, the device) inside the **middle ~75% of the image height**. Don't
put anything critical along the very top or bottom edge.

## The 9 project slots

| File name | Project | Category |
|---|---|---|
| `stonebridge.jpg` | Stonebridge Logistics | Web Development |
| `aurelia.jpg` | Aurelia Skincare | Web Design |
| `horizon.jpg` | Horizon Realty | Web Development |
| `kalahari.jpg` | Kalahari & Co | Branding |
| `pioneer.jpg` | Pioneer Fintech | Web Design |
| `savanna.jpg` | Savanna Eats | Branding |
| `bluerock.jpg` | Bluerock Capital | Web Development |
| `northwind.jpg` | Northwind Trading | Web Design |
| `pulse.jpg` | Pulse Fitness Studio | Branding |

(These are the placeholder names — rename projects freely; if a real client replaces one,
update the card's `data-title`, `data-cat`, description and alt text at the same time.)

Plus: `studio-1.jpg`, `studio-2.jpg` — the two 4:5 team/studio photos (home about-preview
and About page).

## How to capture screenshots that look Awwwards-grade

1. **Capture at 1600 × 1200.** Set your browser viewport to 1600 × 1200 (DevTools →
   device toolbar → responsive), hide scrollbars, screenshot the most designed area of
   the page — usually the hero. Full-page scrolls squashed into 4:3 read as clutter;
   one confident viewport reads as craft.
2. **Websites: show the top of the page.** Browser chrome is optional — flat, clean
   crops are more timeless than device mockups. If you do want frames, use them on
   *every* project or none (consistency beats variety).
3. **Branding projects (Kalahari, Savanna, Pulse): photograph or mock the artifact** —
   packaging, business card, signage — rather than screenshotting a PDF. Fill the frame.
4. **Don't pre-filter.** The site applies a unifying treatment to every card
   (`saturate(.85) contrast(1.05)`), so export images at normal saturation and let the
   CSS make the set feel cohesive.
5. **Dark-ish or mid-tone images sit best** on both the cream grid and the near-black
   gallery. Avoid pure-white full-bleed screenshots — add the page's own background
   padding if a site is very white.
6. **Format:** JPG quality 80–85 (or WebP q80). Target **≤ 350 KB per image** — there
   are 16 image tags on the site and it should stay fast.

## Drop-in

1. Put the files in `assets/portfolio/` in this repo.
2. Each project card has **two** attributes pointing at the image, in **two** places:
   - Portfolio page (`.pf` article, all 9): the `data-img="…"` attribute (feeds the
     modal) **and** the inner `<img src="…">`.
   - Home gallery (`.work-card` article, 5 of the 9 — Stonebridge, Aurelia, Horizon,
     Kalahari, Pulse): same two attributes again.
3. The two studio photos are the `<img>` inside each `.split-media` block (home + about).
   Keep the `data-parallax` attribute on them — it drives the scroll parallax.
4. Update every image's `alt` text to describe the real project.

### Or just hand it to Claude Code

Drop the images into `assets/portfolio/`, then paste:

```
The real portfolio images are in assets/portfolio/ — see PORTFOLIO-IMAGES.md for the
file-to-project mapping. In index.html, replace every Unsplash URL with the matching
local path: for each project update BOTH the data-img attribute and the inner <img src>
on the portfolio .pf card, and (for Stonebridge, Aurelia, Horizon, Kalahari and Pulse)
the same two attributes on the home .work-card. Swap the two .split-media images for
studio-1.jpg and studio-2.jpg, keeping their data-parallax attributes. Rewrite every alt
attribute to describe the real project. Don't change anything else.
```
