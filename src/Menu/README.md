---
title: 'Menu'
type: 'component'
components:
- Menu
- MenuItem
categories:
- Navigation
status: 'New'
designStatus: 'Done'
devStatus: 'Done'
notes: ''
---

An arrow-key navigable ``Menu`` which consists of ``MenuItems``. A ``Menu`` can be employed to produce its common variants, including dropdown menus, select menus, and others. ``Menus`` are keyboard navigable with both tab and arrow keys.

## MenuItem

A ``MenuItem`` is its own distinct component that is used by any kind of menu overlays i.e. dropdown menu and navigation menu.

```jsx live
() => {
  return (
    <Menu>
      <MenuItem> A Menu Item</MenuItem>
      <MenuItem iconBefore={Check}>A Menu Item With an Icon Before</MenuItem>
      <MenuItem iconAfter={Check}>A Menu Item With an Icon After </MenuItem>
      <MenuItem disabled>A Disabled Menu Item</MenuItem>
      <MenuItem as={Hyperlink} destination="https://en.wikipedia.org/wiki/Hyperlink">A Link Menu Item</MenuItem>
      <MenuItem as={Button} variant="tertiary" size="inline">A Button Menu Item</MenuItem>
      <MenuItem as={Form.Checkbox}>A Checkbox Menu Item</MenuItem>
    </Menu>
  );
}
```

## With Form

A ``Menu`` can include things like forms.

```jsx live
() => {
  return (
    <Form.Group>
      <Form.CheckboxSet
          name="color-two"
          onChange={(e) => console.log(e.target.value)}
          defaultValue={['green']}
        >
        <Form.Label>Which Color?</Form.Label>
        <Menu>
          <MenuItem as={Form.Checkbox} value="red">Red</MenuItem>
          <MenuItem as={Form.Checkbox} value="blue">Blue</MenuItem>
          <MenuItem as={Form.Checkbox} value="green">Green</MenuItem>
          <MenuItem as={Form.Checkbox} value="yellow">Yellow</MenuItem>
        </Menu>
      </Form.CheckboxSet>
    </Form.Group>
  );
}
```

## With Modal

A ``Menu`` can be implemented to appear inside a `modalpopup` for a wide variety of use cases. The ``Modal`` brings focus to the first menu element upon the click of the trigger, and can be escaped on click away or key press.

```jsx live
() => {
  const [isOpen, open, close] = useToggle(false);
  const [target, setTarget] = React.useState(null);
  const [selected, setSelected] = useState('...');

  return (
    <>
      <Badge variant="secondary">I like {selected}</Badge>
      <Button ref={setTarget} variant="primary" size="inline" onClick={open}>Click Me To Pick:</Button>
      <ModalPopup positionRef={target} isOpen={isOpen} onClose={close} style={{
        width: 500, height: 50
      }}>
      <div className="bg-white">
        <Menu>
          <MenuItem as={Button} variant="tertiary" size="inline" onClick= {()=>setSelected('Beans')}>Beans</MenuItem>
          <MenuItem as={Button} variant="tertiary" size="inline" onClick= {()=>setSelected('Greens')}>Greens</MenuItem>
          <MenuItem as={Button} variant="tertiary" size="inline" onClick= {()=>setSelected('Tomatoes')}>Tomatoes</MenuItem>
          <MenuItem as={Button} variant="tertiary" size="inline" onClick= {()=>setSelected('Potatoes')}>Potatoes</MenuItem>
        </Menu>
      </div>
      </ModalPopup>
    </>
  )
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Menu` is Paragon's own component. Despite its name, it does **not** implement the ARIA `menu`/`menuitem` roles of the [APG menu pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menu/): it renders a plain container of focusable elements (buttons, links, checkboxes) and adds arrow-key focus movement on top of native semantics. `SelectMenu`, which composes `Menu` inside a popup, is covered below.

### Keyboard support

Provided by Paragon's `useArrowKeyNavigation` hook (a document-level keydown listener that only acts while focus is inside the menu):

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus in and out of the menu and between items — every enabled item is in the page tab sequence (no roving tabindex). |
| <kbd>Arrow Down</kbd> / <kbd>Arrow Right</kbd> | Moves focus to the next item, wrapping from last to first. |
| <kbd>Arrow Up</kbd> / <kbd>Arrow Left</kbd> | Moves focus to the previous item, wrapping from first to last. |
| <kbd>Home</kbd> / <kbd>End</kbd> | Moves focus to the first / last item. |
| <kbd>Enter</kbd> | Clicks the focused item. |

Which elements the arrow keys visit is controlled by the `arrowKeyNavigationSelector` prop (default: enabled `a`, `button`, and `input` elements).

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Container (`Menu`) | `<div class="pgn__menu">` | No role (element changeable via `as`). |
| Item (`MenuItem`) | `<button>` by default | Native button semantics; `as` swaps in other elements (`Hyperlink`, `Form.Checkbox`, ...), whose own semantics then apply. |
| Item icons (`iconBefore`/`iconAfter`) | `aria-hidden="true"`, `role="img"` | Decorative; excluded from the accessible name. |

Because no menu roles are rendered, items announce with their native roles — this is a deliberate design, not an oversight, and matches Paragon's [Dropdown](/components/dropdown) treatment.

### SelectMenu

`SelectMenu` renders a trigger `Button` plus a `Menu` inside a `ModalPopup` (focus handling by [react-focus-on](https://github.com/theKashey/react-focus-on)):

- The trigger renders `aria-haspopup="true"` and `aria-expanded` reflecting the open state; its label is the selected item's content, or `defaultMessage` ("Select...") when nothing is selected.
- On open, focus moves into the popup (to the previously selected item, otherwise the first item) and is trapped there; <kbd>Escape</kbd> or clicking outside closes it; after closing, focus returns to the trigger.
- Each item is cloned with `role="link"`, and the selected item additionally gets `aria-current="page"` — even though the items are value choices rendered as `<button>` elements. See the deviation below.

**Deviation from the APG pattern:** `SelectMenu` behaves as a value selector, but it exposes neither the [listbox/combobox semantics](https://www.w3.org/WAI/ARIA/apg/patterns/combobox/) (`role="listbox"`/`option` with `aria-selected`) nor consistent link semantics: the trigger's `aria-haspopup="true"` announces a "menu" that has no `role="menu"`, items are buttons overridden with `role="link"`, and selection is conveyed with `aria-current="page"`, which describes the current *page* in a navigation set, not a chosen value. The `aria-label="Select Menu"` placed on the inner role-less `<div>` has no effect (labels are not supported on generic elements). Identified in the August 2026 accessibility audit; see Known issues.

### Screen reader expectations

- `Menu` items announce with their native role and text, e.g. "Beans, button" — there is no "menu" context announcement. *(Verification pending per the policy's AT matrix.)*
- The `SelectMenu` trigger announces its current value plus popup and expanded state, e.g. "Select..., menu button, collapsed"; open items announce as "link", with the selected one as "current page". *(Verification pending; the mixed semantics above may vary noticeably between screen readers.)*

### Application responsibilities

- Ensure every item has a discernible text label; icon-only items would have empty accessible names since the icons are `aria-hidden`.
- When passing custom elements via `as`, use natively focusable, semantic elements — arrow-key navigation only visits elements matched by `arrowKeyNavigationSelector`.
- Do not use `Menu`/`SelectMenu` as a form field; for value selection inside forms use `Form.Control as="select"` or `Form.Autosuggest`.
- Localize `defaultMessage` on `SelectMenu` — the default "Select..." is English-only.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — items and the `SelectMenu` trigger
- [Link has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/) — items rendered as links
- [ARIA state or property is permitted](https://www.w3.org/WAI/standards-guidelines/act/rules/5c01ea/) — `aria-label` on the role-less menu `<div>` is not permitted
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-expanded`, `aria-haspopup`, `aria-current`
- [Focusable element has no keyboard trap](https://www.w3.org/WAI/standards-guidelines/act/rules/80af7b/) — the `SelectMenu` popup's focus trap must keep its <kbd>Escape</kbd> exit
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

Identified in the August 2026 accessibility audit:

- `SelectMenu` misrepresents a value-selection control: `<button>` items are overridden with `role="link"`, the selected value is conveyed as `aria-current="page"` instead of `aria-selected`, and the trigger's `aria-haspopup="true"` promises menu semantics the popup does not expose — a deviation from the APG combobox/listbox pattern.
- `SelectMenu` places `aria-label="Select Menu"` on a role-less `<div>`, where it is ignored by assistive technology (and hard-coded in English).
