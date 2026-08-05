---
title: 'Carousel'
type: 'component'
components:
- Carousel
- CarouselItem
- CarouselCaption
categories:
- Content
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |
  A pass through from react-bootstrap
---

<p className="lead">
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/carousel/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for documentation.
  </a>
</p>

## Basic Usage

```jsx live
<Carousel>
  <Carousel.Item>
    <img
      className="d-block w-100"
      src="https://picsum.photos/1600/800/"
      alt="First slide"
    />
    <Carousel.Caption>
      <h3>First slide label</h3>
      <p>Nulla vitae elit libero, a pharetra augue mollis interdum.</p>
    </Carousel.Caption>
  </Carousel.Item>
  <Carousel.Item>
    <img
      className="d-block w-100"
      src="https://picsum.photos/1600/800/"
      alt="Third slide"
    />

    <Carousel.Caption>
      <h3>Second slide label</h3>
      <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
    </Carousel.Caption>
  </Carousel.Item>
  <Carousel.Item>
    <img
      className="d-block w-100"
      src="https://picsum.photos/1600/800/"
      alt="Third slide"
    />

    <Carousel.Caption>
      <h3>Third slide label</h3>
      <p>Praesent commodo cursus magna, vel scelerisque nisl consectetur.</p>
    </Carousel.Caption>
  </Carousel.Item>
</Carousel>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Paragon's `Carousel` is a thin wrapper around [React-Bootstrap's Carousel](https://react-bootstrap-v4.netlify.app/components/carousel/) (pinned at 1.6.5) that changes only the default `nextLabel`/`prevLabel` values. The relevant reference is the [APG carousel pattern](https://www.w3.org/WAI/ARIA/apg/patterns/carousel/); the shipped implementation deviates from it substantially, and the carousel **auto-rotates by default** (`interval` defaults to `5000` ms) — see the deviation note and Known issues.

### Keyboard support

Provided by React-Bootstrap:

| Key | Action |
| --- | --- |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | On the previous/next controls: shows the previous/next slide. |
| <kbd>Arrow Left</kbd> / <kbd>Arrow Right</kbd> | Shows the previous/next slide, but **only while focus is inside the carousel** (the key handler sits on the carousel `<div>`, which is not itself focusable). Disabled with `keyboard={false}`. |

The slide-position indicators cannot be reached or operated by keyboard at all (see Known issues). There is no key or control that pauses auto-rotation; hovering with the mouse pauses it (`pause="hover"`, the default), and moving the mouse away resumes it. Keyboard focus does not pause rotation.

### ARIA roles, states, and properties

All rendered by React-Bootstrap:

| Element | Attribute | Notes |
| --- | --- | --- |
| Carousel container | *(no role)* | Plain `<div class="carousel slide">`; no `role="region"`, `aria-roledescription="carousel"`, or `aria-label`. |
| Previous/next controls | `<a href="#" role="button">` | Anchor-as-button via React-Bootstrap `SafeAnchor`; <kbd>Space</kbd> is handled in script. |
| Control icons | `aria-hidden="true"` | Paragon's default `prevIcon`/`nextIcon` spans are decorative. |
| Control labels | `.sr-only` text | Visually hidden span with `prevLabel`/`nextLabel`; Paragon defaults are the untranslated strings "Previous" and "Next". |
| Indicators | `<ol>` of `<li onClick>` | No role, no accessible name, not focusable — mouse/touch only. |
| Slides (`Carousel.Item`) | *(no role)* | `<div class="carousel-item">`. Inactive slides are `display: none` (removed from the accessibility tree); no `aria-roledescription="slide"` or positional label. |
| Slide changes | *(no live region)* | Slide changes are not announced. |

**Deviations from the APG pattern (identified in the August 2026 accessibility audit):** the pattern calls for a labeled `region` with `aria-roledescription="carousel"`, a visible rotation-control (pause/stop) button, auto-rotation that stops permanently on focus or hover, indicators exposed as buttons (or tabs), slides labeled "x of y", and `aria-live` announcements when rotation is stopped. None of these are present in React-Bootstrap 1.6.5.

### Screen reader expectations

- The previous/next controls announce as buttons named with `prevLabel`/`nextLabel` — e.g. "Previous, button". *(Verification pending per the policy's AT matrix.)*
- Only the active slide's content is readable; users are given no indication that further slides exist beyond the two control buttons, and a slide change under a screen reader's virtual cursor is not announced.
- Because rotation continues during screen reader use, content may change while it is being read.

### Application responsibilities

- **Turn off auto-rotation** by passing `interval={null}` unless you also provide a visible pause/stop control; content that changes on a timer with no way to pause it fails WCAG 2.2.2 (Pause, Stop, Hide).
- Provide localized `prevLabel`/`nextLabel` — the Paragon defaults are hard-coded English strings, not `react-intl` messages.
- Give every slide image a meaningful `alt`, and keep slide text (e.g. `Carousel.Caption`) sufficient on its own since position ("2 of 3") is not conveyed.
- Prefer `indicators={false}` and rely on the previous/next controls, or build separate, focusable slide-picker buttons — the built-in indicators are inaccessible.
- If a slide contains interactive elements, do not auto-rotate: focus can sit inside a slide that rotates to `display: none`, silently destroying the user's position.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Text content that changes automatically can be paused, stopped or hidden](https://www.w3.org/WAI/standards-guidelines/act/rules/efbfc7/) — auto-rotating slides
- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — the previous/next controls (`role="button"`)
- [Image has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/23a2a8/) — slide images
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/) — the controls
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/)

### Known issues

Identified in the August 2026 accessibility audit (all inherited from React-Bootstrap 1.6.5):

- Auto-rotation is on by default and cannot be paused, stopped, or hidden by keyboard or any on-screen control; only mouse hover pauses it, and keyboard focus does not (WCAG 2.2.2).
- Slide-position indicators are unlabeled `<li>` elements with click handlers — not focusable, not named, not operable by keyboard or assistive technology (WCAG 1.1.1, 2.1.1, 4.1.2).
- Arrow-key slide switching only works when focus happens to be inside the carousel, and the carousel exposes no region role, label, or slide announcements, deviating from the APG carousel pattern throughout.
- Paragon's default control labels ("Previous"/"Next") are hard-coded English constants rather than localized messages.
