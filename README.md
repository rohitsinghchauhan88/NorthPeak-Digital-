# NorthPeak Digital

A one-page site for a fictional growth agency, built with vanilla HTML/CSS/JS (no framework, no page builder).

## Structure
```
index.html
css/styles.css
js/script.js
```

## Sections
- Hero with headline + dual CTA
- Services grid (6 disciplines)
- Results / "trail report" testimonials (3, each with a stat)
- Pricing (3 tiers: Basecamp / Ascent / Summit)
- Contact form with client-side validation (required fields, email format check, inline error messages, accessible via `aria-live`/`aria-invalid`)

## Responsive breakpoints
- Base styles target desktop (1440px+)
- `@media (max-width: 900px)` — tablet (768px): services grid drops to 2 columns, pricing/testimonials stack to 1 column
- `@media (max-width: 640px)` — mobile (360px): hamburger nav, single-column everything, stacked hero actions

## Running locally
No build step. Just open `index.html` in a browser, or serve the folder:
```
python3 -m http.server 8000
```

## Deploying

### Option A — Netlify (drag and drop)
1. Go to https://app.netlify.com/drop
2. Drag the whole `northpeak` folder onto the page
3. Netlify gives you a live URL immediately (you can rename it in Site settings)

### Option B — Vercel
```
npm i -g vercel
cd northpeak
vercel --prod
```

### Option C — GitHub Pages
```
cd northpeak
git init
git add .
git commit -m "Initial commit: NorthPeak Digital one-page site"
git branch -M main
git remote add origin https://github.com/<your-username>/northpeak-digital.git
git push -u origin main
```
Then in the repo: **Settings → Pages → Source: Deploy from branch → main / (root)**. Your live URL will be `https://<your-username>.github.io/northpeak-digital/`.

## Notes for Task B (optimization pass)
This build already has a head start on performance/accessibility:
- Semantic landmarks (`header`, `main`, `nav`, `footer`), skip link, visible focus states, `aria-live` form status, labeled inputs
- System font stack fallback isn't used — Google Fonts are loaded via `<link>` with `preconnect`; swap to `font-display: swap` (already set) or self-host for a Lighthouse Performance boost
- SVGs are inline/hand-coded, no image assets yet — if you add photography, remember to compress + lazy-load (`loading="lazy"`) and set explicit width/height to avoid layout shift
