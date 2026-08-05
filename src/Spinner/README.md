---
title: 'Spinner'
type: 'component'
status: 'Stable'
components:
- Spinner
categories:
- Status & metadata
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

A spinning animation that indicates loading.

<p>
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/spinners/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for documentation.
  </a>
</p>

## Basic Usage

```jsx live
<>
  <Spinner animation="border" className="mie-3" screenReaderText="loading" />
  <Spinner animation="grow" className="mie-3" screenReaderText="loading" />
</>
```
## Color Variants

```jsx live
() => {
  const variants = ['primary', 'secondary', 'success', 'danger', 'warning', 'info', 'light', 'dark'];
  return variants.map((variant) => (
    <Spinner animation="border" variant={variant} className="mr-3" screenReaderText="loading" />
  ));
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Spinner` is a loading indicator exposed through the ARIA [`status` role](https://www.w3.org/TR/wai-aria-1.1/#status); no [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) exists for spinners. The `screenReaderText` prop controls whether the spinner is exposed to assistive technology at all (see below). The spinner renders static content and has no keyboard interaction of its own.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Spinner `<div>` | `role="status"` | Set **only when `screenReaderText` is provided**; otherwise no role is rendered. |
| Child `<span class="sr-only">` | visually hidden text = `screenReaderText` | The spinner's only text content. |

Without `screenReaderText`, the component renders an empty, unlabeled `<div>` — nothing is exposed to assistive technology (see Known issues). Under `prefers-reduced-motion: reduce`, the spin/grow animation is slowed (1.5s per cycle), not removed.

### Screen reader expectations

- When a spinner with `screenReaderText` appears, its text (e.g. "loading") is announced politely via the `status` live region, without moving focus. *(Verification pending per the policy's AT matrix.)*
- A spinner rendered without `screenReaderText` announces nothing and is not discoverable.

### Application responsibilities

- Always pass a localized `screenReaderText` — it is optional in the API but required for an accessible result.
- Remove the spinner when loading completes; if the outcome isn't conveyed by the content that appears, provide a completion message (e.g. in a `role="status"` region).
- Avoid several simultaneous spinners for one operation — each is its own live region.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above; few target live regions directly:

- [Role attribute has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/674b10/) — `role="status"`

### Known issues

- `screenReaderText` is optional, and without it the spinner renders with no role and no text — invisible to screen reader users. Identified in the August 2026 accessibility audit.
