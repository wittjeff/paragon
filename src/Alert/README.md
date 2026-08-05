---
title: 'Alert'
type: 'component'
components:
- Alert
- AlertHeading
- AlertLink
categories:
- Status & metadata
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

The `Alert` component displays a short, important message in a way that attracts the user's attention.
Alerts offer four severity levels that set a distinctive icon and color:
- **Info**: used to convey general information or actions that aren't critical, note that info variant should not contain any icons in it
- **Warning**: used to display information that needs attention
- **Success**: used for success messages
- **Danger**: used to communicate problems that have to be resolved immediately

This component utilizes and extends the `Alert` component from react-bootstrap.<br/> <a href="https://react-bootstrap-v4.netlify.app/components/alerts/" target="_blank" rel="noopener noreferrer"> See React-Bootstrap for additional documentation.</a>

## Basic Usage

```jsx live
<>
<Alert variant="success">
  This is a "success" alert with{' '}
  <Alert.Link href="#">an example link</Alert.Link>. Give it a click if you
  like.
</Alert>
<Alert variant="info">
  This is a "info" alert with{' '}
  <Alert.Link href="#">an example link</Alert.Link>. Give it a click if you
  like.
</Alert>
<Alert variant="danger">
  This is a "danger" alert with{' '}
  <Alert.Link href="#">an example link</Alert.Link>. Give it a click if you
  like.
</Alert>
<Alert variant="warning">
  This is a "warning" alert with{' '}
  <Alert.Link href="#">an example link</Alert.Link>. Give it a click if you
  like.
</Alert>
</>
```

### With Action Buttons
The ``Alert`` component supports a dismissible button and a custom call-to-action button, via the ``dismissible`` and ``button`` props respectively. The buttons may be right aligned or stacked. On extra small screen widths, the buttons will be stacked.

The stacked variant should be used for:
- Placement in sidebars or small container
- Extra-small breakpoint and below
---

```jsx live
<>
  <Alert
    variant="info"
    actions={[
      <Button>Hello</Button>,
    ]}
    dismissible
    closeLabel="Dismiss"
    onClose={(e) => { console.log('closed', e); } }
  >
    This is a "info" alert with{' '}
    <Alert.Link href="#">an example link</Alert.Link>. Give it a click if you
    like.
  </Alert>
  <Alert variant="warning" dismissible>
    This is a "info" alert with{' '}
    <Alert.Link href="#">an example link</Alert.Link>. Give it a click if you
    like.
  </Alert>
  <Alert
    variant="success"
    icon={CheckCircle}
    dismissible
    actions={[
      <Button>Hello</Button>,
    ]}
  >
    <Alert.Heading>Hey, nice to see you</Alert.Heading>
    <p>
      Aww yeah, you successfully read this important alert message. This example
      text is going to run a bit longer so that you can see how spacing within an
      alert works with this kind of content.
    </p>
  </Alert>
  <Alert
    variant="danger"
    icon={Info}
    actions={[
      <Button>Hello</Button>,
    ]}
    dismissible
    onClose={(e) => { console.log('closed', e); } }
    stacked
  >
    <Alert.Heading>Hey, nice to see you</Alert.Heading>
    <p>
      Aww yeah, you successfully read this important alert message. This example
      text is going to run a bit longer so that you can see how spacing within an
      alert works with this kind of content.
    </p>
  </Alert>
</>
```

### Variants

```jsx live
<>
<Alert variant="success" dismissible icon={CheckCircle}>
  <Alert.Heading>Hey, nice to see you</Alert.Heading>
  <p>
    Aww yeah, you successfully read this important alert message. This example
    text is going to run a bit longer so that you can see how spacing within an
    alert works with this kind of content.
  </p>
</Alert>
<Alert variant="warning" dismissible icon={WarningFilled}>
  <Alert.Heading>Hey, nice to see you</Alert.Heading>
  <p>
    Aww yeah, you successfully read this important alert message. This example
    text is going to run a bit longer so that you can see how spacing within an
    alert works with this kind of content.
  </p>
</Alert>
<Alert variant="danger" dismissible icon={Info}>
  <Alert.Heading>Hey, nice to see you</Alert.Heading>
  <p>
    Aww yeah, you successfully read this important alert message. This example
    text is going to run a bit longer so that you can see how spacing within an
    alert works with this kind of content.
  </p>
</Alert>
</>
```

### Kitchen Sink

```jsx live
<Alert
  variant="success"
  actions={[
    <Button>Hello</Button>
  ]}
  dismissible
  stacked
>
  <Alert.Heading>Hey, nice to see you</Alert.Heading>
  <p>
    Aww yeah, you successfully read this important alert message. This example
    text is going to run a bit longer so that you can see how spacing within an
    alert works with this kind of content.
  </p>
  <hr />
  <p className="mb-0">
    Whenever you need to, be sure to use margin utilities to keep things nice
    and tidy.
  </p>
</Alert>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Paragon's `Alert` wraps [React-Bootstrap's Alert](https://react-bootstrap-v4.netlify.app/components/alerts/) and follows the [APG alert pattern](https://www.w3.org/WAI/ARIA/apg/patterns/alert/): a live region that announces a brief, important message when it appears, without moving focus. One deviation is noted below.

### Keyboard support

The alert itself is not focusable and has no keyboard interaction of its own. The optional dismiss button (`dismissible`) and call-to-action buttons (`actions`) are native buttons reached with <kbd>Tab</kbd> and activated with <kbd>Enter</kbd> / <kbd>Space</kbd>; `Alert.Link` renders a regular link.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Alert container | `role="alert"` | Provided by React-Bootstrap; implies an assertive, atomic live region. Present whenever the alert is shown. |
| Dismiss button | `<button>` with visible text | Paragon renders its own text button — a localized "Dismiss" by default, overridable via `closeLabel`. React-Bootstrap's icon-only close button is not used. |
| Icon (`icon` prop) | `aria-hidden="true"` on the `svg` | Decorative; hidden from assistive technology. |
| `Alert.Heading` | `<div class="h4">` by default | Styled as a heading but not exposed as one — pass `as="h3"` (or the level that fits your page outline) to support heading navigation. |

**Deviation from the APG pattern:** the alert pattern is for non-interactive messages — content that requires user action points to the [alert dialog pattern](https://www.w3.org/WAI/ARIA/apg/patterns/alertdialog/) instead. With `dismissible` and/or `actions`, Paragon renders buttons inside the `role="alert"` live region. Identified in the August 2026 accessibility audit; see Known issues.

### Screen reader expectations

- When an alert appears after page load (`show` becomes `true`, or the alert mounts), its content is announced immediately, interrupting other speech, without moving focus. *(Verification pending per the policy's AT matrix.)*
- Alerts present in the initial page render are read in place as ordinary content, not announced.
- The dismiss button announces as "Dismiss, button" (or your `closeLabel`).
- The severity variant (success, danger, ...) is conveyed only by color and the optional decorative icon; it is not announced.

### Application responsibilities

- Start the message with text that conveys severity when it matters ("Error:", "Success:") — the variant color and icon are invisible to screen reader users.
- Keep alert content brief; `role="alert"` interrupts assertively. For less urgent messages consider [PageBanner](/components/pagebanner) (polite) or plain page content.
- You control `show` — when the user dismisses an alert, move focus somewhere sensible (e.g. the control or region that produced the alert); the focused dismiss button disappears with it.
- Localize `closeLabel` if the built-in translated "Dismiss" doesn't fit your context.
- If the message requires a decision before the user continues, use `AlertModal` instead of an alert.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — dismiss and action buttons
- [Link has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/) — `Alert.Link`
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — message text against variant backgrounds
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/) — buttons and links inside the alert

### Known issues

- Dismiss and action buttons render inside the `role="alert"` live region; the APG reserves the alert role for non-interactive content and points interactive messages to the alert dialog pattern — identified in the August 2026 accessibility audit.
