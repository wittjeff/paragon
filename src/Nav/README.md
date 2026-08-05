---
title: 'Nav'
type: 'component'
components:
- Nav
- NavItem
- NavLink
- NavDropdown
categories:
- Navigation
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

Navigation bits in Bootstrap all share a general ``Nav`` component and styles. Swap ``variant``s to switch between each style. The base ``Nav`` component is built with flexbox and provide a strong foundation for building all types of navigation components.

<p>
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/navs/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for additional documentation.
  </a>
</p>

## Basic Usage

```jsx live
() => {
  const handleSelect = (eventKey) => alert(`selected ${eventKey}`);

  return (
    <Nav variant="pills" activeKey="1" onSelect={handleSelect}>
      <Nav.Item>
        <Nav.Link eventKey="1" href="#/home">
          NavLink 1 content
        </Nav.Link>
      </Nav.Item>
      <Nav.Item>
        <Nav.Link eventKey="2" title="Item">
          NavLink 2 content
        </Nav.Link>
      </Nav.Item>
      <Nav.Item>
        <Nav.Link eventKey="3" disabled>
          NavLink 3 content
        </Nav.Link>
      </Nav.Item>
      <NavDropdown title="Dropdown" id="nav-dropdown">
        <NavDropdown.Item eventKey="4.1">Action</NavDropdown.Item>
        <NavDropdown.Item eventKey="4.2">Another action</NavDropdown.Item>
        <NavDropdown.Item eventKey="4.3">Something else here</NavDropdown.Item>
        <NavDropdown.Divider />
        <NavDropdown.Item eventKey="4.4">Separated link</NavDropdown.Item>
      </NavDropdown>
    </Nav>
  );
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Paragon's `Nav` is a pass-through of [React-Bootstrap's Nav](https://react-bootstrap-v4.netlify.app/components/navs/) (pinned at 1.6.5). There is no single APG pattern for a generic nav: by default it renders a group of links with no ARIA roles. Its semantics change with context — inside a `TabContainer` it becomes a `tablist` and follows the [APG tabs pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/) (see the [Tabs](/components/tab) accessibility section for that treatment and its deviations).

### Keyboard support

Provided by React-Bootstrap:

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus between nav links; every enabled link is in the page tab sequence. |
| <kbd>Enter</kbd> | Activates the focused link (native link behavior). |
| <kbd>Space</kbd> | Fires the link's selection handler (added by React-Bootstrap's `SafeAnchor`); it does not trigger native navigation on links with a real `href`. |
| <kbd>Arrow Left</kbd> / <kbd>Arrow Up</kbd> | When an item is active (`activeKey`), selects and focuses the previous enabled item, wrapping at the ends — selection follows focus. |
| <kbd>Arrow Right</kbd> / <kbd>Arrow Down</kbd> | As above, for the next enabled item. |

Note: the arrow-key selection behavior is attached unconditionally by React-Bootstrap's `AbstractNav` — it applies even outside a tablist, whenever an item is marked active.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Container (`Nav`) | `<div class="nav">` | No role, landmark, or list semantics by default; use the `as` prop to change the element. Inside a `TabContainer`, React-Bootstrap sets `role="tablist"`. |
| `Nav.Item` | `<div class="nav-item">` | Plain wrapper, no role. |
| `Nav.Link` | `<a>` (React-Bootstrap `SafeAnchor`) | With `href`: an ordinary link. Without `href`: React-Bootstrap adds `href="#"` and `role="button"`. |
| `Nav.Link` (disabled) | `aria-disabled="true"`, `tabindex="-1"` | Removed from the tab sequence; clicks are suppressed. |
| `Nav.Link` (in a tablist) | `role="tab"`, `aria-selected`, `id`/`aria-controls` | Wired by `TabContainer`'s context. |
| `NavDropdown` | See [Dropdown](/components/dropdown) | Same toggle/menu semantics — including the `aria-haspopup` deviation documented there. |

The active `Nav.Link` outside a tablist is indicated by the `active` class only — no ARIA state is set on it (see Known issues).

### Screen reader expectations

- By default, links announce as plain links (or buttons, when they have no `href`) with their text; the container adds no grouping or landmark announcement. *(Verification pending per the policy's AT matrix.)*
- The currently active item is not distinguished in speech outside a tablist context.

### Application responsibilities

- Nav renders no landmark: for site or page navigation, render it as a labeled landmark (`as="nav"` plus `aria-label`, or wrap it in a `<nav>`), so screen reader users can jump to it.
- Give every `Nav.Link` a real `href` when it navigates; reserve `href`-less links (announced as buttons) for in-page actions.
- If the active link represents the current page, add `aria-current="page"` to it yourself — the component does not set it.
- For tab-style switching of in-page panels, compose `Nav` with `TabContainer` (or use the [Tabs](/components/tab) component) so tab semantics are exposed; don't rely on `variant="tabs"` styling alone.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Link has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/) — every `Nav.Link`
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-disabled`, `aria-selected`
- [ARIA required context role](https://www.w3.org/WAI/standards-guidelines/act/rules/ff89c9/) — `role="tab"` links must be owned by a `tablist` (satisfied in `TabContainer` usage)
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

Identified in the August 2026 accessibility audit:

- Outside a tablist, the active item is conveyed by the `active` class only — no `aria-current` or other ARIA state is rendered, so the visual "you are here" indication has no programmatic equivalent unless the application adds it.
- Paragon has no unit tests for `Nav`; keyboard and ARIA behavior is inherited from React-Bootstrap 1.6.5 without local assertions.
