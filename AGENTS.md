# AGENTS.md

## Stack
- **Astro v6** — static site (`output: 'static'`), no SSR, no React islands
- **Tailwind CSS v4** — config via `@theme` directive in `src/styles/global.css`, NOT `tailwind.config.js`
- **Vite plugin**: `@tailwindcss/vite` injected in `astro.config.mjs`
- **Node** >= 22.12.0
- **TypeScript**: `astro/tsconfigs/strict` base, no `any` anywhere per parent rules

## Commands
```bash
npm run dev        # local server at localhost:4321
npm run build      # outputs to dist/
npm run preview    # preview build locally
npm run astro ...  # CLI passthrough (e.g., npm run astro check)
npm run check      # DOES NOT EXIST — don't use. Use `npx astro check` for type checking.
```

## Architecture
Single-page landing site: `pages/index.astro` → `layouts/Layout.astro` + 7 section components in `src/components/`.

- `Layout.astro` — HTML shell, meta tags, OG tags, Schema.org JSON-LD, fonts (Inter + Playfair Display from Google Fonts), and the IntersectionObserver that powers scroll reveals
- `Navbar.astro` — fixed top nav with desktop/mobile variants, inline scroll-effect script
- `Hero.astro` — full-viewport hero
- `About.astro` — brand story + stats
- `Categories.astro` — 6-category grid with hover overlays
- `Values.astro` — 4-value cards + quote block
- `Contact.astro` — contact info + form (client-side only, no API backend)
- `Footer.astro` — site footer with social links and nav

## Tailwind v4 Conventions
Brand tokens are defined in `src/styles/global.css` under `@theme`:
```css
@theme {
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --font-serif: 'Playfair Display', Georgia, serif;
  --color-brand-black: #0a0a0a;
  --color-brand-dark: #1a1a1a;
  --color-brand-gold: #c9a96e;
  --color-brand-gold-light: #d4b87a;
  --color-brand-cream: #f5f5f0;
  --color-brand-gray: #666666;
  --color-brand-gray-light: #999999;
}
```
Use these as Tailwind classes: `text-brand-gold`, `bg-brand-cream`, `border-brand-gold/30`, etc.
Do NOT create a `tailwind.config.js` file — Tailwind v4 uses CSS-based config only.

## Scroll Animations
- CSS classes `.reveal`, `.reveal.visible`, `.reveal-delay-{1,2,3,4}` live in `global.css`
- IntersectionObserver in `Layout.astro` adds `.visible` class when elements enter viewport
- Add `class="reveal"` to any element that should animate in on scroll
- Add `reveal-delay-{n}` for staggered delays
- One-time animation (observer unobserves after triggering), no re-trigger on scroll-back

## Style Patterns
- Sections alternate backgrounds: `bg-brand-cream` (light) ↔ `bg-brand-dark` / `bg-brand-black` (dark)
- Primary CTAs: `bg-brand-gold text-brand-black` buttons
- Secondary CTAs: `border border-white/{opacity} text-white` outlined buttons
- Serif headings (`font-serif`), sans-serif body (`font-sans`)
- Mobile-first responsive: base → `md:` → `lg:`
- Icons: inline SVG only, no icon library

## Contact Form
The form in `Contact.astro` is **client-side only** — it shows a success message on submit but sends no data anywhere. The fetch call to `/api/contact` is commented out. If a backend is needed, create the endpoint and uncomment/update the fetch logic.

## Files to Know
- `public/_headers` — Cloudflare Pages security headers (X-Frame-Options, CSP, etc.)
- `.github/workflows/` — empty; no CI configured
- `.claude/settings.local.json` — Claude Code permissions config

## Do NOT Apply
This repo does NOT use: React, SSR, Cloudflare D1/R2/KV bindings, Vite path aliases, CVA, Radix, shadcn/ui, Resend, i18n routing, honeypot fields, or rate limiting. Those come from a parent template that doesn't match this repo's actual tech stack. Always verify against the real codebase before following parent-level conventions.
