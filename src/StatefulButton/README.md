---
title: 'StatefulButton'
type: 'component'
components:
- StatefulButton
categories:
- Buttonlike
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: ''
---

The stateful button is a button used to display an actionable icon.

## Basic usage

```jsx live
() => {
  const props = {
    labels: {
      default: 'Save',
      pending: 'Saving',
      complete: 'Saved',
      error: 'Error'
    },
    variant: 'primary',
  };
  return (
    <Stack direction="horizontal" gap={2}>
      <StatefulButton {...props} />
      <StatefulButton state="pending" {...props} />
      <StatefulButton state="complete" {...props} />
      <StatefulButton state="error" {...props} />
    </Stack>
  );
};
```

## Custom icons and disabled states

```jsx live
() => {
  const downloadButtonProps = {
    labels: {
      default: 'Download',
      pending: 'Downloading',
      complete: 'Downloaded',
    },
    icons: {
      default: <Icon src={Download} />,
      pending: <Icon src={SpinnerSimple} className="icon-spin" />,
      complete: <Icon src={Check} />,
    },
    disabledStates: ['pending', 'complete'],
    variant: 'primary',
  };
  return (
    <Stack direction="horizontal" gap={2}>
      <StatefulButton state="default" {...downloadButtonProps} />
      <StatefulButton state="pending" {...downloadButtonProps} />
      <StatefulButton state="complete" {...downloadButtonProps} />
    </Stack>
  );
};
```

## Custom states with Paragon icons

```jsx live
() => {
  const buttonProps = {
    labels: {
      unedited: 'Save (no changes)',
      edited: 'Save Changes',
    },
    icons: {
      unedited: <Icon src={Add} />,
      edited: <Icon src={Add} />,
    },
    disabledStates: ['unedited'],
    variant: 'primary',
  };
  return (
    <Stack direction="horizontal" gap={2}>
      <StatefulButton state="unedited" {...buttonProps} />
      <StatefulButton state="edited" {...buttonProps} />
    </Stack>
  );
};
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`StatefulButton` renders a Paragon [Button](/components/button) (a native `<button type="button">`) whose label and icon swap with the `state` prop. It follows the [APG button pattern](https://www.w3.org/WAI/ARIA/apg/patterns/button/) and adds live-region and disabled semantics for its state changes.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Activates the button (native behavior). In a disabled state the button stays focusable and activation is silently swallowed. |

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Button | `aria-live="assertive"` | Always present; when the label changes with `state`, the new label is announced immediately, interrupting other speech. |
| Button | `aria-disabled` | `true` while `state` is listed in `disabledStates` (default `['pending', 'complete']`). The native `disabled` attribute is deliberately **not** set, so the button keeps focus during and after a state change; clicks are ignored in code instead. |
| State icons | `aria-hidden` `<svg>` | Decorative (spinner, check, cancel by default). |
| Label fallback | `.sr-only` text = the raw `state` key | Rendered only when `labels` has no entry for the current state. |

### Screen reader expectations

- On a state change, the new label is announced without moving focus — e.g. activating "Download" announces "Downloading", then "Downloaded". *(Verification pending per the policy's AT matrix.)*
- In a disabled state the button announces as dimmed/disabled but remains focusable.

### Application responsibilities

- Provide a localized label for **every** state you use; otherwise the raw, unlocalized state key (e.g. "pending") is what screen reader users hear.
- Make labels state-descriptive on their own ("Downloading", not "Please wait") — they are announced without visual context.
- Choose `disabledStates` deliberately: a state listed there keeps focus but ignores activation, which suits brief in-flight states; for long-lived inert states consider whether a truly disabled control communicates better.
- Icons are decorative — never encode information only in the icon difference between states.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — in every state, including the sr-only fallback
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-live`, `aria-disabled`
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

None identified in the August 2026 accessibility audit. Note that `aria-live="assertive"` (rather than `"polite"`) is a deliberate but interruptive choice inherited from the current implementation; it is flagged for review in AT verification rather than as a confirmed defect.
