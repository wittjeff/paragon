---
title: 'Stack'
type: 'component'
components:
- Stack
categories:
- Layout
status: 'New'
designStatus: 'Done'
devStatus: 'In progress'
notes: |
---

Shorthand helpers that build on top of our flexbox utilities to make component layout faster and easier than ever. Similar to the [Bootstrap Stack](https://react-bootstrap.github.io/docs/layout/stack/) component.

## Basic Usage

## Vertical direction

Stacks are vertical by default and stacked items are full-width by default. Watch this pull request to see more details about the [auto stretching behavior](https://github.com/openedx/paragon/pull/1188).

```jsx live
<Stack gap={3}>
  <Button>first button</Button>
  <Button>second button</Button>
  <Button>third button</Button>
</Stack>
```

## Horizontal direction

```jsx live
<Stack direction="horizontal" gap={3}>
  <div className="border p-2">first block</div>
  <div className="border p-2">second block</div>
  <div className="border p-2">third block</div>
</Stack>
```

## Reversed props

- **Vertical** with `reversed` prop
```jsx live
<Stack gap={3} reversed>
  <Button>first button</Button>
  <Button>second button</Button>
  <Button>third button</Button>
</Stack>
```
- **Horizontal** with `reversed` prop
```jsx live
<Stack direction="horizontal" gap={3} reversed>
  <div className="border p-2">first block</div>
  <div className="border p-2">second block</div>
  <div className="border p-2">third block</div>
</Stack>
```
## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Stack` is a flexbox wrapper: it renders a single `<div>` with `pgn__vstack`/`pgn__hstack` classes. There is no matching [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/).

Stack renders static content and has no keyboard interaction of its own. No ARIA attributes are rendered.

**Direction and visual order caveat:** the `reversed` prop is implemented purely in CSS (`flex-direction: column-reverse` / `row-reverse`). It reverses the visual order without changing DOM order, so reading order and <kbd>Tab</kbd> order run opposite to what sighted users see (WCAG 1.3.2, 2.4.3).

### Application responsibilities

- When the order of stacked items is meaningful — and always when they are interactive (buttons, links, form fields) — reorder the children in source instead of using `reversed`.
- Reserve `reversed` for purely decorative arrangements where sequence carries no meaning.
- The stack itself conveys no semantics; if the items form a list, render a real list (`<ul>`/`<li>`) as the stack's content — `Stack` has no `as` prop and always renders a `<div>`.

### Relevant ACT rules

No ACT rule tests CSS flexbox ordering directly; conformance with WCAG 1.3.2 (Meaningful sequence) and 2.4.3 (Focus order) when `reversed` is used requires manual review of the rendered page.

### Known issues

None identified in the August 2026 accessibility audit.
