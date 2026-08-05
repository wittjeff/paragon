---
title: 'MailtoLink'
type: 'component'
components:
- MailtoLink
categories:
- Buttonlike
status: 'Needs Work'
designStatus: 'Done'
devStatus: 'To Do'
notes: |
  Remove unnecessary props.
  Remove baked in English strings.
  Use React.forwardRef for ref forwarding
---

## minimal usage

```jsx live
<MailtoLink to="edx@example.com">
  edx@example.com
</MailtoLink>
```

## with blank target

```jsx live
<MailtoLink to="edx@example.com" target="_blank">
  edx@example.com
</MailtoLink>
```

## with onClick

```jsx live
<MailtoLink
  to="edx@example.com"
  target="_blank"
  onClick={() => { /* some actions */ }}
>
  edx@example.com
</MailtoLink>
```

## with subject and body

```jsx live
<MailtoLink
  to="edx@example.com"
  subject="Check out this mailto component!"
  body="This mailto component is awesome!"
>
  email with subject and body
</MailtoLink>
```

## with cc and bcc

```jsx live
<MailtoLink
  cc="edx@example.com"
  bcc="edx@example.com"
>
  More mail, this time with cc and bcc
</MailtoLink>
```

## with multiple cc and bcc

```jsx live
<MailtoLink
  cc={['foo@example.com', 'bar@example.com', 'baz@example.com']}
  bcc={['foo@example.com', 'bar@example.com', 'baz@example.com']}
>
  edx@example.com
</MailtoLink>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`MailtoLink` builds a `mailto:` URL and renders it through [Hyperlink](/components/hyperlink), so it is a native `<a>` element — the [APG link pattern](https://www.w3.org/WAI/ARIA/apg/patterns/link/) via native semantics. Hyperlink's accessibility notes apply.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Enter</kbd> | Follows the link, opening the user's email client (native `<a>` behavior). |

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Link | `<a href="mailto:...">` | Native link role. |
| Link with `target="_blank"` | `rel="noopener noreferrer"` | Provided by `Hyperlink`. |
| External-link icon (`target="_blank"`) | `aria-hidden` `<svg>` + `.sr-only` text | The visually hidden text defaults to **"Dismiss"** — see Known issues. |

### Screen reader expectations

- Announces the link text and "link" — e.g. "edx@example.com, link". *(Verification pending per the policy's AT matrix.)*
- With the default props and `target="_blank"`, the announcement ends with the erroneous "Dismiss" suffix noted below.

### Application responsibilities

- Make the link text convey that it starts an email (the address itself, or e.g. "Email support").
- If you set `target="_blank"`, pass an `externalLink` object with meaningful, localized `alternativeText` and `title` (e.g. "in a new tab" / "Opens in a new tab") — do not rely on the defaults.
- Note the built-in default strings are hard-coded English and are not localized.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Link has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/)
- [Link in context is descriptive](https://www.w3.org/WAI/standards-guidelines/act/rules/5effbb/) — the name should convey the email action, including the icon's hidden text
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

- The default `externalLink.alternativeText` is the string "Dismiss", so a `target="_blank"` MailtoLink with default props appends the screen-reader-only text "Dismiss" to the link name instead of an "in a new tab" indication — a misleading text alternative (WCAG 1.1.1, 2.4.4), identified in the August 2026 accessibility audit. Unlike `Hyperlink`, these defaults also bypass localization.
