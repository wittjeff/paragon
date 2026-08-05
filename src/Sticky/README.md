---
title: 'Sticky'
type: 'component'
components:
- Sticky
categories:
- Content
status: 'New'
designStatus: 'Done'
devStatus: 'In progress'
notes: |
---

Makes elements remain at the top or bottom of the viewport, like a sticky navbar.

## Basic Usage

### Top positioning

```jsx live
<div style={{ display: 'flex', width: '100%', height: '400px', border: '2px dashed grey' }}>
  <Sticky>
    <div style={{ background: '#0000aa', width: '100%', height: '150px', color: '#ffffff'}}>
      Sticky to the top
    </div>
  </Sticky>
</div>
```

### Bottom positioning

```jsx live
<div style={{ display: 'flex', width: '100%', height: '400px', border: '2px dashed grey' }}>
  <Sticky position="bottom">
    <div style={{ background: '#aa0000', width: '100%', height: '150px', color: '#ffffff' }}>
      Sticky to the bottom
    </div>
  </Sticky>
</div>
```

### With offset

Valid `offset` values are the same as for [the spacing classes](/foundations/spacing).

```jsx live
<div style={{ display: 'flex', width: '100%', height: '400px', border: '2px dashed grey' }}>
  <Sticky offset={3.5}>
    <div style={{ background: '#00aa00', width: '100%', height: '150px', color: '#ffffff' }}>
      Sticky with offset
    </div>
  </Sticky>
</div>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Sticky` is a positioning wrapper: it renders a single `<div>` with CSS `position: sticky` classes (plus a shadow class toggled by an `IntersectionObserver`) and no ARIA attributes. It renders static content and has no keyboard interaction of its own; no [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) applies. Sticky positioning is invisible to assistive technology — screen readers read the content in DOM order regardless of where it is pinned.

### ARIA roles, states, and properties

No ARIA roles, states, or properties are rendered.

### Application responsibilities

- Content pinned over the page can obscure other content. Ensure focused elements are not hidden behind a sticky bar as users tab through the page — e.g. give the page a matching `scroll-padding` — per WCAG 2.4.11 (Focus Not Obscured) and 2.4.7.
- Check the layout at 400% zoom / 320 px width: a sticky region that grows to consume most of the viewport can make the remaining content unusable (WCAG 1.4.10 Reflow). Consider disabling stickiness at small viewport heights.
- Keep DOM order sensible: place a bottom-pinned bar at the end of its container so visual and reading order match (WCAG 1.3.2).
- The wrapper is generic — if the sticky content is a navigation bar or toolbar, supply the appropriate landmark or role on your own content.

### Relevant ACT rules

No ACT rule targets sticky positioning itself; these page-level rules commonly interact with it:

- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/) — focus must not be hidden behind the sticky region
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — sticky content overlays varying backgrounds

### Known issues

None identified in the August 2026 accessibility audit.
