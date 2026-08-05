---
title: 'SelectableBox'
type: 'component'
components:
- SelectableBox
- SelectableBoxSet
categories:
- Forms
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |
---

A box that has selection states. It can be used as an alternative to a radio button or checkbox set.

The ``SelectableBox`` can contain any kind of content as long as it is not clickable. In other words, there should be no clickable targets distinct from selection.

## Basic Usage

As ``Checkbox``

```jsx live
() => {
  const type = 'checkbox';
  const allCheeseOptions = ['swiss', 'cheddar', 'pepperjack'];
  const [checkedCheeses, { add, remove, set, clear }] = useCheckboxSetValues(['swiss']);

  const handleChange = (e) => {
    e.target.checked ? add(e.target.value) : remove(e.target.value);
  };
  
  const isInvalid = () => checkedCheeses.includes('swiss');
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });
  
  return (
    <SelectableBox.Set
      className="bg-light-200 p-3"
      value={checkedCheeses}
      type={type}
      onChange={handleChange}
      name="cheeses"
      columns={isExtraSmall ? 1 : 2}
      ariaLabel="cheese selection"
    >
      <SelectableBox value="swiss" type={type} aria-label="swiss checkbox">
        <h3>It is my first SelectableBox</h3>
        <p>Swiss</p>
      </SelectableBox>
      <SelectableBox value="cheddar" inputHidden={false} type={type} aria-label="cheddar checkbox">
        <h3>Cheddar</h3>
      </SelectableBox>
      <SelectableBox
        value="pepperjack"
        inputHidden={false}
        type={type}
        isInvalid={isInvalid()}
        aria-label="pepperjack checkbox"
      >
        <h3>Pepperjack</h3>
      </SelectableBox>
    </SelectableBox.Set>
  );
}
```

## As Radio

```jsx live
() => {
  const [value, setValue] = useState('green');
  const handleChange = (e) => setValue(e.target.value);
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <SelectableBox.Set
      value={value}
      onChange={handleChange}
      name="colors"
      columns={isExtraSmall ? 1 : 3}
      ariaLabel="color selection"
    >
      <SelectableBox value="red" aria-label="red checkbox">
        <h3>It is Red color</h3>
        <p>Select me</p>
      </SelectableBox>
      <SelectableBox value="green" inputHidden={false} aria-label="green radio-button">
        <h3>Green</h3>
        <p>Leaves and grass</p>
      </SelectableBox>
      <SelectableBox value="blue" inputHidden={false} aria-label="blue radio-button">
        <h3>Blue</h3>
        <p>The sky</p>
      </SelectableBox>
    </SelectableBox.Set>
  );
}
```

## As Checkbox
As ``Checkbox`` with ``isIndeterminate``

```jsx live
() => {
  const type = 'checkbox';
  const allCheeseOptions = ['swiss', 'cheddar', 'pepperjack'];
  const [checkedCheeses, { add, remove, set, clear }] = useCheckboxSetValues(['swiss']);

  const allChecked = allCheeseOptions.every(value => checkedCheeses.includes(value));
  const someChecked = allCheeseOptions.some(value => checkedCheeses.includes(value));
  const isIndeterminate = someChecked && !allChecked;
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.small.maxWidth });

  const handleChange = (e) => {
    e.target.checked ? add(e.target.value) : remove(e.target.value);
  };

  const handleCheckAllChange = ({ checked }) => {
    checked ? set(allCheeseOptions) : clear();
  };

  return (
    <>
      <SelectableBox
        className="mb-3"
        checked={allChecked}
        isIndeterminate={isIndeterminate}
        onClick={handleCheckAllChange}
        inputHidden={false}
        type={type}
        aria-label="all options checkbox"
      >
        All the cheese
      </SelectableBox>
      <SelectableBox.Set
        value={checkedCheeses}
        type={type}
        onChange={handleChange}
        name="cheeses"
        columns={isExtraSmall ? 1 : 3}
        ariaLabel="cheese selection"
      >
        <SelectableBox value="swiss" type={type} aria-label="swiss checkbox">
          <h3>It is my first SelectableBox</h3>
          <p>Swiss</p>
        </SelectableBox>
        <SelectableBox value="cheddar" inputHidden={false} type={type} aria-label="cheddar checkbox">
          <h3>Cheddar</h3>
        </SelectableBox>
        <SelectableBox value="pepperjack" inputHidden={false} type={type} aria-label="pepperjack checkbox">
          <h3>Pepperjack</h3>
        </SelectableBox>
      </SelectableBox.Set>
    </>
  );
}
```

As ``Checkbox`` with ``ariaLabelledby``

```jsx live
() => {
  const type = 'checkbox';
  const allCheeseOptions = ['swiss', 'cheddar', 'pepperjack'];
  const [checkedCheeses, { add, remove, set, clear }] = useCheckboxSetValues(['swiss']);

  const handleChange = (e) => {
    e.target.checked ? add(e.target.value) : remove(e.target.value);
  };
  
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });
  
  return (
    <Stack className="bg-light-200 p-3">
      <h3 id="cheese selection" className="mb-4">
        Select your favorite cheese:
      </h3>
      <SelectableBox.Set
        value={checkedCheeses}
        type={type}
        onChange={handleChange}
        name="cheeses"
        columns={isExtraSmall ? 1 : 3}
        ariaLabelledby="cheese selection"
      >
        <SelectableBox value="swiss" inputHidden={false} type={type} aria-label="swiss checkbox">
          <h3>Swiss</h3>
        </SelectableBox>
        <SelectableBox value="cheddar" inputHidden={false} type={type} aria-label="cheddar checkbox">
          <h3>Cheddar</h3>
        </SelectableBox>
        <SelectableBox value="pepperjack" inputHidden={false} type={type} aria-label="pepperjack checkbox">
          <h3>Pepperjack</h3>
        </SelectableBox>
      </SelectableBox.Set>
    </Stack>
  );
}
```

## Without active outline
The `showActiveBoxState` property only affects `SelectableBox` that have `inputHidden` set to `false`.
If a component has no input, the border is always rendered in an active state.

```jsx live
() => {
  const [value, setValue] = useState('apples');
  const handleChange = (e) => setValue(e.target.value);
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <SelectableBox.Set
      value={value}
      onChange={handleChange}
      name="fruits"
      columns={isExtraSmall ? 1 : 3}
      ariaLabel="fruits selection"
    >
      <SelectableBox
        value="apples"
        inputHidden={false}
        showActiveBoxState={false}
        aria-label="apple radio-button"
      >
        <h3>Apples</h3>
      </SelectableBox>
      <SelectableBox
        value="oranges"
        inputHidden={false}
        showActiveBoxState={false}
        aria-label="orange radio-button"
      >
        <h3>Oranges</h3>
      </SelectableBox>
      <SelectableBox
        value="bananas"
        inputHidden={false}
        showActiveBoxState={false}
        aria-label="banana radio-button"
      >
        <h3>Bananas</h3>
      </SelectableBox>
    </SelectableBox.Set>
  );
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`SelectableBox` presents itself visually as a radio/checkbox alternative, but what it renders is a
focusable `role="button"` `<div>` wrapping a hidden-by-default native input. It does **not**
implement the [APG radio group](https://www.w3.org/WAI/ARIA/apg/patterns/radio/) or
[checkbox](https://www.w3.org/WAI/ARIA/apg/patterns/checkbox/) pattern its appearance suggests —
see the deviations below.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus to the next / previous box; every box is its own tab stop (`tabindex="0"`). |
| <kbd>Enter</kbd> | Toggles/selects the focused box (covered by unit tests). |
| <kbd>Space</kbd> | Not explicitly handled: activation is bound to the deprecated `keypress` event with no key filtering and no `preventDefault`, so behavior varies by browser and the page may scroll. |

Arrow keys do not move between boxes: the underlying radio inputs are hidden with `tabindex="-1"`,
so native radio-group arrow navigation is unavailable — a deviation from the APG radio pattern.

### ARIA roles, states, and properties

What the shipped code renders (confirmed by unit test snapshots):

| Element | Attribute | Notes |
| --- | --- | --- |
| `SelectableBox.Set` container (`<div>`) | `role="radiogroup"` (type `radio`, the default) or `role="group"` (type `checkbox`) | Rendered via `Form.RadioSet` / `Form.CheckboxSet`. |
| `SelectableBox.Set` container | `aria-label` or `aria-labelledby` | From the required `ariaLabel` / `ariaLabelledby` props. |
| Each box (`<div>`) | `role="button"`, `tabindex="0"` | No `aria-checked` or `aria-pressed` — selection state is conveyed by CSS classes only. |
| Inner control | `<input type="radio">` or `<input type="checkbox">`, `tabindex="-1"` | With the default `inputHidden`, the input has the `hidden` attribute and is removed from the accessibility tree. |

**Deviations from the APG patterns (identified in the August 2026 accessibility audit):**

- Boxes expose `role="button"` with no state attribute. With `inputHidden` (the default) the native
  input is also hidden from assistive technology, so **whether a box is selected is not perceivable
  non-visually** (WCAG 4.1.2).
- The set announces itself as a `radiogroup`/`group` whose owned elements are buttons, not
  radios/checkboxes — a required-owned-elements violation and a misleading interaction promise.
- Extra props (including `aria-label`) are spread onto **both** the box `<div>` and the inner
  input; passing `id` this way produces duplicate ids.

### Screen reader expectations

- A focused box announces its `aria-label` (or flattened content) and "button" — with no indication
  of whether it is selected. *(Verification pending per the policy's AT matrix; this gap is the
  primary audit finding above.)*
- With `inputHidden={false}`, the visible radio/checkbox exists in the accessibility tree as a
  separate, unlabeled control that focus never reaches. *(Verification pending per the policy's AT
  matrix.)*

### Application responsibilities

- Always supply `ariaLabel` or `ariaLabelledby` on `SelectableBox.Set`, and an `aria-label` on each
  `SelectableBox` — content inside `role="button"` (headings, paragraphs) is flattened into a
  single unstructured name.
- Do not place links, buttons, or other interactive elements inside a box (the component's own
  documentation above also requires this): nested interactive content inside `role="button"` is
  invalid and unreachable by keyboard.
- Until the state-exposure gap is fixed, reflect the current selection in visible text elsewhere on
  the page (e.g. a summary line) so the selection is available to assistive technology, or prefer
  `Form.RadioSet` / `Form.CheckboxSet` when the visual box treatment is not essential.
- Headings inside boxes (`<h3>` in the examples) do not create navigable structure; keep the page's
  real heading outline outside the boxes.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — each box exposes `role="button"`
- [ARIA required owned elements](https://www.w3.org/WAI/standards-guidelines/act/rules/bc4a75/) — `radiogroup` must own `radio` elements, not buttons
- [Element with presentational children has no focusable content](https://www.w3.org/WAI/standards-guidelines/act/rules/307n5z/) — the visible input rendered inside `role="button"` when `inputHidden={false}`
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-labelledby` references on the set
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

Identified in the August 2026 accessibility audit (code-verified, not yet filed):

- Selection state is not exposed to assistive technology: `role="button"` carries no
  `aria-checked`/`aria-pressed`, and the default-hidden input is outside the accessibility tree.
- `role="radiogroup"` / `role="group"` containers own `role="button"` children instead of
  radios/checkboxes; arrow-key group navigation is unavailable.
- Keyboard activation relies on the deprecated `keypress` event with no key filtering and no
  `preventDefault` (only <kbd>Enter</kbd> is asserted in tests).
- Extra props, including `aria-label`, are duplicated onto both the box and its inner input.
