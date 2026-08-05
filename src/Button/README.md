---
title: 'Button'
type: 'component'
components:
- Button
categories:
- Buttonlike
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
---

This component utilizes `Button` from React-Bootstrap and extends it with an ability to add icons before and after button label, see [below](#with-icons-before-or-after) for usage example.<br/> <a href="https://react-bootstrap-v4.netlify.app/components/buttons/" target="_blank" rel="noopener noreferrer"> See React-Bootstrap for additional documentation. </a>

## Core Buttons

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.medium.maxWidth });

  return (
    <Stack gap={2} direction={ isExtraSmall ? "vertical" : "horizontal" }>
      <Button variant="brand">Brand</Button>
      <Button variant="outline-brand">Outline Brand</Button>
      <Button variant="primary">Primary</Button>
      <Button variant="outline-primary">Outline Primary</Button>
      <Button variant="tertiary">Tertiary</Button>
    </Stack>
)}
```

## Core Buttons (Inverse Pallete)

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.medium.maxWidth });

  return (
    <Stack
      className="bg-dark-700 p-4"
      gap={2}
      direction={ isExtraSmall ? "vertical" : "horizontal" }
    >
      <Button variant="inverse-brand">Brand</Button>
      <Button variant="inverse-outline-brand">Outline Brand</Button>
      <Button variant="inverse-primary">Primary</Button>
      <Button variant="inverse-outline-primary">Outline Primary</Button>
      <Button variant="inverse-tertiary">Tertiary</Button>
    </Stack>
)}
```

## Utility Buttons

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <>
      <Stack
        className="mb-2"
        gap={2}
        direction={ isExtraSmall ? "vertical" : "horizontal" }
      >
        <Button variant="success">Success</Button>
        <Button variant="danger">Danger</Button>
        <Button variant="outline-success">Success</Button>
        <Button variant="outline-danger">Danger</Button>
      </Stack>
      <Stack
        gap={2}
        direction={ isExtraSmall ? "vertical" : "horizontal" }
      >
        <Button variant="link">Link</Button>
        <Button variant="light">Light</Button>
        <Button variant="dark">Dark</Button>
        <Button variant="outline-light">Light</Button>
        <Button variant="outline-dark">Dark</Button>
      </Stack>
    </>
)}
```

## Size

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <>
      <Stack
        className="mb-2"
        gap={2}
        direction={ isExtraSmall ? "vertical" : "horizontal" }
      >
        <Button variant="primary" size="lg">
          Large button
        </Button>
        <Button variant="outline-primary" size="lg">
          Large button
        </Button>
      </Stack>
      <Stack
        className="mb-2"
        gap={2}
        direction={ isExtraSmall ? "vertical" : "horizontal" }
      >
        <Button variant="primary" size="sm">
          Small button
        </Button>
        <Button variant="outline-primary" size="sm">
          Small button
        </Button>
      </Stack>
      <Stack
        className="mb-2"
        gap={2}
        direction={ isExtraSmall ? "vertical" : "horizontal" }
      >
        <Button variant="link" size="inline">Inline button</Button>
        <Button variant="link" size="inline">Inline button</Button>
      </Stack>
    </>
  )
}
```

### When to use the inline size

Use inline size buttons for when a button sits with a line of text.

```jsx live
<p>
  <span className="mr-1">2 items selected.</span>
  <Button variant="link" size="inline" className="mr-1">Select all</Button>
  <Button variant="link" size="inline">Clear</Button>
</p>
```

## Block Buttons

```jsx live
<>
  <Button variant="primary" size="lg" block>
    Block level button
  </Button>
  <Button variant="secondary" size="lg" block>
    Block level button
  </Button>
</>
```

### Disabled

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <Stack
      className="mb-2"
      gap={2}
      direction={ isExtraSmall ? "vertical" : "horizontal" }
    >
      <Button variant="primary" disabled>Primary disabled</Button>
      <Button variant="secondary" disabled>Secondary disabled</Button>
      <Button as="a" href="https://edx.org" disabled>Link disabled</Button>
    </Stack>
    )
}
```

### With empty href
For link to be `disabled`, it must have href defined with some value.

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <Stack
      className="mb-2"
      gap={2}
      direction={ isExtraSmall ? "vertical" : "horizontal" }
    >
      <Button as="a" disabled>No href</Button>
      <Button as="a" href="" disabled>Empty string href</Button>
    </Stack>
  )
}
```

### With Icons before or after

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.large.maxWidth });

  return (
    <Stack
      className="mb-2"
      gap={2}
      direction={ isExtraSmall ? "vertical" : "horizontal" }
    >
      <Button variant="brand" iconBefore={ArrowBack}>
        Brand
      </Button>
      <Button variant="outline-brand" iconAfter={ArrowDropDown}>
        Outline Brand
      </Button>
      <Button variant="primary" iconBefore={Remove} iconAfter={Add}>
        Primary
      </Button>
      <Button variant="outline-primary" iconBefore={Highlight}>
        Outline Primary
      </Button>
      <Button variant="tertiary" iconAfter={Add}>
        Tertiary
      </Button>
    </Stack>
  )
}
```

## Stateful buttons
To implement loading state using a `Button` component, the [StatefulButton](https://paragon-openedx.netlify.app/components/statefulbutton/) component
is available for use. <br/>
This specialized component is designed to seamlessly manage and display boot states, providing a more efficient and 
user-friendly experience.

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Paragon's `Button` wraps [React-Bootstrap's Button](https://react-bootstrap-v4.netlify.app/components/buttons/) and follows the [APG button pattern](https://www.w3.org/WAI/ARIA/apg/patterns/button/) by rendering a native `<button>`. When `href` is set it instead renders an `<a>` element styled as a button — link semantics, not button semantics.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Activates the button (native `<button>` behavior). |
| <kbd>Enter</kbd> | On the anchor rendering (`href` set): follows the link. |
| <kbd>Space</kbd> | On the anchor rendering: intercepted by React-Bootstrap's `SafeAnchor` — page scroll is prevented and your `onClick` handler fires; native link navigation does not occur. |

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Default rendering | `<button type="button">` | Provided by React-Bootstrap; `type` defaults to `button`, so it never submits a form unless you pass `type="submit"`. |
| With `href` | `<a href="...">` | Provided by React-Bootstrap (`SafeAnchor`); native link role. |
| With a trivial `href` (empty or `"#"`) | `role="button"`, `href="#"` | `SafeAnchor` assigns `role="button"` and suppresses navigation. |
| `disabled`, button rendering | `disabled` | Native attribute; the button leaves the tab order. |
| `disabled`, anchor rendering | `aria-disabled="true"`, `tabindex="-1"` | Provided by `SafeAnchor`; clicks are prevented and the link leaves the tab order. |
| `iconBefore` / `iconAfter` | `aria-hidden="true"` on the `<svg>` | Icons render through `Icon` with no label — decorative. |

### Screen reader expectations

- Announces its text content and role — e.g. "Save, button". *(Verification pending per the policy's AT matrix.)*
- With `href`, announces as a link, not a button — users will expect navigation.
- Icons are silent; the accessible name comes solely from `children`.

### Application responsibilities

- Provide discernible, localized text as `children`. For icon-only actions use [IconButton](/components/iconbutton) instead.
- Use `href` for navigation and the default button for actions, so the announced role matches the behavior.
- Don't rely on `variant` color alone to convey meaning (e.g. destructive actions) — the label should carry it.
- The `as` prop replaces the element entirely; if you pass a custom component, you own its semantics and keyboard support.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — the default rendering
- [Link has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/) — the `href` rendering
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — label text across `variant`s
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

None identified in the August 2026 accessibility audit.
