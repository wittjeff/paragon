---
title: 'Avatar'
type: 'component'
components:
- Avatar
categories:
- Imagery & Iconography
status: 'New'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

The Avatar component represents a user’s identity in the UI.

It is used in the global navigation’s user menu and may also be used to indicate ownership of user generated content such as a discussion post or open response submission.


## Basic Usage

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <Stack
      className="bg-light p-2"
      gap={2}
      direction={ isExtraSmall ? "vertical" : "horizontal" }
    >
      <Avatar size="xxl" />
      <Avatar size="xl" />
      <Avatar size="lg" />
      <Avatar />
      <Avatar size="sm" />
      <Avatar size="xs" />
    </Stack>
)}
```


## Huge

```jsx live
<>
  <Avatar size="huge" />
</>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Avatar` renders a single `<img>` element. There is no matching [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/); standard image text-alternative guidance applies.

Avatar renders static content and has no keyboard interaction of its own.

### ARIA roles, states, and properties

No ARIA attributes are rendered. The image's accessible name comes entirely from the `alt` prop, which defaults to an empty string — **by default an Avatar is decorative** and hidden from assistive technology.

### Screen reader expectations

- With the default `alt=""`, the avatar is skipped entirely. *(Verification pending per the policy's AT matrix.)*
- With `alt` set (usually the user's name), it announces as that name plus "image" — e.g. "Casey, image". *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Keep the default empty `alt` only when the user's identity is already adjacent as text (e.g. next to a username in a post header) — announcing the name twice helps no one.
- Set `alt` to the user's name whenever the avatar is the only thing identifying the user.
- If the avatar is the sole content of a link or button (e.g. a user-menu trigger), an empty `alt` leaves that control with no accessible name — name the control (`aria-label` on the control, or a non-empty `alt`).
- Name the identity, not the picture: `alt="Casey"`, not `alt="smiling person"`.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Image has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/23a2a8/) — when the avatar conveys identity
- [Image accessible name is descriptive](https://www.w3.org/WAI/standards-guidelines/act/rules/qt1vmo/) — the `alt` should identify the user
- [Image not in the accessibility tree is decorative](https://www.w3.org/WAI/standards-guidelines/act/rules/e88epe/) — the default `alt=""` rendering

### Known issues

None identified in the August 2026 accessibility audit.
