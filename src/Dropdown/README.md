---
title: 'Dropdown'
type: 'component'
components:
- Dropdown
- DropdownItem
- DropdownMenu
- DropdownButton
- DropdownToggle
- DropdownHeader
- DropdownDivider
categories:
- Navigation
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
---

<p className="lead">
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/dropdowns/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for documentation.
  </a>
</p>

## Basic Usage
```jsx live
<DropdownButton id="dropdown-basic-button" title="Dropdown button">
  <Dropdown.Item href="#/action-1">Action</Dropdown.Item>
  <Dropdown.Item href="#/action-2">Another action</Dropdown.Item>
  <Dropdown.Item href="#/action-3">Something else</Dropdown.Item>
</DropdownButton>
```

## Advanced Usage

```jsx live
<>
  <Dropdown onToggle={(isOpen, event, metadata) => console.log('debug', 'onToggle', { isOpen, event, metadata })} className="mb-3">
    <Dropdown.Toggle variant="success" id="dropdown-basic-1">
      Dropdown Button
    </Dropdown.Toggle>
    <Dropdown.Menu>
      <Dropdown.Header>Header</Dropdown.Header>
      <Dropdown.Item>Action</Dropdown.Item>
      <Dropdown.Item>Another action</Dropdown.Item>
      <Dropdown.Divider />
      <Dropdown.Item>Something else</Dropdown.Item>
    </Dropdown.Menu>
  </Dropdown>

  <Dropdown variant="dark" onToggle={(isOpen, event, metadata) => console.log('debug', 'onToggle', { isOpen, event, metadata })} className="mb-3">
    <Dropdown.Toggle variant="success" id="dropdown-basic-2">
      Dark Variant
    </Dropdown.Toggle>
    <Dropdown.Menu>
      <Dropdown.Header>Header</Dropdown.Header>
      <Dropdown.Item>Action</Dropdown.Item>
      <Dropdown.Item>Another action</Dropdown.Item>
      <Dropdown.Divider />
      <Dropdown.Item>Something else</Dropdown.Item>
    </Dropdown.Menu>
  </Dropdown>
  <Dropdown autoClose={false} onToggle={(isOpen, event, metadata) => console.log('debug', 'onToggle', { isOpen, event, metadata })} className="mb-3">
    <Dropdown.Toggle variant="success" id="dropdown-basic-2">
      autoClose=false
    </Dropdown.Toggle>
    <Dropdown.Menu>
      <Dropdown.Header>Header</Dropdown.Header>
      <Dropdown.Item>Action</Dropdown.Item>
      <Dropdown.Item>Another action</Dropdown.Item>
      <Dropdown.Divider />
      <Dropdown.Item>Something else</Dropdown.Item>
    </Dropdown.Menu>
  </Dropdown>
  <Dropdown autoClose="inside" onToggle={(isOpen, event, metadata) => console.log('debug', 'onToggle', { isOpen, event, metadata })} className="mb-3">
    <Dropdown.Toggle variant="success" id="dropdown-basic-3">
      autoClose=inside
    </Dropdown.Toggle>
    <Dropdown.Menu>
      <Dropdown.Header>Header</Dropdown.Header>
      <Dropdown.Item>Action</Dropdown.Item>
      <Dropdown.Item>Another action</Dropdown.Item>
      <Dropdown.Divider />
      <Dropdown.Item>Something else</Dropdown.Item>
    </Dropdown.Menu>
  </Dropdown>
  <Dropdown autoClose="outside" onToggle={(isOpen, event, metadata) => console.log('debug', 'onToggle', { isOpen, event, metadata })} className="mb-3">
    <Dropdown.Toggle variant="success" id="dropdown-basic-4">
      autoClose=outside
    </Dropdown.Toggle>
    <Dropdown.Menu>
      <Dropdown.Header>Header</Dropdown.Header>
      <Dropdown.Item>Action</Dropdown.Item>
      <Dropdown.Item>Another action</Dropdown.Item>
      <Dropdown.Divider />
      <Dropdown.Item>Something else</Dropdown.Item>
    </Dropdown.Menu>
  </Dropdown>
</>
```

## With IconButton

You can use `Dropdown.Toggle` with [IconButton](/components/iconbutton) component, note that all props you provide to `Dropdown.Toggle` will get passed down to the `IconButton`.

```jsx live
<Dropdown>
  <Dropdown.Toggle
    id="dropdown-toggle-with-iconbutton"
    as={IconButton}
    src={MoreVert}
    iconAs={Icon}
    variant="primary"
    aria-label="More actions"
  />
  <Dropdown.Menu>
    <Dropdown.Item href="#/action-1">Action</Dropdown.Item>
    <Dropdown.Item href="#/action-2">Another action</Dropdown.Item>
    <Dropdown.Item href="#/action-3">Something else</Dropdown.Item>
  </Dropdown.Menu>
</Dropdown>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Paragon's `Dropdown` wraps [React-Bootstrap's Dropdown](https://react-bootstrap-v4.netlify.app/components/dropdowns/). It behaves as a **disclosure that reveals a list of links or buttons** — it does *not* implement the ARIA `menu`/`menuitem` roles of the [APG menu button pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menu-button/). See the deviation note below.

### Keyboard support

Provided by React-Bootstrap / react-overlays:

| Key | Action |
| --- | --- |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | On the toggle: opens or closes the menu (native button activation). |
| <kbd>Arrow Down</kbd> | On the toggle: opens the menu and moves focus to the first item. In the menu: moves focus to the next item. |
| <kbd>Arrow Up</kbd> | In the menu: moves focus to the previous item. |
| <kbd>Escape</kbd> | Closes the menu and returns focus to the toggle. |
| <kbd>Tab</kbd> | Moves focus out; the menu closes. |
| <kbd>Enter</kbd> | On an item: activates it (follows the link or fires the button). |

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Toggle (`Dropdown.Toggle`) | `<button>` | Native button semantics; requires an `id` ("necessary for assistive technologies"). |
| Toggle | `aria-haspopup="true"` | See deviation note — announces a popup that is not exposed as an ARIA menu. |
| Toggle | `aria-expanded` | Reflects open/closed state. |
| Menu (`Dropdown.Menu`) | *(no role)* | Rendered as a plain container; items are links (`href`) or buttons. |
| Item (`Dropdown.Item`) | `<a>` or `<button>` | Link when `href` is set; otherwise button-like anchor. |

**Deviation from the APG pattern:** the toggle sets `aria-haspopup="true"` (which announces "menu" in most screen readers), but the popup itself has no `role="menu"` and items have no `role="menuitem"` — inherited from Bootstrap's intentional avoidance of ARIA menu semantics. The result is a mixed signal: users are told to expect a menu but arrow-key/menu semantics are not exposed as one. Identified in the August 2026 accessibility audit; see Known issues.

### Screen reader expectations

- The toggle announces as a button with your label, "menu", and collapsed/expanded state — e.g. "Dropdown Button, menu button, collapsed". *(Verification pending per the policy's AT matrix.)*
- Items announce as links or buttons with their text, not as menu items.

### Application responsibilities

- Give the toggle a discernible label: visible text, or `iconButtonProps` with a meaningful `alt` when using the icon-button variant.
- Use `href` items for navigation and plain items for actions, so the announced role matches the behavior.
- With `autoClose={false}` or `autoClose="inside"`, you alter dismissal behavior — ensure a keyboard user can still predictably close the menu (Escape still closes via `rootClose` unless you've made closing conditional).
- Dropdown menus are not a substitute for `<select>` in forms — use `Form.Control as="select"` or `Form.Autosuggest` for value selection.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — the toggle
- [Link has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/) — items rendered with `href`
- [ARIA state or property is permitted](https://www.w3.org/WAI/standards-guidelines/act/rules/5c01ea/)
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-expanded`, `aria-haspopup`
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

- `aria-haspopup="true"` without `role="menu"` on the popup produces a misleading "menu" announcement (deviation from the APG menu button pattern) — [#4403](https://github.com/openedx/paragon/issues/4403), identified in the August 2026 accessibility audit.
- Unit tests cover open/close behavior only; no automated keyboard or ARIA assertions yet ([#4403](https://github.com/openedx/paragon/issues/4403)).
