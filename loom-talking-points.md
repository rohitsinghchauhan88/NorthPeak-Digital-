# Loom Walkthrough — Talking Points

I can't record video or audio myself, so here's a script you can talk through in ~2 minutes. Screen-record the Lighthouse report and the live site while you say this.

## 3 things to be proud of

**1. The contrast bugs were found by actual calculation, not guesswork.**
Walk through the changelog table. Point out that the brand's signature orange (`#d6541f`) looked fine to the eye on white text, but measured at 4.08:1 — just under the 4.5:1 AA threshold. Say something like: "This is the kind of failure you can't eyeball. I ran the WCAG contrast formula on every text/background pair in the design system and found three real failures, not just theoretical ones."

**2. Performance optimizations didn't touch the design.**
Show the site still looks identical before/after — same colors, same layout, same brand personality — but is lighter under the hood (fewer font weights requested, minified assets, no wasted favicon request). "None of this is a visual compromise. It's the same site, just leaner."

**3. The fixes are targeted, not blanket.**
Point out that you *kept* the bright orange everywhere it's actually safe to use — icons, large bold stat numbers (which only need 3:1 contrast), decorative lines — and only swapped it for the darker variant where small/normal text sat on top of it. "I didn't just darken everything to be safe. I checked each case against the actual WCAG size/weight thresholds."

## 1 thing you'd do differently

Be honest here — good options:
- "I'd self-host the fonts instead of pulling from Google Fonts. That's the single biggest remaining performance lever and I didn't get to it this round."
- "I'd add real automated testing — right now the contrast fixes were verified by hand-calculating luminance ratios. A `pa11y` or `axe-core` CI check would catch regressions automatically next time instead of relying on me remembering to re-check."
- Or, if true for you: "I'd get real user testing on the mobile nav before calling the accessibility pass done — automated tools like Lighthouse catch contrast and markup issues but not whether the interaction actually feels right with a screen reader or keyboard-only."
