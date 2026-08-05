---
title: 'IconButton'
type: 'component'
components:
- IconButton
- IconButtonWithTooltip
categories:
- Buttonlike
status: 'New'
designStatus: 'Done'
devStatus: 'Done'
notes: ''
---

## Basic Usage with Paragon Icon

```jsx live
() => {
  const variants = ["brand", "primary", "secondary", "success", "warning", "danger", "light", "dark", "black"];
  return (
    <div className="d-flex flex-wrap">
      {variants.map((variant) => (
        <IconButton key={variant} src={Close} alt="Close" onClick={() => {}} variant={variant} className="mr-2" />
      ))}
    </div>
  );
}
```

## With tooltips

```jsx live
() => {
  const variants = ["brand", "primary", "secondary", "success", "warning", "danger", "light", "dark", "black"];
  return (
    <div className="d-flex">
      {variants.map((variant) => (
        <IconButtonWithTooltip
          key={variant}
          tooltipPlacement='top'
          tooltipContent={<div>a nice tooltip of {variant}!</div>}
          src={Close}
          alt="Close"
          onClick={() => {}}
          variant={variant}
          className="mr-2"
        />
      ))}
    </div>
  );
}
```

## Active State

```jsx live
() => {
  const variants = ["brand", "primary", "secondary", "success", "warning", "danger", "light", "dark", "black"];
  return (
    <div className="d-flex">
      {variants.map((variant) => (
        <IconButton 
          isActive
          key={variant}
          src={Close}
          alt="Close"
          onClick={() => {}}
          variant={variant}
          className="mr-2 mb-2"
        />
      ))}
    </div>
  );
}
```

### isActive=true with inverted colors

```jsx live
() => {
  const variants = ["brand", "primary", "secondary", "success", "warning", "danger", "light", "dark", "black"];
  return (
    <div className="d-flex">
      {variants.map((variant) => (
        <IconButton 
          invertColors
          isActive
          key={variant}
          src={Close}
          alt="Close"
          onClick={() => {}}
          variant={variant}
          className="mr-2"
        />
      ))}
    </div>
  );
}
```


## Inverted Colors

```jsx live
<div className="d-flex">
  <div className="p-1 bg-brand">
    <IconButton
      src={MenuIcon}
      alt="Menu"
      onClick={() => console.log("You clicked the menu button")}
      variant="brand"
      invertColors
    />
  </div>
  <div className="p-1 bg-primary">
    <IconButton
      src={MenuIcon}
      alt="Menu"
      onClick={() => console.log("You clicked the menu button")}
      variant="primary"
      invertColors
    />
  </div>
  <div className="p-1 bg-secondary">
    <IconButton
      src={MenuIcon}
      alt="Menu"
      onClick={() => console.log("You clicked the menu button")}
      variant="secondary"
      invertColors
    />
  </div>
  <div className="p-1 bg-success">
    <IconButton
      src={MenuIcon}
      alt="Menu"
      onClick={() => console.log("You clicked the menu button")}
      variant="success"
      invertColors
    />
  </div>
  <div className="p-1 bg-warning">
    <IconButton
      src={MenuIcon}
      alt="Menu"
      onClick={() => console.log("You clicked the menu button")}
      variant="warning"
      invertColors
    />
  </div>
  <div className="p-1 bg-danger">
    <IconButton
      src={MenuIcon}
      alt="Menu"
      onClick={() => console.log("You clicked the menu button")}
      variant="danger"
      invertColors
    />
  </div>
  <div className="p-1 bg-light">
    <IconButton
      src={MenuIcon}
      alt="Menu"
      onClick={() => console.log("You clicked the menu button")}
      variant="light"
      invertColors
    />
  </div>
  <div className="p-1" style={{ background: "black" }}>
    <IconButton
      src={MenuIcon}
      alt="Menu"
      onClick={() => console.log("You clicked the menu button")}
      variant="black"
      invertColors
    />
  </div>
</div>
```

## Sizes

```jsx live
<>
  <div className="mb-1">
    Small
    <IconButton
      src={MenuIcon}
      alt="Menu"
      onClick={() => {}}
      variant="primary"
      size="sm"
    />
  </div>
  <div className="mb-1">
    Inline:
    <IconButton
      src={MenuIcon}
      alt="Menu"
      onClick={() => {}}
      variant="primary"
      size="inline"
    />
  </div>
  <div className="x-small mb-1">
    An <strong>inline</strong> Icon Button inherits font size!
    For example, applying className="x-small" will make the Icon Button look like this:
    <IconButton
      src={Favorite}
      alt="Favorite"
      onClick={() => {}}
      variant="primary"
      size="inline"
    />
    . The Icon Button will also wrap with the text as long as it is not a direct child of a flex box.
  </div>
</>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`IconButton` renders a native `<button type="button">` whose accessible name comes entirely from the required `alt` prop (rendered as `aria-label`) — the [APG button pattern](https://www.w3.org/WAI/ARIA/apg/patterns/button/) via native semantics. The icon itself is decorative (`aria-hidden` svg). `IconButtonWithTooltip` wraps the same button in an `OverlayTrigger`/`Tooltip`.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Activates the button (native `<button>` behavior). |

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Button | `<button type="button">` | |
| Button | `aria-label` = `alt` | The only accessible name; verified by unit test. |
| Icon `<svg>` | `aria-hidden="true"`, `focusable="false"` | Decorative; rendered via `Icon`. |
| Tooltip (`IconButtonWithTooltip`) | `id="iconbutton-tooltip-<placement>"` | The id is derived from `tooltipPlacement` only, so multiple instances on a page share the same id — see Known issues. |

### Screen reader expectations

- Announces the `alt` text and "button" — e.g. "Close, button". *(Verification pending per the policy's AT matrix.)*
- The tooltip content of `IconButtonWithTooltip` is visual reinforcement; the announced name is still `alt`.

### Application responsibilities

- Write `alt` as the button's action ("Close", "Edit settings"), not a description of the glyph; localize it.
- Keep `tooltipContent` consistent with `alt` so hover/focus users and screen reader users get the same message.
- `invertColors` only swaps the visual palette for dark backgrounds — verify contrast of the chosen `variant` against your background.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — `aria-label` from `alt`
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/)
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

- `IconButtonWithTooltip` builds the tooltip `id` from `tooltipPlacement` alone (`iconbutton-tooltip-top` by default), so two or more tooltipped icon buttons with the same placement render duplicate ids on one page, breaking the id's uniqueness and any programmatic association that relies on it (WCAG 1.3.1) — identified in the August 2026 accessibility audit.
