---
title: 'Tooltip'
type: 'component'
components:
- Tooltip
categories:
- Overlays
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

`Tooltips` display informative text when users hover over, focus on, or tap an element.

## Basic Usage

```jsx live
<>
  {['top', 'right', 'bottom', 'left'].map((placement) => (
    <OverlayTrigger
      key={placement}
      placement={placement}
      overlay={
        <Tooltip id={`tooltip-${placement}`}>
          Tooltip on <strong>{placement}</strong>.
        </Tooltip>
      }
    >
      <Button variant="primary" className="mr-2 mb-2">Tooltip on {placement}</Button>
    </OverlayTrigger>
  ))}
</>
```

## Light version

```jsx live
<div className="bg-dark-700 p-4">
  {['top', 'right', 'bottom', 'left'].map((placement) => (
    <OverlayTrigger
      key={placement}
      placement={placement}
      overlay={
        <Tooltip variant="light" id={`tooltip-${placement}`}>
          Tooltip on <strong>{placement}</strong>.
        </Tooltip>
      }
    >
      <Button variant="inverse-outline-brand" className="mr-2 mb-2">Tooltip on {placement}</Button>
    </OverlayTrigger>
  ))}
</div>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Paragon's `Tooltip` wraps [React-Bootstrap's Tooltip](https://react-bootstrap-v4.netlify.app/components/overlays/#tooltips) and implements the [APG tooltip pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tooltip/) with deviations noted below. Show/hide behavior comes from the `OverlayTrigger` component that positions it (default trigger: `['hover', 'focus']`).

### Keyboard support

Provided by React-Bootstrap's `OverlayTrigger`:

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> | Moving focus to the trigger shows the tooltip; moving focus away hides it. Focus never moves into the tooltip. |
| <kbd>Escape</kbd> | **Does not dismiss the tooltip** — deviation from the APG tooltip pattern; see Known issues. |

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Tooltip root (`.tooltip`) | `role="tooltip"` | Provided by React-Bootstrap. |
| Tooltip root | `id` (required prop) | Required "for accessibility": it is what the automatic `aria-describedby` association points at. |
| Trigger element | `aria-describedby` = tooltip `id` | Added automatically only while the tooltip is shown (react-overlays `ariaDescribedBy` popper modifier); removed on hide. |

**Deviation from the APG tooltip pattern** (identified in the August 2026 accessibility audit): the pattern requires <kbd>Escape</kbd> to dismiss the tooltip, and WCAG 1.4.13 additionally requires hover-triggered content to be hoverable. Neither is satisfied — see Known issues.

### Screen reader expectations

- When the trigger receives focus, the tooltip text is exposed as the trigger's accessible description and is typically read after the trigger's name and role — e.g. "Save, button, Tooltip on top". *(Verification pending per the policy's AT matrix.)*
- The tooltip itself is not focused and triggers no separate announcement on open. *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Always supply a unique `id` — without it the trigger gets no `aria-describedby` association.
- Keep tooltip text brief, supplementary, and non-interactive; a tooltip must never be the only place essential information appears, given the 1.4.13 gaps below.
- Use a natively focusable trigger (normally a `Button`) so keyboard users can reach the tooltip; never pass `trigger="hover"` alone (React-Bootstrap logs a console warning).
- Do not put links, buttons, or other interactive content inside a tooltip — focus cannot reach it. Use `Popover` with a click trigger or `ModalPopup` for richer content.
- If a control's tooltip is its only label (e.g. an icon button), give the control its own accessible name as well — a description does not replace a name.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Role attribute has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/674b10/) — `role="tooltip"`
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — the injected `aria-describedby` reference
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/) — the trigger element
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — tooltip text, including the `light` variant

### Known issues

Identified in the August 2026 accessibility audit; both are inherited from React-Bootstrap 1.6.5 (see also the Overlay component's accessibility notes):

- <kbd>Escape</kbd> does not dismiss the tooltip, so content shown on hover or focus cannot be dismissed without moving the pointer or focus — fails the *dismissible* requirement of WCAG 1.4.13 and deviates from the APG tooltip pattern.
- The tooltip hides on mouse-out of the trigger and cannot itself be hovered — fails the *hoverable* requirement of WCAG 1.4.13.
