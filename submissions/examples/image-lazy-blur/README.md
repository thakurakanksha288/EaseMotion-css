# image-lazy-blur

**EaseMotion CSS · Blur-up Image Lazy Loading Placeholder**

Simulates the progressive "blur-up" image loading pattern popularised by Medium.
A tiny low-resolution placeholder renders instantly, blurred via CSS. When the
full-resolution image loads, it fades and unblurs into view with a smooth scale
transition — all driven by CSS custom properties and a small vanilla observer.

---

## Classes

| Class | Applied to | Purpose |
|---|---|---|
| `ease-lazy-wrap` | Container `<div>` | Positions layers, clips overflow, shows shimmer skeleton |
| `ease-lazy-placeholder` | Low-res `<img>` | Absolutely-positioned blurred stand-in |
| `ease-lazy-img` | Full-res `<img>` | Hidden until loaded; transitions in on `.ease-lazy-loaded` |
| `ease-lazy-loaded` | Added to wrapper by JS | Triggers the CSS reveal transition |
| `ease-lazy-fade` | Optional on wrapper | Disable scale; use fade-only reveal |
| `ease-lazy-caption` | Optional `<div>` inside wrapper | Slides up caption on hover |

---

## Usage

```html
<div class="ease-lazy-wrap" style="width:320px; height:220px;">
  <!-- Tiny placeholder (e.g. 20×14 px) — CSS blurs it to fill -->
  <img
    class="ease-lazy-placeholder"
    src="path/to/tiny-placeholder.jpg"
    alt=""
    aria-hidden="true"
  />
  <!-- Full image — set data-src; JS copies to src when in view -->
  <img
    class="ease-lazy-img"
    data-src="path/to/full-image.jpg"
    src=""
    alt="Descriptive alt text"
    width="320"
    height="220"
  />
  <!-- Optional caption -->
  <div class="ease-lazy-caption">Your caption here</div>
</div>
```

Add the activation script (from `demo.html`) once per page.

---

## CSS Custom Properties

| Variable | Default | Description |
|---|---|---|
| `--ease-lazy-duration` | `0.6s` | Reveal transition duration |
| `--ease-lazy-easing` | `cubic-bezier(0.4,0,0.2,1)` | Easing curve |
| `--ease-lazy-blur-amount` | `20px` | Starting blur on placeholder |
| `--ease-lazy-scale-start` | `1.05` | Starting scale on full image |
| `--ease-lazy-radius` | `12px` | Border radius |
| `--ease-lazy-shadow` | `0 8px 32px …` | Box shadow |

---

## Accessibility

- Placeholder images use `aria-hidden="true"` and empty `alt`
- Full images require a meaningful `alt` attribute
- `prefers-reduced-motion` disables blur/scale; only opacity fades remain
- Caption is keyboard-accessible via `:focus-within`

---

## Features

- ✅ No external CDN dependencies
- ✅ `ease-*` prefixed classes throughout
- ✅ Light/dark agnostic — CSS custom properties for full theming
- ✅ `prefers-reduced-motion` respected
- ✅ Shimmer skeleton while nothing is loaded
- ✅ Two variants: blur+scale (default) and fade-only (`ease-lazy-fade`)
- ✅ Optional hover caption overlay
- ✅ IntersectionObserver with graceful fallback