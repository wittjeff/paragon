---
title: 'Popover'
type: 'component'
components:
- Popover
- PopoverTitle
- PopoverContent
categories:
- Overlays
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

Popovers are small overlays that present additional content without cluttering the page.

Note that from accessibility perspective `Popover` is treated as a tooltip (the element has `role="tooltip"`) which means that it
shouldn't contain interactive elements (e.g, buttons, links, etc.), you can read more about tooltip specifications [here](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/tooltip_role).
Try using <Link to="/components/modal/modal-popup/">ModalPopup</Link> instead if you want `Popover`'s behaviour with interactive elements.

## Basic Usage

```jsx live
<>
  {['top', 'right', 'bottom', 'left'].map((placement) => (
    <OverlayTrigger
      trigger="click"
      key={placement}
      placement={placement}
      overlay={
        <Popover id={`popover-positioned-${placement}`}>
          <Popover.Title as="h3">{`Popover ${placement}`}</Popover.Title>
          <Popover.Content>
            <strong>Holy guacamole!</strong> Check this info.
          </Popover.Content>
        </Popover>
      }
    >
      <Button variant="secondary" className="mr-2 mb-2">Popover on {placement}</Button>
    </OverlayTrigger>
  ))}
</>
```

## State variants

```jsx live
<>
  <OverlayTrigger
    trigger="click"
    key="top-basic"
    placement="top"
    overlay={
      <Popover className="popover-positioned-top">
        <Popover.Title as="h5">Popover Basic</Popover.Title>
        <Popover.Content>
          <strong>Holy guacamole!</strong> Check this info.
        </Popover.Content>
      </Popover>
    }
  >
    <Button variant="secondary" className="mr-2 mb-2">
      Popover Basic
    </Button>
  </OverlayTrigger>
  <OverlayTrigger
    trigger="click"
    key="top-success"
    placement="top"
    overlay={
      <Popover className="popover-positioned-top" variant="success">
        <Popover.Title as="h5"><Icon src={CheckCircle}/> Popover Success</Popover.Title>
        <Popover.Content>
          <strong>Holy guacamole!</strong> Check this info.
        </Popover.Content>
      </Popover>
    }
  >
    <Button variant="secondary" className="mr-2 mb-2">
      Popover Success
    </Button>
  </OverlayTrigger>
  <OverlayTrigger
    trigger="click"
    key="top-warning"
    placement="top"
    overlay={
      <Popover className="popover-positioned-top" variant="warning">
        <Popover.Title as="h5"><Icon src={WarningFilled} /> Popover Warning</Popover.Title>
        <Popover.Content>
          <strong>Holy guacamole!</strong> Check this info.
        </Popover.Content>
      </Popover>
    }
  >
    <Button variant="secondary" className="mr-2 mb-2">
      Popover Warning
    </Button>
  </OverlayTrigger>
  <OverlayTrigger
    trigger="click"
    key="top-danger"
    placement="top"
    overlay={
      <Popover className="popover-positioned-top" variant="danger">
        <Popover.Title as="h5"><Icon src={Info} /> Popover Danger</Popover.Title>
        <Popover.Content>
          <strong>Holy guacamole!</strong> Check this info.
        </Popover.Content>
      </Popover>
    }
  >
    <Button variant="secondary" className="mr-2 mb-2">
      Popover Danger
    </Button>
  </OverlayTrigger>
</>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Paragon's `Popover` wraps [React-Bootstrap's Popover](https://react-bootstrap-v4.netlify.app/components/overlays/#popovers). There is no APG "popover" pattern; the rendered element uses `role="tooltip"`, so the closest match is the [APG tooltip pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tooltip/), which suits **text-only** content. For interactive overlay content, use `ModalPopup` instead. Show/hide behavior comes from the `Overlay`/`OverlayTrigger` components that position the popover — see the Overlay accessibility notes.

### Keyboard support

`Popover` itself is a static container; keyboard behavior belongs to the trigger, provided by React-Bootstrap's `OverlayTrigger`:

| Key | Action |
| --- | --- |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | With `trigger="click"` on a button (as in the examples above): toggles the popover. Focus stays on the trigger. |
| <kbd>Tab</kbd> | With the default `['hover', 'focus']` trigger: focusing the trigger shows the popover, blurring hides it. |
| <kbd>Escape</kbd> | **Not handled by default** — closes the popover only if `rootClose` is passed through to the underlying `Overlay` (see Known issues). |

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Popover root (`.popover`) | `role="tooltip"` | Provided by React-Bootstrap. See the deviation note below. |
| Popover root | `id` (required prop) | Required "for accessibility": it is what the automatic `aria-describedby` association points at. |
| Trigger element | `aria-describedby` = popover `id` | Added automatically only while the popover is shown (react-overlays `ariaDescribedBy` popper modifier); removed on hide. |
| `Popover.Title` | renders a `div` by default | Not a heading unless you pass `as` (the examples use `as="h3"`). |
| Trigger element | *(no state)* | No `aria-expanded` or `aria-haspopup` is set on the trigger. |

**Deviation from the ARIA tooltip role** (identified in the August 2026 accessibility audit): `role="tooltip"` is defined for non-interactive descriptive text, but `Popover` renders arbitrary structured content (title and body) under that role, and focus never moves into it. Interactive elements inside a popover would sit inside a tooltip role and be unreachable before it hides — which is why this component should hold text only.

### Screen reader expectations

- After the trigger is activated (click trigger), the popover's full text — title and content flattened together — becomes the trigger's accessible description and is typically read after the trigger's name and role. *(Verification pending per the policy's AT matrix.)*
- Opening the popover moves neither focus nor the reading cursor; nothing is announced proactively on open. *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Always supply a unique `id` — without it the trigger gets no `aria-describedby` association.
- Keep content textual and supplementary. If users must interact with the content (buttons, links, form fields), use `ModalPopup` instead.
- Give the trigger a discernible accessible name of its own; the popover text is a description, not a name.
- Pass a heading element via `Popover.Title`'s `as` prop only if it fits your page's heading structure; the default is a styled `div`.
- Prefer `trigger="click"` over hover for content of any length, and mirror essential information in persistent UI (see the WCAG 1.4.13 gaps under Overlay).

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Role attribute has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/674b10/) — the popover's `role="tooltip"`
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — the injected `aria-describedby` reference
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/) — the trigger element
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — popover text, including the `variant` color treatments

### Known issues

Identified in the August 2026 accessibility audit; inherited from React-Bootstrap 1.6.5:

- A click-opened popover cannot be dismissed with <kbd>Escape</kbd> unless `rootClose` is explicitly passed; the only default keyboard dismissal is activating the trigger again.
- When shown on hover/focus, popovers share the Overlay component's WCAG 1.4.13 failures: the content is not *hoverable* (mouse-out of the trigger hides it) and not *dismissible* without moving pointer or focus.
