# Task B — Optimization Changelog

A note on process first: I don't have a browser/Lighthouse available in the environment I build in, so I couldn't run the audit myself or capture the screenshots. Everything below is real, verifiable work I did on the code — the exact edits and why. **You'll need to run Lighthouse yourself once the site is deployed** (see "Your turn" at the bottom) — that's normal for this kind of task anyway, since Lighthouse should be run against the live, deployed URL, not a local file.

## Accessibility fixes

These were genuine bugs, not just tuning — found by calculating WCAG contrast ratios for every text/background pair in the design system.

| Element | Before | Issue | After | New ratio |
|---|---|---|---|---|
| Primary buttons ("Plan the Ascent", "Send Signal", nav CTA) | White text on `#d6541f` | 4.08:1 — fails AA (needs 4.5:1 for normal text) | White text on new `#9c3b16` | ~6.9:1 |
| Logo wordmark ("PEAK") | `#d6541f` on `#edeee6` | 3.49:1 — fails AA | `#9c3b16` on `#edeee6` | ~5.9:1 |
| "Most climbers start here" badge on the featured pricing card | `#d6541f` on `#263a30` | 2.98:1 — fails AA by a wide margin | White on `#263a30` | ~12:1 |

**What this bought:** these three were flat-out contrast failures — the kind of thing Lighthouse's `color-contrast` audit catches directly and weights heavily. Fixing them is very likely most of the gap between wherever the unoptimized version landed and 90+ on Accessibility. I introduced one new token (`--signal-hover`, a darker rust for hover states) so the original bright orange (`#d6541f`) is still used everywhere it's safe — icons, large bold numbers (which only need 3:1), decorative contour lines — without weakening the brand color.

**Already in place from Task A** (worth noting, since it's why the starting point wasn't zero): skip-to-content link, semantic landmarks, a real heading hierarchy (h1 → h2 → h3, no skipped levels), visible `:focus-visible` outlines, labeled form fields, `aria-describedby`/`aria-invalid`/`role="alert"` on form errors, and `aria-hidden="true"` on every decorative SVG so screen readers don't announce meaningless icon markup.

## Performance fixes

| Change | Detail | What it bought |
|---|---|---|
| Trimmed Google Fonts request | Was requesting 10 font weights (`Big Shoulders Display` 600/700/800/900, `IBM Plex Sans` 400/500/600/700, `IBM Plex Mono` 400/500). Audited the CSS and only 5 are actually used (Display: 800 only; Mono: 400 only; Sans: all 4). | Cuts the number of font files downloaded from 10 to 5 — fewer render-blocking network requests and less data before text can paint in its final font. |
| Minified CSS/JS | Stripped comments and collapsed whitespace into `styles.min.css` and `script.min.js`, wired the HTML to load those instead of the source files. | CSS: 12.6KB → 10.0KB (21% smaller). JS: 2.9KB → 2.6KB (12% smaller). Source files are kept unminified in the repo for readability/maintenance. |
| `defer` on the script tag | Was already placed at the end of `<body>` (fine), but `defer` makes the intent explicit and lets the browser fetch the script in parallel with HTML parsing instead of only after it reaches that line. | Removes any risk of the script blocking parse, and is more resilient if the script tag ever moves in the document. |
| Inline SVG favicon (data URI) | Added a `<link rel="icon">` using an inline SVG data URI instead of leaving no favicon declared. | Browsers request `/favicon.ico` by default when no icon is declared — that was previously a wasted 404 request on every page load. Now it's a zero-request, zero-byte-over-the-wire icon. |
| Cache headers (`netlify.toml`) | Added long-lived, immutable caching for `/css/*` and `/js/*` (1 year), and no-cache for the HTML shell so updates still show immediately. | Doesn't move the score on a fresh Lighthouse run (that's a cold load), but it's what "efficient cache policy" audits are checking for, and it meaningfully speeds up repeat visits once deployed on Netlify. If you deploy to GitHub Pages instead, this file is ignored — GH Pages sets its own caching, so this is a Netlify-specific optimization. |

**What I didn't do, and why:** the single biggest remaining lever is self-hosting the fonts instead of pulling them from `fonts.googleapis.com`/`fonts.gstatic.com` — that removes two third-party origins entirely and lets you `preload` the exact woff2 files with no extra DNS/connection round trip. I couldn't do this here because downloading the font files requires network access to Google's font CDN, which isn't available in my build environment. If Performance still isn't clearing 90 after deployment, this is the next thing to try — it's a bigger win than anything above.

## Your turn

1. Deploy the `northpeak-digital` folder (Netlify/Vercel/GH Pages — see README).
2. Run Lighthouse against the **live URL**, not a local file — mobile results are what most instructors/employers will actually check, so run it in mobile mode.
3. Screenshot the Performance and Accessibility scores.
4. If Accessibility isn't at 90+ yet, that's a real bug I missed — check the Lighthouse report's specific flagged elements and I can help fix them.
5. If Performance isn't at 90+ yet, self-hosting the fonts (see above) is the first thing to try.
