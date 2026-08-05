---
title: 'Badge'
type: 'component'
components:
- Badge
categories:
- Status & metadata
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

Badges are composed of text and an accompanying indicator color, and are typically used to add meaning, status, or context when paired with another component.

- Use badges paired with another component to convey status, or provide additional details
- Don’t use badges for freestanding text on a page

<p>
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/badge/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for additional documentation.
  </a>
</p>

## Basic Usage

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.small.maxWidth });

  return (
    <Stack gap={2} direction={ isExtraSmall ? "vertical" : "horizontal" }>
      <Badge variant="primary">Primary</Badge>
      <Badge variant="secondary">Secondary</Badge>
      <Badge variant="success">Success</Badge>
      <Badge variant="danger">Danger</Badge>
      <Badge variant="warning">Warning</Badge>
      <Badge variant="info">Info</Badge>
      <Badge variant="light">Light</Badge>
      <Badge variant="dark">Dark</Badge>
    </Stack>
)}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Badge` is a purely presentational text label; no [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) applies. It renders static content and has no keyboard interaction of its own.

### ARIA roles, states, and properties

No ARIA attributes are rendered. React-Bootstrap's Badge is a plain styled `<span>` (the `as` prop changes the element), so the badge is exposed as ordinary inline text with no role or boundary.

### Screen reader expectations

- The badge's text is read as part of the surrounding content; that it is a badge, and its color variant, are not conveyed. *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Don't rely on the variant color to carry meaning (WCAG 1.4.1 Use of Color): "danger" red and "success" green read identically to a screen reader — the badge text itself must convey the status.
- If the bare text is ambiguous, add visually hidden context (e.g. `<Badge variant="danger">3<span className="sr-only"> failed checks</span></Badge>`).
- Keep the badge adjacent in DOM order to the content it annotates, so the association survives without the visual pairing.
- Badges are not interactive — don't attach click handlers; use a `Button` or `Chip` for actions.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — badge text against each variant background
