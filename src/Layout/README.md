---
title: 'Layout'
type: 'component'
components:
- Layout
categories:
- Layout
status: 'New'
designStatus: 'Done'
devStatus: 'Done'
---

A wrapper component that allows to control the size of child blocks on different screen sizes. It is a shorthand for the grid system (12 columns). Also see [Layout](/foundations/layout) foundations.

## Basic usage

```jsx live
  <Layout
    lg={[{ span: 4, offset: 0 }, { span: 4, offset: 0 }, { span: 4, offset: 0 }]}
    md={[{ span: 'auto', offset: 0 }, { span: 'auto', offset: 0 }, { span: 'auto', offset: 0 }]}
    sm={[{ span: 8, offset: 0 }, { span: 4, offset: 0 }, { span: 6, offset: 6 }]}
    xs={[{ span: 4, offset: 0 }, { span: 4, offset: 0 }, { span: 4, offset: 0 }]}
    xl={[{ span: 3 }, { span: 6 }, { span: 3 }]}
  >
    <Layout.Element style={{ background: '#FFD89E' }}>first block</Layout.Element>
    <Layout.Element style={{ background: '#4CAF50' }}>second block</Layout.Element>
    <Layout.Element style={{ background: '#FFEB3B' }}>third block</Layout.Element>
  </Layout>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Layout` renders its children as React-Bootstrap 1.6.5 grid columns (`<div class="row">` containing `<div class="col-*">` elements). There is no matching [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/).

Layout renders static content and has no keyboard interaction of its own. No ARIA attributes are rendered.

A useful property of the current implementation: `Layout` accepts only `span` and `offset` per breakpoint and strips everything else, so the underlying `Col` `order` property cannot be used through it. Visual order therefore always follows source (DOM) order — offsets add empty space, they never reorder columns — which keeps reading order, focus order, and visual order aligned (WCAG 1.3.2, 2.4.3).

### Application responsibilities

- Order children so that source order is the meaningful reading order — it is also the focus order for any interactive content inside the columns.
- Column spans change per breakpoint and rows wrap; check that narrow-viewport wrapping still reads sensibly, since the sequence stays fixed while the visual arrangement changes.
- If you bypass `Layout` and use the re-exported `Row`/`Col` directly, avoid `Col`'s `order` prop for meaningful content — it decouples visual order from DOM order.
- The grid conveys no structure to assistive technology; use headings or landmarks inside columns when the blocks are distinct page regions.

### Relevant ACT rules

No ACT rule tests CSS grid layout directly; conformance with WCAG 1.3.2 (Meaningful sequence) and 2.4.3 (Focus order) for a given layout requires manual review of the rendered page.

### Known issues

None identified in the August 2026 accessibility audit.
