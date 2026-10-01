# NPS Calculator

A small browser-based tool for calculating and visualizing a Net Promoter Score. Enter response counts across the Detractor, Passive, and Promoter buckets and the score updates live, with a needle animating across a color-coded gauge.

Built with plain HTML, CSS, and JavaScript — no framework, no build step, no dependencies.

**Live demo:** https://nps-calculator.netlify.app/

## Why I built this

I wanted a small, self-contained exercise in DOM and SVG manipulation without reaching for a framework or a charting library — just vanilla JS, directly against the DOM API. NPS felt like a good fit: the math is simple, but mapping a score to a smoothly rotating gauge needle turned out to be a more interesting problem than it looks.

## Running it locally

There's no build step — it's just three static files.

```bash
git clone https://github.com/devlikhon/NPS-Calculator.git
cd NPS-Calculator
```

Then either:

- Open `index.html` directly in your browser, or
- Serve it with any static server (e.g. the VS Code "Live Server" extension) if you want auto-reload on save

No `npm install`, no env vars, nothing else to configure.

## How it works

NPS works by grouping responses into three buckets based on a 0–10 scale:

- **Detractors** (0–6) — unhappy or at-risk
- **Passives** (7–8) — neutral, satisfied but unenthusiastic
- **Promoters** (9–10) — loyal, enthusiastic

The score itself is:

```
NPS = % Promoters − % Detractors
```

which gives a number between -100 and 100. As you type a count into any input, `script.js` recalculates the totals and percentages, derives the NPS score, and updates two things on the page:

1. The score text in the center of the gauge
2. The needle's position — rotated and translated along the SVG arc to point at the correct spot

That second part was the fiddly bit. There's no clean formula for "rotate N degrees per point" across the whole -100 to 100 range, because the visual arc isn't evenly spaced at every score. I ended up hand-mapping ranges of scores to specific `rotate()`/`translate()` values in `calculatePolygonTransform()` — not elegant, but it keeps the needle visually accurate across the whole scale without pulling in a charting library for one shape.

## Project structure

```
index.html    Markup — input groups for each bucket, the SVG gauge
style.css     Layout, gauge styling, color bands, responsive breakpoints
script.js     Input handling, NPS calculation, gauge needle positioning
```

## Known limitations

- No persistence — refreshing the page resets all inputs to zero
- Input validation is minimal (non-negative integers only, no upper bound per field)
- The needle-position mapping in `calculatePolygonTransform()` is a set of hard-coded ranges rather than a continuous formula, so it's accurate but not the most maintainable piece of code here
- Single view only — no history of past calculations, no export
