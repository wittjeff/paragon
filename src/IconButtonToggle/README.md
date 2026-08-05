---
title: 'IconButtonToggle'
type: 'component'
components:
- IconButtonToggle
categories:
- Buttonlike
status: 'Stable'
designStatus: 'In Progress'
devStatus: 'Done'
notes: |
  Helps toggle Data view between list, card view etc.
---

Wraps a set of ``IconButton`` or ``IconButtonWithTooltip`` components in a group and marks as active only one at a time.
The IconButton with a value matching the 'activeValue' is set to be in the active state.
When one presses on one of the buttons, that button is set to active state, and the callback passed to ``onChange`` is called with that value.
You can pass a tooltip if you use the ``IconButtonWithTooltip`` subcomponent.

## Basic Usage
```jsx live
    () => {
      const [activeValue, setActiveValue] = React.useState('card');
      return (
        <>
        <div className="mr-2 mt-2 mb-2">Current value is: <strong>{activeValue}</strong></div>
        <IconButtonToggle activeValue={activeValue} onChange={ value => setActiveValue(value) }>
          <IconButton value="card" src={GridView} iconAs={Icon} alt="Card" />
          <IconButton value="list" src={ListView} iconAs={Icon} alt="List" />
        </IconButtonToggle>
        </>
      );
    }
```

## Basic Usage (with tooltips)
```jsx live
    () => {
      const [activeValue, setActiveValue] = React.useState('card');
      return (
        <>
        <div className="mr-2 mt-2 mb-2">Current value is: <strong>{activeValue}</strong></div>
        <IconButtonToggle activeValue={activeValue} onChange={ value => setActiveValue(value) }>
          <IconButtonWithTooltip tooltipContent="Card view" value="card" src={GridView} iconAs={Icon} alt="Card" />
          <IconButtonWithTooltip tooltipContent="List view" value="list" src={ListView} iconAs={Icon} alt="List" />
        </IconButtonToggle>
        </>
      );
    }
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`IconButtonToggle` renders its `IconButton` children inside a plain `<div>` and marks the active one with `aria-selected` — a single-select toggle group. The closest APG guidance is the [radio group pattern](https://www.w3.org/WAI/ARIA/apg/patterns/radio/) (or `aria-pressed` toggle buttons); the current markup follows neither — see the deviation below.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> | Moves focus to each button in the group; every button is its own tab stop. |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Selects the focused button (fires `onChange` with its `value`). |

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Container | *(none)* | Plain `<div>`; no `role`, no label. |
| Each child | `<button>` with `aria-label` = `alt` | Via `IconButton`. |
| Active child | `aria-selected="true"` | Set on every child (`false` when inactive) — see the deviation below. |

**Deviation (identified in the August 2026 accessibility audit):** `aria-selected` is not a permitted state on an element with a `button` role — it is valid only on roles such as `option`, `tab`, or `row`. The toggle-button pattern calls for `aria-pressed` (or a `radiogroup` structure), so the current markup exposes the selection state through an attribute assistive technology is not required to honor on buttons.

### Screen reader expectations

- Each button announces its `alt` text and "button". Whether the selected state is announced is unreliable given the impermissible `aria-selected` — treat it as unannounced. *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Give every child a distinct, action-describing `alt` (it is each button's only accessible name).
- Label the group's purpose in nearby visible text or your own wrapper (the container renders no label).
- Because selection may not be announced, reflect the active choice elsewhere in the UI (e.g. the view itself visibly changing) rather than relying on the toggle state alone.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [ARIA state or property is permitted](https://www.w3.org/WAI/standards-guidelines/act/rules/5c01ea/) — `aria-selected` on a native button fails this rule
- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/)
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

Identified in the August 2026 accessibility audit:

- The selected state is exposed with `aria-selected` on native buttons, which is not a permitted role/state pairing; `aria-pressed` (or a `radiogroup`/`radio` structure) is required for the state to be reliably announced (WCAG 4.1.2).
- The container is an unlabeled `<div>` with no group role, so the buttons are not programmatically related as a single-select group (WCAG 1.3.1).
