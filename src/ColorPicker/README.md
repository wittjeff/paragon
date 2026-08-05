---
title: 'ColorPicker'
type: 'component'
components:
- ColorPicker
categories:
- Imagery & Iconography
status: 'In progress'
designStatus: 'In progress'
devStatus: 'In progress'
notes: |
---

### Basic Usage

```jsx live
() => {
  const [colorSm, setColorSm] = useState('')
  const [colorMd, setColorMd] = useState('')

  return (
  <div className="d-flex bg-light p-2" style={{ columnGap: '1rem' }}>
    <ColorPicker color={colorSm} setColor={setColorSm} size="sm" />
    <ColorPicker color={colorMd} setColor={setColorMd} size="md" />
  </div>
  )
}
```
### With good and malformatted default color passed

```jsx live

() => {
  const [color, setColor] = useState('#32116c')
  const [colorBad, setColorBad] = useState('#badcolor')

  return (
  <div className="d-flex bg-light p-2" style={{ columnGap: '1rem' }}>
    <ColorPicker color={color} setColor={setColor} />
    <ColorPicker color={colorBad} setColor={setColorBad} />
  </div>
  )
}
```


## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`ColorPicker` is a swatch button that opens a popup containing a two-dimensional color area, a hue slider (both from [react-colorful](https://github.com/omgovich/react-colorful)), and a hex text field. No [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) exists for color pickers; the two picker controls follow the [APG slider pattern](https://www.w3.org/WAI/ARIA/apg/patterns/slider/) (with deviations noted below), and the popup traps focus like a dialog without exposing dialog semantics (see Known issues).

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | On the swatch button: opens the popup (native button activation). |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | In the popup: cycles through the color area, the hue slider, and the hex input. Focus is trapped in the popup while it is open. |
| <kbd>Arrow Left</kbd> / <kbd>Arrow Right</kbd> | On the color area: decreases/increases saturation by 5%. On the hue slider: decreases/increases hue by 5% of the range (18°). |
| <kbd>Arrow Up</kbd> / <kbd>Arrow Down</kbd> | On the color area: increases/decreases brightness by 5%. On the hue slider: no effect (see Known issues). |
| <kbd>Escape</kbd> | Closes the popup and returns focus to the swatch button. |

When the popup opens, focus moves into it automatically (first stop: the color area). Focus trapping,
focus return, and <kbd>Escape</kbd>/outside-click dismissal are provided by `ModalPopup` via
[react-focus-on](https://github.com/theKashey/react-focus-on).

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Swatch trigger | `<button type="button">` | Rendered with no text and no label — its accessible name is empty. See Known issues. |
| Tooltip ("Color picker") | `role="tooltip"` | Shown on hover/focus but not associated with the button — React-Bootstrap 1.6.5 `OverlayTrigger` sets no `aria-describedby`. |
| Color area | `role="slider"`, `tabindex="0"`, `aria-label="Color"`, `aria-valuetext` ("Saturation X%, Brightness Y%") | Provided by react-colorful. No `aria-valuenow`; see Known issues. |
| Hue slider | `role="slider"`, `tabindex="0"`, `aria-label="Hue"`, `aria-valuenow` (0–360), `aria-valuemin`, `aria-valuemax` | Provided by react-colorful. |
| Hex input | `<input>` with auto-generated `id`; `<label>` "Hex" with matching `htmlFor` | Association provided by `Form.Group`. |
| Validation message | `id` registered into the input's `aria-describedby` | "Colors must be in hexadecimal format." — rendered by `Form.Control.Feedback` while the value is invalid. The input never receives `aria-invalid`; see Known issues. |
| Popup container | plain `<div>` | No `role="dialog"` and no accessible name; see Known issues. |

### Screen reader expectations

- The swatch button currently announces as an unnamed button — the tooltip text is not exposed to assistive technology (see Known issues).
- On open, focus lands on the color area, expected to announce as "Color, slider" with the saturation/brightness value text. *(Verification pending per the policy's AT matrix.)*
- The hue slider is expected to announce as "Hue, slider" with its numeric value. *(Verification pending per the policy's AT matrix.)*
- The hex field is expected to announce as "Hex, edit text"; while the value is invalid, the error message should be read as part of its description. *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- The component's API (`color`, `setColor`, `className`, `size`) provides no way to name the swatch button — until that gap is fixed, place visible text immediately before the picker stating what the color controls (e.g. "Banner background color").
- Don't rely on the swatch fill alone to communicate the current selection — display the hex value or a text equivalent nearby (WCAG 1.4.1).
- The validation message exists only while the popup is open; if an invalid or empty color matters at submit time, validate again at the form level.
- If users must enter precise colors, keep the hex field discoverable — the sliders alone don't allow exact entry.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — the swatch trigger (currently failing; see Known issues)
- [Form field has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/) — the hex input
- [Element with role attribute has required states and properties](https://www.w3.org/WAI/standards-guidelines/act/rules/4e8ab6/) — `aria-valuenow` on the color-area slider (currently failing; see Known issues)
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — slider values
- [Focusable element has no keyboard trap](https://www.w3.org/WAI/standards-guidelines/act/rules/80af7b/) — <kbd>Escape</kbd> must always exit the popup's focus trap
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

Identified in the August 2026 accessibility audit:

- The swatch button has an empty accessible name: it renders no text, the tooltip is not programmatically associated (React-Bootstrap 1.6.5 `OverlayTrigger` sets no `aria-describedby`), and the component API offers no prop to supply a label (WCAG 4.1.2).
- The popup is a plain `<div>` with no dialog role or accessible name — nothing announces that a picker has opened beyond focus landing on the "Color" slider.
- The color-area slider has `aria-valuetext` but no `aria-valuenow` (inherited from react-colorful ≤ 5.8) — fails the ARIA `slider` required-property rule.
- The hue slider does not respond to <kbd>Arrow Up</kbd>/<kbd>Arrow Down</kbd> (inherited from react-colorful) — deviation from the APG slider pattern.
- The hex input never receives `aria-invalid` when validation fails — React-Bootstrap 1.6.5 `Form.Control` conveys invalid state via CSS class only; the error text is still exposed through `aria-describedby`.
