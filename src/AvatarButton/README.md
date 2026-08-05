---
title: 'AvatarButton'
components:
- AvatarButton
categories:
- Buttonlike
type: 'component'
status: 'New'
designStatus: 'Done'
devStatus: 'Done'
notes: |


---

A button that contains the user’s Avatar.

## Basic Usage

```jsx live
<>
  <div>
    <AvatarButton size="lg" src="https://picsum.photos/128/128/">Casey</AvatarButton>
  </div>
  <div>
    <AvatarButton size="md" src="https://picsum.photos/128/128/">Casey</AvatarButton>
  </div>
  <div>
    <AvatarButton size="sm" src="https://picsum.photos/128/128/">Casey</AvatarButton>
  </div>
</>
```


## With Dropdown

```jsx live
<Dropdown>
  <Dropdown.Toggle as={AvatarButton} src="https://picsum.photos/128/128">
    Casey
  </Dropdown.Toggle>

  <Dropdown.Menu alignRight>
    <Dropdown.Item href="#/action-1">Resume your last course</Dropdown.Item>
    <Dropdown.Item href="#/action-2">Dashboard</Dropdown.Item>
    <Dropdown.Item href="#/action-3">Profile</Dropdown.Item>
    <Dropdown.Item href="#/action-3">Account</Dropdown.Item>
    <Dropdown.Divider />
    <Dropdown.Item href="#/action-3">Sign out</Dropdown.Item>
  </Dropdown.Menu>
</Dropdown>
```

Props set on Dropdown.Toggle get passed through to the "as" component

```jsx live
<Dropdown>
  <Dropdown.Toggle as={AvatarButton} size="sm" src="https://picsum.photos/128/128/">
    Casey
  </Dropdown.Toggle>

  <Dropdown.Menu alignRight>
    <Dropdown.Item href="#/action-1">Resume your last course</Dropdown.Item>
    <Dropdown.Item href="#/action-2">Dashboard</Dropdown.Item>
    <Dropdown.Item href="#/action-3">Profile</Dropdown.Item>
    <Dropdown.Item href="#/action-3">Account</Dropdown.Item>
    <Dropdown.Divider />
    <Dropdown.Item href="#/action-3">Sign out</Dropdown.Item>
  </Dropdown.Menu>
</Dropdown>
```

## No label

For use in mobile viewports or constrained views.

```jsx live
<>
  <div>
    <AvatarButton showLabel={false} size="lg" src="https://picsum.photos/128/128/">Casey</AvatarButton>
  </div>
  <div>
    <AvatarButton showLabel={false} size="md" src="https://picsum.photos/128/128/">Casey</AvatarButton>
  </div>
  <div>
    <AvatarButton showLabel={false} size="sm" src="https://picsum.photos/128/128/">Casey</AvatarButton>
  </div>

  <Dropdown>
    <Dropdown.Toggle  showLabel={false} as={AvatarButton} src="https://picsum.photos/128/128/">
      Casey
    </Dropdown.Toggle>

    <Dropdown.Menu alignRight>
      <Dropdown.Item href="#/action-1">Resume your last course</Dropdown.Item>
      <Dropdown.Item href="#/action-2">Dashboard</Dropdown.Item>
      <Dropdown.Item href="#/action-3">Profile</Dropdown.Item>
      <Dropdown.Item href="#/action-3">Account</Dropdown.Item>
      <Dropdown.Divider />
      <Dropdown.Item href="#/action-3">Sign out</Dropdown.Item>
    </Dropdown.Menu>
  </Dropdown>
</>
```

CSS overrides can be applied to this component through the `.btn-avatar` class name.

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`AvatarButton` renders a Paragon [Button](/components/button) (a native `<button type="button">`) containing an `Avatar` image and, optionally, the label text. It follows the [APG button pattern](https://www.w3.org/WAI/ARIA/apg/patterns/button/) via native semantics.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Activates the button (native `<button>` behavior). |

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Button | `<button type="button">` | Native semantics via `Button`. |
| Avatar image, `showLabel` (default) | `alt=""` | Decorative — the visible label text is the accessible name. |
| Avatar image, `showLabel={false}` | `alt` = `children` | The image alt carries the button's accessible name when the label is hidden. |

### Screen reader expectations

- Announces the user's name and "button" — e.g. "Casey, button" — whether or not the label is visually shown. *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Always pass the user's name as `children`, even with `showLabel={false}` — it becomes the avatar's `alt` and the button's only accessible name. Without it the button is unnamed.
- `src` is optional; the fallback avatar changes nothing about naming.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — especially with `showLabel={false}`
- [Image has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/23a2a8/) — the avatar when it is the name source
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

None identified in the August 2026 accessibility audit.
