---
title: 'ActionRow'
type: 'component'
components:
- ActionRow
- ActionRowSpacer
categories:
- Buttonlike
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

A layout utility for the common use case of aligning buttons, links or text
in a row in a control bar or nav.

ActionRow assumes that its last child is the primary action and lays out actions so that the last item is in a primary location. If horizontal, the primary action sits on the right. If stacked, the primary action sits at the top of the stack (this is done via `flex-direction: column-reverse;`).

## Basic Usage

```jsx live
<ActionRow>
  <Button variant="tertiary">
    Cancel
  </Button>
  <Button variant="primary">
    Submit
  </Button>
</ActionRow>
```

## Usage with a Spacer

ActionRow can also be used with a helper component ``ActionRow.Spacer`` to insert empty space between children

```jsx live
<ActionRow>
  <Form.Checkbox className="flex-column flex-sm-row">
      Don't ask me again.
  </Form.Checkbox>
  <ActionRow.Spacer />
  <Button variant="tertiary">
    Cancel
  </Button>
  <Button variant="primary">
    Submit
  </Button>
</ActionRow>
```

## Stacked Usage

```jsx live
<ActionRow isStacked>
  <p className="x-small">
    Bespoke leggings yuccie, portland umami readymade craft beer vaporware sriracha.
  </p>
  <Button variant="tertiary">
    Go back
  </Button>
  <Button variant="primary">
    Continue
  </Button>
</ActionRow>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`ActionRow` is a layout utility: it renders a plain `<div>` (or the `as` element) with flexbox styling and adds no ARIA attributes of its own. No APG pattern applies. `ActionRow.Spacer` renders an empty `<span>`.

ActionRow renders static layout and has no keyboard interaction of its own; children keep their own semantics, and focus moves through them in DOM order.

### ARIA roles, states, and properties

No ARIA attributes are rendered.

**Focus order in stacked mode (identified in the August 2026 accessibility audit):** `isStacked` reverses the *visual* order with `flex-direction: column-reverse`, so the primary action (the last child) appears at the top while remaining last in the DOM. Keyboard focus and screen reader reading order still follow DOM order, so a sighted keyboard user reaches the visually first control last — a WCAG 1.3.2 (Meaningful sequence) and 2.4.3 (Focus order) concern. An open fix, [openedx/paragon#4271](https://github.com/openedx/paragon/pull/4271), aligns DOM and visual order.

### Application responsibilities

- Pass the primary action as the last child — both layouts assume this.
- Until #4271 lands, weigh the stacked layout's reversed focus order when the row contains more than one focusable action.
- If you change `as`, keep it a semantically neutral container (or own the semantics of what you pass).

### Relevant ACT rules

No ACT rule tests layout and focus order directly; the stacked-mode concern above falls under WCAG 1.3.2 and 2.4.3, which are evaluated manually.

### Known issues

- Stacked mode's visual order does not match DOM/focus order (`flex-direction: column-reverse`) — identified in the August 2026 accessibility audit; open fix in [openedx/paragon#4271](https://github.com/openedx/paragon/pull/4271).
