---
title: 'Sheet'
type: 'component'
components:
- Sheet
categories:
- Overlays
status: 'New'
designStatus: 'In Progress'
devStatus: 'In Progress'
notes: |
  Window-level edge-anchored content container with elevation shadow, with option to block content.
---

## Basic Usage

```jsx live

() => {
  const [blocking, setBlocking] = useState(false);
  const [dark, setDark] = useState(false);
  const [position, setPosition] = useState('bottom');
  const [show, setShow] = useState(false);

  const positions = ['left', 'right', 'top', 'bottom'];

  return (
    <>
      <DropdownButton
        id="position-dropdown-btn"
        onSelect={setPosition}
        title="Sheet Position"
      >
        {positions.map(position => (
          <Dropdown.Item eventKey={position}>{position}</Dropdown.Item>
        ))}
      </DropdownButton><br />
      <Button onClick={() => setShow(true)} className="mb-2 mb-md-0">
        Show the Sheet
      </Button>{' '}
      <Button onClick={() => setBlocking(!blocking)} className="mb-2 mb-md-0">
        {blocking ? "Disable": "Enable"} blocking content
      </Button>{' '}
      <Button onClick={() => setDark(!dark)} className="mb-2 mb-md-0">
        Set {dark ? "Light": "Dark"} mode
      </Button>

      <Sheet
        position={position}
        show={show}
        blocking={blocking}
        variant={dark ? 'dark' : 'light'}
        onClose={() => setShow(false)}
      >
        This is a Sheet component <br />
        <Button
          onClick={() => setShow(false)}
          variant={dark ? 'inverse-primary' : 'primary'}
        >
          Hide Me!
        </Button>
      </Sheet>
    </>
  )
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Sheet` is a window-edge-anchored panel rendered into a portal (`#sheet-root`). It traps focus like a modal dialog (using [react-focus-on](https://github.com/theKashey/react-focus-on), the same library as `ModalLayer`), but it exposes **live region semantics (`role="alert"`), not dialog semantics** — a deviation from the [APG dialog (modal) pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/) it behaviorally resembles. See Known issues.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus through focusable elements inside the sheet. Focus is trapped: it cannot reach the page behind the sheet while it is open. |
| <kbd>Escape</kbd> | Closes the sheet (calls `onClose`). **Disabled when `blocking` is set.** |

Clicking outside the sheet also closes it unless `blocking` is set. Focus management (trap, moving focus in on open, returning it on close) is provided by react-focus-on.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Sheet (`.pgn__sheet-component`) | `role="alert"` | Asserted in the `Sheet.test.jsx` snapshots. See the deviation note below. |
| Sheet | `aria-live="polite"`, `aria-atomic="true"` | `polite` contradicts the assertive semantics implied by `role="alert"`. |
| Scrim (`.pgn__sheet-skrim`) | `role="presentation"` | Removed from the accessibility tree. |
| — | *(no `role="dialog"`, no accessible name, no `aria-modal`)* | Nothing identifies the sheet as a dialog or names it. |

**Deviation from the APG dialog pattern** (identified in the August 2026 accessibility audit): the sheet contains focusable content and traps focus, yet is exposed as an alert live region with no role name or accessible name, and with a conflicting politeness level. See Known issues.

### Screen reader expectations

- When the sheet appears its content may be announced via the live region, but a live region inserted into the DOM together with its content is not reliably announced by all screen readers. *(Verification pending per the policy's AT matrix.)*
- Because no dialog role or accessible name is exposed, users are not told they have entered a named dialog when focus moves into the sheet. *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Always render a visible, focusable close control inside the sheet. With `blocking`, <kbd>Escape</kbd> and outside-click dismissal are disabled, and without a close control keyboard users have no way out of the focus trap (WCAG 2.1.2).
- Start the sheet's content with a heading that states its purpose; the component provides no accessible name of its own.
- Control `show` deliberately — `show` defaults to `true`, so an unmanaged sheet renders (and traps focus) immediately on mount.
- Check text contrast when using `variant="dark"` with custom content.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Focusable element has no keyboard trap](https://www.w3.org/WAI/standards-guidelines/act/rules/80af7b/) — the trap must have a keyboard exit (<kbd>Escape</kbd> or a close control)
- [Role attribute has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/674b10/) — `role="alert"`, `role="presentation"`
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-live`, `aria-atomic`
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/) — controls inside the sheet
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — both `light` and `dark` variants

### Known issues

Identified in the August 2026 accessibility audit:

- The sheet traps focus like a modal but renders `role="alert"` with `aria-live="polite"` instead of `role="dialog"` with an accessible name: users are not informed they are in a dialog, the panel has no name, and the explicit `polite` value contradicts the assertive behavior implied by `role="alert"` (WCAG 4.1.2, 1.3.1).
- With `blocking` set, both <kbd>Escape</kbd> and outside-click dismissal are disabled while focus stays trapped; the component itself renders no close control, so keyboard escape depends entirely on the consumer providing one (WCAG 2.1.2 risk).
