# Webxperts Website

Marketing site for Webxperts — a Johannesburg web design & development studio.

Single self-contained static file: **`index.html`** (HTML + CSS + JS inline, no build step).
Brand: gold (`#FDD000`) on near-black. Motion via GSAP + Lenis (loaded from CDN).

## Preview locally

```bash
# open directly
start index.html        # Windows
open index.html         # macOS

# or serve (recommended)
python3 -m http.server 8000   # http://localhost:8000
```

## Working on it with Claude Code

This repo has a `CLAUDE.md` with full context — the design system, architecture, the
contrast rules, and the outstanding TODO list. Claude Code reads it automatically. From
this folder:

```bash
claude
```

Then describe what you want, e.g. *"swap the placeholder portfolio for these real
projects"* or *"wire the contact form to Formspree"*.

## Deploy

It's a static file — drop it on any static host (Cloudflare Pages, Netlify, Vercel,
GitHub Pages, or your existing hosting). No server needed.

## Before going live

See the TODO list in `CLAUDE.md` — real portfolio, real testimonial names, real stats,
and wiring the contact form to a backend.
