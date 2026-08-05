---
title: 'Icon'
type: 'component'
components:
- Icon
categories:
- Imagery & Iconography
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |
---

Displays an svg icon from `@openedx/paragon/icons`. See [Icons Foundation Documentation](/foundations/icons) for a list of all available icons.

## Basic Usage

```jsx live
// Included in this live jsx scope:
// import { Add, AddCircle } from '@openedx/paragon/icons';
<Icon src={Add} />
```
### With HTML attributes

HTML attributes can be passed to this component allowing for customization of the color, size, or addition of any necessary ARIA attributes.

```jsx live
// Included in this live jsx scope:
// import { Add, AddCircle } from '@openedx/paragon/icons';
<div className="d-flex align-items-center bg-dark">
  <Icon src={Add} className="mx-3 text-white" />
  <Icon src={Add} className="mx-3 text-white" size="xs" />
  <Icon src={Add} className="mx-3 text-white" size="sm" />
  <Icon src={Add} className="mx-3 text-white" size="md" />
  <Icon src={Add} className="mx-3 text-white" size="lg" />
  <Icon src={Add} className="text-white" style={{ height: '48px', width: '48px' }} />
</div>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Icon` renders an inline svg inside a `<span>`. There is no matching [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/); the relevant question is whether the icon is decorative (the default) or carries meaning.

Icon renders static content and has no keyboard interaction of its own; the svg is kept out of the tab order (`focusable="false"`, an IE11 workaround).

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| svg | `role="img"`, `focusable="false"` | Always set when `src` is provided. |
| svg (default) | `aria-hidden="true"` | Applied whenever `svgAttrs` contains neither `aria-label` nor `aria-labelledby` — **icons are decorative by default**. |
| svg (labeled) | `aria-label` or `aria-labelledby` from `svgAttrs` | When either is passed, `aria-hidden` is not set and the svg is exposed with that name. |
| `screenReaderText` | sibling `<span class="sr-only">` | Visually hidden text read in place of the (still hidden) svg. |
| Legacy rendering (no `src`) | `<span>` with `aria-hidden` = `hidden` prop (default `true`) | Icon-font usage. Note the `hidden` prop has no effect when `src` is provided. |

### Screen reader expectations

- By default the icon is skipped entirely; only `screenReaderText`, if provided, is read as plain text. *(Verification pending per the policy's AT matrix.)*
- With `svgAttrs={{ 'aria-label': ... }}`, the svg announces as that label plus "image"/"graphic". *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Keep the decorative default when the icon sits next to text that already carries the meaning (e.g. an icon inside a labeled button).
- When the icon is the only carrier of meaning, provide exactly one text alternative: `screenReaderText` (treat it like `alt` on an image) **or** `svgAttrs['aria-label']` — supplying both produces a double announcement.
- Never rely on an icon to label an interactive control — name the control itself (e.g. `IconButton`'s required `alt`), not the icon inside it.
- Icon color comes from the surrounding text color; when an icon conveys state (e.g. error), don't let color be the only signal (WCAG 1.4.1).

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [SVG element with explicit role has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/7d6734/) — applies when the svg is labeled via `svgAttrs`
- [Image not in the accessibility tree is decorative](https://www.w3.org/WAI/standards-guidelines/act/rules/e88epe/) — the `aria-hidden` default
- [Element with aria-hidden has no content in sequential focus navigation](https://www.w3.org/WAI/standards-guidelines/act/rules/6cfa84/) — satisfied via `focusable="false"`

### Known issues

None identified in the August 2026 accessibility audit.
