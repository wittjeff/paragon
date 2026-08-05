---
title: 'Overlay'
type: 'component'
components:
- Overlay
- OverlayTrigger
categories:
- Overlays
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

A set of components for positioning beautiful overlays, tooltips, popovers, and anything else you need.

This component is used to power Tooltips and Popovers.

<p>
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/overlays/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for additional documentation.
  </a>
</p>

## Basic usage

```jsx live
() => {
  const [isOpen, open, close] = useToggle(false);
  const target = React.useRef(null);

  return (
    <>
      <div className="d-flex">
        <Button variant="danger" ref={target} onClick={() => open(!isOpen)}>
          Click me to see
        </Button>
      </div>
      <Overlay target={target.current} show={isOpen} placement="right">
        {({ placement, arrowProps, show: _show, popper, ...props }) => (
          <div
            {...props}
            style={{
              backgroundColor: 'rgba(255, 100, 100, 0.85)',
              padding: '2px 10px',
              color: 'white',
              borderRadius: 3,
              ...props.style,
            }}
          >
            Simple tooltip
          </div>
        )}
      </Overlay>
    </>
  );
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Overlay` and `OverlayTrigger` are positioning primitives (they power `Tooltip` and `Popover`) and have no APG pattern of their own. When they show content on hover or focus, [WCAG 1.4.13 Content on Hover or Focus](https://www.w3.org/WAI/WCAG22/Understanding/content-on-hover-or-focus.html) applies, and for the common tooltip use the [APG tooltip pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tooltip/) applies. Behavior below is provided by React-Bootstrap 1.6.5 (and its react-overlays dependency) unless noted.

### Keyboard support

Interaction depends on the `trigger` prop of `OverlayTrigger` (default `['hover', 'focus']`):

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> | Moving focus to the trigger element shows the overlay (`focus` trigger); moving focus away hides it. Focus never moves into the overlay itself. |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | With the `click` trigger on a native button: toggles the overlay (native button activation fires the injected `onClick`). |
| <kbd>Escape</kbd> | **Not handled by default.** Only when `rootClose` is set on `Overlay` does <kbd>Escape</kbd> (or a click outside) call `onHide` — provided by react-overlays' `useRootClose`. `OverlayTrigger` does not set `rootClose`. |

### ARIA roles, states, and properties

`Overlay` renders no element of its own — it positions, shows, and hides the overlay element you pass it, so what lands in the accessibility tree is decided by that element (see `Tooltip` and `Popover`).

| Element | Attribute | Notes |
| --- | --- | --- |
| Trigger element | `aria-describedby` = overlay `id` | Added automatically **only while the overlay is shown**, and only when the overlay element has an `id` and `role="tooltip"`. Provided by react-overlays' `ariaDescribedBy` popper modifier; removed on hide. |
| Trigger element | *(no state)* | `OverlayTrigger` injects event handlers only — no `aria-expanded`, no `aria-haspopup`. |

**Deviation from WCAG 1.4.13** (identified in the August 2026 accessibility audit): hover/focus-triggered overlays are neither *dismissible* (no <kbd>Escape</kbd> handling by default) nor *hoverable* (the mouse-out handler sits on the trigger only, so moving the pointer onto the overlay content hides it). They are *persistent* while the pointer or focus remains on the trigger. Inherited from React-Bootstrap 1.6.5; see Known issues.

### Screen reader expectations

- With a correctly wired overlay (`id` plus `role="tooltip"`, e.g. Paragon's `Tooltip` or `Popover`), the overlay text is exposed as the trigger's accessible description and is typically read after the trigger's name and role while the overlay is shown. *(Verification pending per the policy's AT matrix.)*
- Overlay content without `role="tooltip"` and an `id` (such as the plain `div` in the example above) is not associated with the trigger and is not announced on focus.

### Application responsibilities

- Use a natively focusable trigger (normally a `Button`). `OverlayTrigger` only attaches event handlers; it cannot make a non-interactive element keyboard-operable.
- Never use `trigger="hover"` by itself — keyboard and touch users would never see the overlay. React-Bootstrap logs a console warning for this.
- Prefer Paragon's `Tooltip` or `Popover` as the overlay content, with a unique `id`, so the automatic `aria-describedby` association applies.
- Don't place interactive content in hover/focus overlays — focus cannot reach it before the overlay hides. Use `ModalPopup` for interactive overlay content.
- Given the 1.4.13 gaps below, don't put essential information in hover/focus overlays; make it available in persistent UI as well.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — the injected `aria-describedby` reference
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/) — the trigger element
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — custom overlay content styled by the consumer

### Known issues

Identified in the August 2026 accessibility audit; both are inherited from React-Bootstrap 1.6.5:

- Hover-triggered overlay content hides on mouse-out of the trigger and cannot itself be hovered — fails the *hoverable* requirement of WCAG 1.4.13.
- There is no <kbd>Escape</kbd> dismissal unless `rootClose` is set on `Overlay`, so content shown on hover or focus cannot be dismissed without moving the pointer or focus — fails the *dismissible* requirement of WCAG 1.4.13.
