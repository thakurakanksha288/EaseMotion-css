# Animated Notification Bell

An animated notification bell icon with an unread-count badge, covering three variants (numeric, dot-only, 99+ overflow) and three state-class-driven animations (ring-shake, badge pop-in, badge pulse-ring). Pure CSS, zero dependencies.

---

## Folder structure

```
submissions/examples/notification-bell/
├── demo.html   ← interactive demo
├── style.css    ← component styles
└── README.md
```

---

## Variants

| Variant | Class modifier | Badge content |
|---------|---------------|---------------|
| Numeric badge | *(none)* | Count text, e.g. `3` |
| Dot-only | `ease-bell--dot` | Empty — just a dot |
| 99+ overflow | `ease-bell--overflow` | `99+` or any long text |

---

## Markup

The component is a single wrapper element (any tag; `<button>` is recommended for accessibility) that contains an inline SVG and a badge `<span>`.

```html
<!-- Basic (numeric badge) -->
<button class="ease-bell" aria-label="Notifications">
  <svg class="ease-bell__icon" viewBox="0 0 24 24" aria-hidden="true">
    <path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/>
    <path d="M13.73 21a2 2 0 0 1-3.46 0"/>
  </svg>
  <span class="ease-bell__badge">3</span>
</button>

<!-- Dot-only variant -->
<button class="ease-bell ease-bell--dot" aria-label="Notifications">
  <svg class="ease-bell__icon" viewBox="0 0 24 24" aria-hidden="true">
    <path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/>
    <path d="M13.73 21a2 2 0 0 1-3.46 0"/>
  </svg>
  <span class="ease-bell__badge"></span>
</button>

<!-- 99+ overflow variant -->
<button class="ease-bell ease-bell--overflow" aria-label="Notifications">
  <svg class="ease-bell__icon" viewBox="0 0 24 24" aria-hidden="true">
    <path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/>
    <path d="M13.73 21a2 2 0 0 1-3.46 0"/>
  </svg>
  <span class="ease-bell__badge">99+</span>
</button>
```

---

## State classes

Add or remove these classes on the `.ease-bell` wrapper from your JavaScript notification logic.

### `.has-unread`

**Purpose:** Reveals the badge and starts the continuous pulse-ring animation.  
**Add when:** There are unread notifications.  
**Remove when:** The user opens/reads all notifications.

```js
// Show badge + pulse ring
bellEl.classList.add('has-unread');

// Hide badge + stop pulse
bellEl.classList.remove('has-unread');
```

Badge count update (retrigger pop animation):
```js
badge.textContent = newCount;
badge.style.animation = 'none';
void badge.offsetWidth; // force reflow
badge.style.animation = '';
```

---

### `.is-ringing`

**Purpose:** Plays a one-shot ring-shake (rotation oscillation) on the bell icon.  
**Add when:** A new notification arrives.  
**Remove when:** Animation ends (use `animationend` event).

```js
function ring(bellEl) {
  bellEl.classList.remove('is-ringing');
  void bellEl.offsetWidth; // force reflow so re-trigger works
  bellEl.classList.add('is-ringing');
  bellEl.addEventListener(
    'animationend',
    () => bellEl.classList.remove('is-ringing'),
    { once: true }
  );
}
```

---

## Wiring both states together (typical usage)

```js
async function checkNotifications(bellEl, badgeEl) {
  const { count } = await fetch('/api/notifications/unread').then(r => r.json());

  if (count > 0) {
    badgeEl.textContent = count > 99 ? '99+' : count;
    badgeEl.setAttribute('aria-label', `${count} unread notifications`);
    bellEl.classList.add('has-unread');
    ring(bellEl); // shake on new arrivals
  } else {
    bellEl.classList.remove('has-unread', 'is-ringing');
  }
}
```

---

## Animations

| Animation | Keyframe name | Trigger | Duration |
|-----------|--------------|---------|----------|
| Ring-shake | `ease-bell-shake` | `.is-ringing` (one-shot) | 600 ms |
| Badge pop-in | `ease-badge-pop` | `.has-unread` applied (one-shot) | 350 ms |
| Badge pulse-ring | `ease-badge-pulse` | `.has-unread` active (looping) | 1.8 s |

All animations are suppressed when `prefers-reduced-motion: reduce` is detected, while badge visibility is preserved.

---

## CSS custom properties

Override these on `:root` or a scoped selector to theme the component.

| Property | Default | Description |
|----------|---------|-------------|
| `--ease-bell-size` | `2rem` | Wrapper width/height |
| `--ease-bell-color` | `#374151` | Icon stroke (idle) |
| `--ease-bell-color-active` | `#111827` | Icon stroke (hover/focus) |
| `--ease-bell-badge-bg` | `#ef4444` | Badge background |
| `--ease-bell-badge-color` | `#ffffff` | Badge text color |
| `--ease-bell-badge-size` | `1.1rem` | Badge min-width/height |
| `--ease-bell-dot-size` | `0.55rem` | Dot variant size |
| `--ease-bell-ring-color` | `rgba(239,68,68,0.45)` | Pulse-ring color |
| `--ease-bell-duration-shake` | `600ms` | Ring-shake duration |
| `--ease-bell-duration-pop` | `350ms` | Badge pop-in duration |
| `--ease-bell-duration-pulse` | `1.8s` | Pulse-ring loop period |
| `--ease-bell-easing-bounce` | `cubic-bezier(0.34,1.56,0.64,1)` | Pop-in easing |

---

## Accessibility

- Use a `<button>` wrapper so the bell is keyboard-focusable.
- Set `aria-label="Notifications"` on the wrapper.
- Add `aria-live="polite"` and `aria-atomic="true"` on the wrapper so screen readers announce badge count changes.
- Set a meaningful `aria-label` on `.ease-bell__badge` (e.g. `"3 unread notifications"`).
- The icon SVG uses `aria-hidden="true"` since the wrapper label is sufficient.

---

## License

Part of the EaseMotion CSS project. MIT License.