---
title: 'Toast'
type: 'component'
components:
- Toast
categories:
- Overlays
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: ''
---

``Toast`` is a pop-up style message that shows the user a brief, fleeting, dismissible message about a successful app process.

``Toasts`` sit fixed to the bottom left of the window.

## Behaviors

<ul>
  <li>Auto-dismiss: Toast automatically dismisses after 5 seconds by default.</li>
  <li>Disable timer: On hover of the Toast container. On hover or focus of dismiss icon or optional button</li>
  <li>Re-enable timer: On mouse leave of the Toast container. On blur of dismiss icon or option button</li>
  <li>Auto-dismiss timer: 5 - 15 second range.</li>
</ul>

## Basic Usage

```jsx live
() => {
  const [show, setShow] = useState(false);

  return (
    <>
      <Toast
        onClose={() => setShow(false)}
        show={show}
      >
        Example of a basic Toast.
      </Toast>

      <Button onClick={() => setShow(true)}>Show Toast</Button>
    </>
  );
}
```

## With Button

```jsx live
() => {
  const [show, setShow] = useState(false);

  return (
    <>
      <Toast
        action={{
          label: "Optional Button",
          onClick: () => console.log('You clicked the action button.')
        }}
        onClose={() => setShow(false)}
        show={show}
      >
        Success! Example of a Toast with a button.
      </Toast>

      <Button onClick={() => setShow(true)}>Show Toast</Button>
    </>
  );
}
```

## With Link

```jsx live
() => {
  const [show, setShow] = useState(false);

  return (
    <>
      <Toast
        action={{
          label: "Optional Link",
          href: "#"
        }}
        onClose={() => setShow(false)}
        show={show}
      >
        Success! Example of a Toast with a link.
      </Toast>

      <Button onClick={() => setShow(true)}>Show Toast</Button>
    </>
  );
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Toast` wraps [React-Bootstrap's Toast](https://react-bootstrap-v4.netlify.app/components/toasts/) and renders into a portal (`#toast-root`) fixed to the bottom left of the window. There is no APG pattern for toasts; the rendered element is an ARIA live region (`role="alert"`). Because it auto-dismisses, [WCAG 2.2.1 Timing Adjustable](https://www.w3.org/WAI/WCAG22/Understanding/timing-adjustable.html) applies — see below.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus to the dismiss button and the optional action inside the toast. Focus on any element inside the toast disables auto-dismiss; on blur the timer restarts with the full delay. |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Activates the dismiss button or the action (native button/link activation). |
| <kbd>Escape</kbd> | Not handled — the toast cannot be dismissed with <kbd>Escape</kbd>. |

Auto-dismiss defaults to 5 seconds (`delay={5000}`). Hovering anywhere over the toast also disables the timer; it restarts on mouse-out. This pause behavior is implemented in Paragon's `Toast` (`onFocus`/`onBlur`/`onMouseOver`/`onMouseOut` toggling `autohide`).

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Toast container (`.toast`) | `role="alert"`, `aria-live="assertive"`, `aria-atomic="true"` | Provided by React-Bootstrap; asserted in `Toast.test.tsx`. See the deviation note below. |
| Dismiss icon button | `<button>` with `aria-label` = `closeLabel` prop | Defaults to a localized "Close" (Paragon `IconButton`). |
| Action with `onClick` | `<button>` | Accessible name is `action.label`. |
| Action with `href` | anchor exposed with `role="button"` | Queried by role `button` in `Toast.test.tsx`; announced as a button but navigates like a link. |

**Deviation identified in the August 2026 accessibility audit:** `role="alert"` with `aria-live="assertive"` interrupts whatever the screen reader is currently speaking, but `Toast` is documented for brief messages about *successful* app processes — non-urgent content for which a polite live region (`role="status"`) would be appropriate. Inherited from React-Bootstrap 1.6.5.

### Screen reader expectations

- When `show` becomes true, the toast's full content — the message plus the action's label, per `aria-atomic="true"` — is expected to be announced immediately, interrupting current speech. *(Verification pending per the policy's AT matrix.)*
- The dismiss button announces as "Close, button" (or your `closeLabel`). *(Verification pending per the policy's AT matrix.)*
- The announcement does not move focus; reaching the toast's controls requires navigating to them.

### Application responsibilities

- Keep messages brief and self-contained (`children` is a string); the toast disappears by default after 5 seconds.
- Never make a toast the only path to essential information or an essential action — it auto-dismisses and is easy to miss. Repeat the action in persistent UI.
- If you include an `action`, consider a longer `delay` (the design guidance above allows 5–15 seconds) so keyboard users can reach it before dismissal.
- Provide a localized `closeLabel` when your app is not using Paragon's bundled translations.
- Don't stack critical workflows on toasts; use an `Alert` (persistent) for information users must act on.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — the dismiss button and optional action
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-live`, `aria-atomic`
- [Text content that changes automatically can be paused, stopped or hidden](https://www.w3.org/WAI/standards-guidelines/act/rules/efbfc7/) — the auto-dismiss timer's pause-on-hover/focus behavior
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/) — dismiss and action controls
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — toast text on the dark background

### Known issues

Identified in the August 2026 accessibility audit:

- The assertive `role="alert"` live region is used for non-urgent success messages, interrupting current screen reader output (inherited from React-Bootstrap 1.6.5); a polite `role="status"` region would match the component's documented purpose.
- The default 5-second auto-dismiss also removes the optional action button; the pause mechanism requires the user to hover or focus the toast before the timer expires, which keyboard and screen reader users may not manage — a WCAG 2.2.1 concern when an `action` is present.
