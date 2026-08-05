---
title: 'Chip'
type: 'component'
components:
- Chip
categories:
- Buttonlike
status: 'New'
designStatus: 'Done'
devStatus: 'Done'
notes: |
---

``Chips`` are compact elements that represent an input, attribute, or action. Similar to the [Badge](/components/badge) component, but interactive.

## Basic Usage

```jsx live
<Stack
  gap={2}
  direction="horizontal"
>
  <Chip>New</Chip>
  <Chip disabled>New</Chip>
</Stack>
```

## Clickable Variant

Use `onClick` prop to make the whole `Chip` clickable, this will also add appropriate styles to make `Chip` interactive.

```jsx live
<Chip onClick={() => console.log('Click!')}>Click Me</Chip>
```

## With isSelected prop

```jsx live
<Chip isSelected>New</Chip>
```

## With Icon Before and After
### Basic Usage

Use `iconBefore` and `iconAfter` props to provide icons for the `Chip`, note that you also can provide
accessible names for these icons for screen reader support via `iconBeforeAlt` and `iconAfterAlt` respectively. 

```jsx live
<Stack
  gap={2}
  direction="horizontal"
>
  <Chip iconBefore={Person} iconBeforeAlt="icon-before">Person</Chip>
  <Chip iconAfter={Close} iconAfterAlt="icon-after">Close</Chip>
  <Chip
    iconBefore={Person}
    iconAfter={Close}
    iconAfterAlt="icon-after"
    iconBeforeAlt="icon-before"
  >
    Both
  </Chip>
</Stack>
```

### Clickable icon variant

Provide click handlers for icons via `onIconAfterClick` and `onIconBeforeClick` props. 

```jsx live
<Stack
  gap={2}
  direction="horizontal"
>
  <Chip
    iconBefore={Person}
    iconBeforeAlt="icon-before"
    onIconBeforeClick={() => console.log('onIconBeforeClick')}
  >
    Person
  </Chip>
  <Chip
    iconAfter={Close}
    onIconAfterClick={() => console.log('onIconAfterClick')}
    iconAfterAlt="icon-after"
  >
    Close
  </Chip>
  <Chip
    iconBefore={Person}
    iconAfter={Close}
    onIconAfterClick={() => console.log('onIconAfterClick')}
    onIconBeforeClick={() => console.log('onIconBeforeClick')}
    iconAfterAlt="icon-after"
    iconBeforeAlt="icon-before"
  >
    Both
  </Chip>
  <Chip
    iconBefore={Person}
    iconAfter={Close}
    onIconAfterClick={() => console.log('onIconAfterClick')}
    onIconBeforeClick={() => console.log('onIconBeforeClick')}
    iconAfterAlt="icon-after"
    iconBeforeAlt="icon-before"
    disabled
  >
    Both
  </Chip>
</Stack>
```

**Note**: both `Chip` and its icons cannot be made interactive at the same time, e.g. if you provide both `onClick` and `onIconAfterClick` props,
`onClick` will be ignored and only the icon will get interactive behaviour, see example below (this is done to avoid usability issues where users might click on the `Chip` itself by mistake when they meant to click the icon instead).

```jsx live
<Chip
  iconBefore={Person}
  iconBeforeAlt="icon-before"
  onIconBeforeClick={() => console.log('onIconBeforeClick')}
  onClick={() => console.log('onClick')}
>
  Person
</Chip>
```

### Inverse Pallete

```jsx live
<Stack
  className="bg-dark-700 p-4"
  gap={2}
  direction="horizontal"
>
  <Chip variant="dark" iconBefore={Person} iconBeforeAlt="icon-before">New</Chip>
  <Chip
    variant="dark"
    iconAfter={Close}
    onIconAfterClick={() => console.log('onIconAfterClick')}
    iconAfterAlt="icon-after"
  >
    New 1
  </Chip>
  <Chip
    variant="dark"
    iconAfter={Close}
    onIconAfterClick={() => console.log('onIconAfterClick')}
    iconAfterAlt="icon-after"
    disabled
  >
    New
  </Chip>
</Stack>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Chip` has three renderings. With no click handlers it is a static `<div>` with no ARIA. With `onClick` (and no icon click handlers) the whole chip becomes a div with `role="button"` and `tabIndex="0"`, approximating the [APG button pattern](https://www.w3.org/WAI/ARIA/apg/patterns/button/) — with keyboard deviations noted below. With `onIconBeforeClick`/`onIconAfterClick`, the chip body stays static and each clickable icon renders as an [IconButton](/components/iconbutton) (a real `<button>` named by `iconBeforeAlt`/`iconAfterAlt`); `onClick` is ignored in this mode.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> | Moves focus to the interactive chip, or to each clickable icon button. |
| <kbd>Enter</kbd> | Activates the interactive chip or a focused icon button. |
| <kbd>Space</kbd> | Activates a focused icon button (native). On the interactive chip, activation is wired to the deprecated `keypress` event without key filtering — see the deviation below. |

**Deviation from the APG pattern (identified in the August 2026 accessibility audit):** the interactive chip binds its `onClick` directly to `onKeyPress` with no key check and no `preventDefault`, so activation is not limited to <kbd>Enter</kbd> and <kbd>Space</kbd> — any key that produces a `keypress` event (including printable characters) invokes the handler, and <kbd>Space</kbd>'s default page scroll is not suppressed.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Chip with `onClick` | `role="button"`, `tabindex="0"` | Only when no icon click handlers are set. |
| Chip, static | *(none)* | Plain `<div>`; decorative icons render as `aria-hidden` svgs. |
| Clickable icon | `<button>` with `aria-label` = `iconBeforeAlt`/`iconAfterAlt` | Via `IconButton`. |
| `isSelected` | *(none)* | Renders only the `selected` CSS class — the state is not exposed to assistive technology (see Known issues). |
| `disabled` | *(none on the chip)* | Renders only the `disabled` CSS class; clickable icons get `tabindex="-1"` but no `disabled`/`aria-disabled` (see Known issues). |

### Screen reader expectations

- An interactive chip announces its content and "button" — e.g. "New, button". *(Verification pending per the policy's AT matrix.)*
- Selection and disabled states are **not** announced (no ARIA state is rendered).
- Clickable icons announce their `alt` text and "button".

### Application responsibilities

- Always provide `iconBeforeAlt`/`iconAfterAlt` alongside the corresponding click handler — it is the icon button's only accessible name; describe the action (e.g. "Remove New filter"), not the glyph.
- Icon alt texts must distinguish the chip they belong to when several chips are shown together.
- Do not combine `onClick` with icon click handlers — the chip-level handler is silently ignored.
- Because selection is only conveyed visually, mirror `isSelected` in visible text or manage state outside the chip until the gap below is fixed.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — the interactive chip and clickable icons
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — label across `light`/`dark` variants and the reduced-opacity disabled state
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

Identified in the August 2026 accessibility audit:

- Keyboard activation of the interactive chip uses the deprecated `keypress` event with no key filtering: any printable key activates the chip, and <kbd>Space</kbd> is not prevented from scrolling the page (deviation from the APG button pattern).
- `isSelected` is visual-only (CSS outline/class); no `aria-pressed` or equivalent state is rendered, so selection is not programmatically determinable (WCAG 4.1.2).
- `disabled` is visual-only on the chip (reduced opacity): an interactive chip keeps `tabindex="0"` and a live `onClick`, so it remains fully operable while appearing disabled; clickable icons are removed from the tab order but keep their click handlers and set no `aria-disabled` (WCAG 4.1.2).
