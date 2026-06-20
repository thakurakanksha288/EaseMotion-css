# Scroll-Driven Animations

Utility classes that tie EaseMotion CSS animations to native scroll progress
using `animation-timeline`, with zero JavaScript.

## What it adds
- `.em-scroll-timeline` — drives an animation by overall page scroll progress
  (e.g. a reading progress bar).
- `.em-scroll-view` — drives an animation by an element's visibility in the
  viewport (e.g. fade/scale-in on scroll).

## Usage
\`\`\`html
<div class="em-progress-bar em-scroll-timeline"></div>

<div class="em-fade-in em-scale-up em-scroll-view">...</div>
\`\`\`

## Why it fits EaseMotion CSS
Avoids IntersectionObserver/GSAP for common scroll effects, staying true to
the framework's zero-dependency, lightweight philosophy. Falls back safely
via `prefers-reduced-motion`.

## Browser support
Chrome/Edge 115+, Firefox 137+ (or behind flag in some versions), Safari TP.
Older browsers simply skip the animation — content is visible by default.