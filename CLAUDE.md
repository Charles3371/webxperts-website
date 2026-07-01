# Webxperts — Marketing Website

Context for working on this project. Read this before making changes.

## What this is

A redesign of the Webxperts marketing site (https://www.webxperts.co.za). Webxperts
is a Johannesburg-based web design & development studio (web design, web development,
brand identity). The whole site currently lives in a **single self-contained file:
`index.html`** — all HTML, CSS, and JS inline. No build step.

Target aesthetic: high-end / "Awwwards-grade" studio site — confident, kinetic,
motion-led, but fast and accessible.

## Run / preview

It's a static file. Any of these work:

```bash
# simplest
open index.html            # macOS
start index.html           # Windows

# or serve locally (better — avoids any file:// quirks)
python3 -m http.server 8000      # then visit http://localhost:8000
npx serve .
```

There is no package.json, bundler, or framework. Edits to `index.html` are live on refresh.

## Architecture

- **Single-page app** with hash routing (`#home`, `#about`, `#services`, `#portfolio`,
  `#contact`). Each "page" is a `<section class="page" data-page="...">`. JS toggles the
  active page, swaps `<title>`/meta description per route, and runs that page's animations.
- **Per-page meta** is in a `metaByRoute` object in the script. Keep it in sync if you
  add/rename routes (it drives SEO titles + descriptions).
- **JSON-LD** `ProfessionalService` schema is in `<head>` — update contact details there too.
- **Routing detail:** on navigation each page rebuilds its GSAP context (reverts old
  triggers, rebuilds, then `ScrollTrigger.refresh()`), which avoids stale-pin bugs. If you
  add a pinned/scroll-triggered section, wire it into that per-page build, not global init.

## Dependencies (all via CDN — no install)

- **GSAP 3.12.5** + **ScrollTrigger** — cdnjs
- **Lenis 1.1.14** (smooth scroll) — jsdelivr, integrated into GSAP's ticker
- **Google Fonts:** Bricolage Grotesque (display), Inter (body), Space Mono (labels)

If the site ever needs to work offline or pass strict CSP, these are the things to vendor
locally.

## Design system (CSS custom properties in `:root`)

Brand is **gold on near-black**. The accent is `#FDD000` (from the logo/favicon).

```
--ink:#0B0A0D  --ink-2:#141318  --ink-3:#1E1C24        /* near-black surfaces */
--bone:#ECE6D9 --bone-2:#E2DBCB --bone-3:#D6CDB8        /* warm cream surfaces */
--brand:#FDD000                                         /* always-bright gold: buttons, logo, favicon */
--uv:#FDD000   --uv-soft:#FFDE45  --uv-deep:#C9A300      /* CONTEXTUAL accent */
--on-ink:#ECE6D9  --on-ink-dim:#9A9189                  /* text on dark */
--on-bone:#1A1518 --on-bone-dim:#6B6258                 /* text on light */
--display:'Bricolage Grotesque'  --body:'Inter'  --mono:'Space Mono'
```

**IMPORTANT contrast rule — don't break this:** pure `#FDD000` is unreadable on the cream
(`--bone`) sections. So the accent is *contextual*: `.section--ink` keeps it bright gold,
and `.section--bone` (and `.modal-box`) **override `--uv`/`--uv-soft`/`--uv-deep` to a deep
amber (`#7A5F00`)** so accent text stays legible on cream. Buttons (`.btn-uv`) always use
`--brand` (bright gold) with near-black text, on every surface.

- If you add an accent-colored element, use `var(--uv)` / `var(--uv-soft)` so it adapts
  to its surface automatically. Use `var(--brand)` only when you want bright gold regardless
  (buttons, the logo mark, the favicon).
- Sections alternate `.section--ink` (dark) and `.section--bone` (cream) for rhythm.

## Signature interactions (preserve these when refactoring)

- Preloader (word reveal + 0→100 counter), then slide up.
- Hero: kinetic headline where the last word cycles (`#cycle` / `.cycle-track`). Hero type
  is capped by viewport height (`min(11.5vw, 16vh)` + a `max-height:780px` media query) so
  the CTA always stays above the fold — don't remove that cap.
- Gold page-transition wipe between routes (`#wipe`).
- **Masked word reveals** on all `.display-m` / `.display-l` headings with `.anim`: JS
  `splitWords()` wraps words in `.wm` (mask) / `.ww` (word); GSAP staggers them up on
  scroll. Splitting only happens when motion is allowed; guard `el._split` prevents
  re-splitting across route changes.
- **Curtain reveals** on `.split-media` (clip-path inset) + image scale-settles on
  portfolio/work cards (GSAP uses `clearProps:'transform'` on complete so the CSS hover
  zoom takes over — keep that, or hover breaks).
- **Button label roll**: `.btn` text is wrapped at boot into `.btn-label > .btn-t/.btn-t2`
  (duplicate slides in on hover). To change a button's text from JS use
  `setBtnLabel(btn, 'Text')` — do NOT touch childNodes directly (the contact form uses this).
- Home: pinned **horizontal** "Selected Work" gallery (`.work-track`, disabled below
  900px) with a progress bar and **scroll-velocity skew** on the track.
- **Velocity-reactive marquee**: `#trustRow`'s CSS animation is replaced at runtime by a
  GSAP loop whose speed/skew follow scroll velocity; hovering slows it. Cleanup lives in
  `buildPage` via the `marqueeTick` ticker handle.
- Footer: giant wordmark rises character-by-character (`.fc` spans) once on first view,
  and a **live Johannesburg clock** (`#jhbClock`, Intl API) ticks in the footer bottom.
- Magnetic buttons (`.magnetic`, desktop + fine-pointer only), animated stat counters
  (`.count[data-to]`), parallax images (`[data-parallax]`).
- `prefers-reduced-motion` is fully respected (no Lenis, no cycling, no splitting, static
  reveals; button roll disabled via media query). Keep any new motion behind that check.

## Brand assets

- Logo mark = an angular "X" made of 3 `<polygon>`s, inlined in the header + footer
  `.brand` SVG (filled `var(--brand)`). The favicon is the same mark as an SVG data-URI
  (gold X on near-black rounded square).
- Source files the client provided: `webxperts_x_logo.svg` (white-filled X, viewBox
  0 0 640 640) and `Favicon.png`. Recolor the SVG via `fill`.

## Real content already integrated

- Tagline, services (Web Design / Web Development / Brand Identity), the 5-step process
  (Free consultation → Discuss needs & goals → Proposal → Approve → Start & launch),
  payment terms (upfront / monthly instalments / pay-as-you-go), and contact details
  (hello@webxperts.co.za · +27 65 961 8206 · Instagram / WhatsApp / Facebook).
- Testimonials: one real named review (Mike K., Orange Group) + two real Google reviews.

## TODO / still placeholder (do NOT present as final/real)

- [ ] **Portfolio**: 9 case studies + images are illustrative. Swap for real projects —
      see **PORTFOLIO-IMAGES.md** for exact image specs, capture guidance, and the
      drop-in swap prompt. Images go in `assets/portfolio/`.
- [ ] **Trust marquee** client names are placeholders.
- [ ] **Testimonials**: two are attributed "Verified Google review" — add real names.
- [ ] **Stats** (8+ years / 120+ projects / 4.9 rating) are placeholders — confirm real numbers.
- [ ] **Contact form** is front-end simulated only. Wire `#contactForm` submit to a real
      endpoint (Formspree / Getform / own API). There's a comment in the JS marking where.
- [ ] Social URLs use `/webxperts` handles — confirm they're correct.
- [ ] Decide: keep dark/cream alternation, or go fully dark (two near-black tones) for a
      more uniform gold-on-black look. (Open design question from the client.)

## Possible next steps (nice-to-haves, not started)

- Split the single file into partials or a small build (e.g. Vite) if it grows.
- Extract the 5 routes into real static pages for non-JS crawlability (currently SPA).
- Add Open Graph image, sitemap.xml, robots.txt for deploy.

## Conventions

- Keep it a single static file unless deliberately introducing a build step.
- Match existing code style (vanilla JS, no framework; CSS custom properties; BEM-ish
  class names). No inline styles except the few one-off layout nudges already present.
- Test responsive (mobile menu, the horizontal pin disabling < 900px) and reduced-motion
  after any motion/layout change.
