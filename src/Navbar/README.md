---
title: 'Navbar'
type: 'component'
components:
- Navbar
- NavbarBrand
- NavbarToggle
- NavbarCollapse
categories:
- Navigation
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes:
---

A powerful, responsive navigation header, the navbar. Includes support for branding, navigation, and more.

<p>
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/navbar/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for documentation.
  </a>
</p>

## Basic Usage

```jsx live
<Navbar bg="light" expand="lg">
  <Navbar.Brand href="#home">React-Bootstrap</Navbar.Brand>
  <Navbar.Toggle aria-controls="basic-navbar-nav" />
  <Navbar.Collapse id="basic-navbar-nav">
    <Nav className="mie-auto">
      <Nav.Link href="#home">Home</Nav.Link>
      <Nav.Link href="#link">Link</Nav.Link>
      <NavDropdown title="Dropdown" id="basic-nav-dropdown">
        <NavDropdown.Item href="#action/3.1">Action</NavDropdown.Item>
        <NavDropdown.Item href="#action/3.2">Another action</NavDropdown.Item>
        <NavDropdown.Item href="#action/3.3">Something</NavDropdown.Item>
        <NavDropdown.Divider />
        <NavDropdown.Item href="#action/3.4">Separated link</NavDropdown.Item>
      </NavDropdown>
    </Nav>
    <Form inline>
      <FormControl type="text" placeholder="Search" />
      <Button variant="outline-success">Search</Button>
    </Form>
  </Navbar.Collapse>
</Navbar>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Paragon's `Navbar` is a pass-through of [React-Bootstrap's Navbar](https://react-bootstrap-v4.netlify.app/components/navbar/) (pinned at 1.6.5): a `<nav>` landmark whose contents collapse behind a toggle button below the `expand` breakpoint. The toggle is a [disclosure](https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/)-style control, with deviations noted below.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus through the navbar's contents (brand link, toggle, links, form controls). Collapsed content is hidden with `display: none` and is not reachable. |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | On the toggle: expands or collapses the navbar body (native button activation). |

Keyboard behavior inside the navbar belongs to the composed components ([Nav](/components/nav), [Dropdown](/components/dropdown), form fields) — see their pages.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Container (`Navbar`) | `<nav role="navigation">` | Paragon defaults `as="nav"` and `role="navigation"` (the explicit role is redundant on `<nav>` but harmless). |
| Toggle (`Navbar.Toggle`) | `<button type="button">` with `aria-label` = `label` prop | Defaults to `"Toggle navigation"` (untranslated English). The default visual content is an empty CSS-background span, so the `aria-label` is the entire accessible name. |
| Toggle | *(no `aria-expanded`)* | Provided by React-Bootstrap: the expanded/collapsed state is reflected only in a `collapsed` class — no ARIA state is rendered. See Known issues. |
| Toggle | `aria-controls` *(consumer-supplied)* | Not rendered by the library; pass it yourself pointing at the `Navbar.Collapse` `id`, as in the example above. |
| Body (`Navbar.Collapse`) | `<div class="navbar-collapse">` | Shown/hidden via Bootstrap collapse classes; no ARIA of its own. |
| Brand (`Navbar.Brand`) | `<nav class="navbar">` (!) | Paragon's default `as: 'nav'` overrides React-Bootstrap's element choice — see Known issues. |

### Screen reader expectations

- The navbar announces as a "navigation" landmark. *(Verification pending per the policy's AT matrix.)*
- The toggle announces as "Toggle navigation, button" (or your `label`), with **no** expanded/collapsed state — a consequence of the missing `aria-expanded` below.

### Application responsibilities

- Pass `aria-controls` on `Navbar.Toggle` referencing the `id` of `Navbar.Collapse`.
- Localize the toggle's `label` prop — the default is English-only.
- If the page has more than one `<nav>` landmark, give the navbar an `aria-label` so landmarks are distinguishable.
- Until the Brand issue below is fixed, pass an explicit element: `<Navbar.Brand as="a" href="...">` (or `as="span"` when it is not a link).
- Mark the link to the current page inside the navbar with `aria-current="page"` — nothing in the component does this for you.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — the toggle
- [Link has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/) — the brand and nav links
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — a consumer-supplied `aria-controls` must reference an existing `id`
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

Identified in the August 2026 accessibility audit:

- `Navbar.Toggle` renders no `aria-expanded`: React-Bootstrap 1.6.5 conveys the collapsed state only via a CSS class, so screen reader users are not told whether activating the toggle opened or closed the menu, nor that it is a disclosure at all.
- `Navbar.Brand` defaults to `as: 'nav'` in Paragon's wrapper, so `<Navbar.Brand href="...">` renders a `<nav href="...">` element instead of the `<a>` React-Bootstrap would choose — the brand stops being a functioning, focusable link, and an unnamed `navigation` landmark is nested inside the navbar. Work around it with an explicit `as` prop (see Application responsibilities).
- Paragon has no unit tests for `Navbar`; behavior is inherited from React-Bootstrap 1.6.5 without local assertions.
