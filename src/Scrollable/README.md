---

title: 'Scrollable'
type: 'component'
components:
- Scrollable
categories:
- Content
status: 'New'
designStatus: 'In Progress'
devStatus: 'In Progress'
notes: |
  Adds overflow:auto scrolling to wrapped elements. And adds dropshadows when content wrapped within is scrolled up or down

---

## Scrollable area adorned with top and bottom drop shadows on scroll detection

<span>
Shadow is shown on top if you scroll upwards
Shadow is shown on bottom if you scroll downwards

If you don't specify height you will get a default height per the Scrollable component (subject to change)
</span>

## Basic usage of scrollable with provided height

```jsx live
<Scrollable id="test" style={{'height': '40vh'}}>
  <p>Do not use overflow in wrapped component, Scrollable provides overflow:auto around wrapped element</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
  <p>A very long scrollable Content.</p>
</Scrollable>
```

## With padding used by consumer

```jsx live
<Scrollable style={{'height': '40vh'}}>
  <div className="p-4">
    <p>Do not use overflow in wrapped component, Scrollable provides overflow:auto around wrapped element</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
    <p>A very long scrollable Content.</p>
  </div>
</Scrollable>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Scrollable` renders a vertically scrolling container. It is keyboard-reachable (`tabindex="0"`, satisfying [ACT rule 0ssw9k](https://www.w3.org/WAI/standards-guidelines/act/rules/0ssw9k/)), but it currently applies `role="scrollbar"` to the content container itself, which misrepresents the element and its contents to assistive technology — see Known issues before using this component for anything other than plain text content.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> | Focuses the scroll container (it has `tabindex="0"`). |
| <kbd>Arrow Up</kbd> / <kbd>Arrow Down</kbd>, <kbd>Page Up</kbd> / <kbd>Page Down</kbd>, <kbd>Home</kbd> / <kbd>End</kbd> | Native scrolling of the focused container. |

### ARIA roles, states, and properties

What the shipped code renders on the scroll container:

| Element | Attribute | Notes |
| --- | --- | --- |
| Scroll container | `role="scrollbar"` | Misapplied: `scrollbar` is a widget role for a scrollbar control, not for the scrolled content region, and it treats its children as presentational (see Known issues). |
| Scroll container | `aria-valuemin="0"`, `aria-valuemax="100"`, `aria-valuenow` | `aria-valuenow` tracks the scroll position as a percentage. |
| Scroll container | `aria-controls="scrollbar"` | Broken reference: it points at a literal element ID `scrollbar` that this component never renders. |
| Scroll container | `tabindex="0"` | Keyboard-focusable and scrollable. |

The top/bottom shadow edges are CSS pseudo-elements and do not affect the accessibility tree.

### Screen reader expectations

- Because of `role="scrollbar"`, the container is expected to announce as a scrollbar with a value (e.g. "scrollbar, 0"), rather than as a content region — and content inside a role whose children are presentational may be skipped entirely by some assistive technology. *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Until the role misuse is fixed, prefer `Scrollable` only for supplementary plain-text content, and verify with a screen reader that your content remains readable.
- Do not place interactive elements inside without testing: focusable content inside an element with presentational children violates [ACT rule 307n5z](https://www.w3.org/WAI/standards-guidelines/act/rules/307n5z/).
- Give the container a constrained height (as in the examples) so scrollability is real and predictable; ensure content also reflows at 400% zoom (WCAG 1.4.10).

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Scrollable content can be reached with sequential focus navigation](https://www.w3.org/WAI/standards-guidelines/act/rules/0ssw9k/) — passes via `tabindex="0"`
- [Element with role attribute has required states and properties](https://www.w3.org/WAI/standards-guidelines/act/rules/4e8ab6/) — `scrollbar` requires a valid `aria-controls` and `aria-valuenow`
- [Element with presentational children has no focusable content](https://www.w3.org/WAI/standards-guidelines/act/rules/307n5z/) — `scrollbar` treats children as presentational
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-controls` must reference an existing element

### Known issues

Identified in the August 2026 accessibility audit:

- `role="scrollbar"` is applied to the scrolled content container itself. The `scrollbar` role describes a scrollbar *control* operating on a separate element; on the content container it (a) misrepresents the region as a value widget, (b) marks all child content as presentational per the ARIA specification, risking the content being ignored by assistive technology, and (c) makes any focusable child a violation of ACT rule 307n5z (WCAG 1.3.1, 4.1.2).
- `aria-controls="scrollbar"` references a nonexistent element ID, so the role's required relationship is broken (WCAG 4.1.2).
- The container has no accessible name; unlike `OverflowScroll`, no `ariaLabel` prop exists to identify what the region contains.
