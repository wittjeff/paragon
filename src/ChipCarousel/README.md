---
title: 'ChipCarousel'
type: 'component'
components:
- ChipCarousel
categories:
- Content
status: 'New'
designStatus: 'Done'
devStatus: 'Done'
notes: |
---

The ``ChipCarousel`` component creates a scrollable horizontal block of chips with buttons for navigating to the previous and next set of chips.

## Basic Usage

```jsx live
() => {
  const MAX_PERCENTAGE = 100;
  const MAX_FIXED = 1000;
  const [offset, setOffset] = useState(50);
  const [offsetType, setOffsetType] = useState('fixed');
  const [gap, setGap] = useState(3)
  
  const handleChangeOffsetType = (value) => {
    const currentMax = offsetType === 'percentage' ? MAX_PERCENTAGE : MAX_FIXED
    const newMax = value === 'percentage' ? MAX_PERCENTAGE : MAX_FIXED
    const ration = offset / currentMax
    const newOffset = Math.floor(newMax * ration)
    setOffset(newOffset);
    setOffsetType(value);
  }
  
  return (
    <>
      {/* start example form block */}
      <ExamplePropsForm
        inputs={[
          {
            value: offset,
            setValue: setOffset,
            range: {
              min: 0,
              max: offsetType === 'percentage' ? MAX_PERCENTAGE : MAX_FIXED,
              step: offsetType === 'percentage' ? 1 : 50,
            },
            name: 'offset'
          },
          {
            value: offsetType,
            setValue: handleChangeOffsetType,
            options: ['percentage', 'fixed'],
            name: 'offsetType'
          },
          {
            value: gap,
            setValue: setGap,
            range: { min: 0, max: 6, step: 0.5 },
            name: 'gap'
          },
        ]}
      />
      {/* end example form block */}
      <ChipCarousel
        offset={offset}
        offsetType={offsetType}
        ariaLabel="example chip carousel"
        gap={gap}
        items={Array.from({ length: 40 },
          (_, index) => (
            <Chip
              key={`Chip-${index}`}
              onClick={() => console.log(`Chip #${index + 1} clicked`)}
            >
              Chip #{index + 1}
            </Chip>
          )
        )}
      />
    </>
  )
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`ChipCarousel` composes [OverflowScroll](/components/overflowscroll) (a labeled scroll region) with two `IconButton` scroll controls and a row of [Chip](/components/chip) elements. Despite the name, it does not auto-rotate and is closer to a scrollable toolbar of chips than to the [APG carousel pattern](https://www.w3.org/WAI/ARIA/apg/patterns/carousel/); no APG pattern maps to it directly.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves through the visible scroll control(s) and then each interactive chip in order. Focusing a chip scrolls it into view, so all chips are keyboard-reachable even with scrolling disabled. |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | On a scroll control: scrolls the chip row by `offset`. On a chip with an `onClick`: activates it (chips render `role="button"` with a key handler). |

Chips without an `onClick` are static and not focusable. The overflow container itself is deliberately not a tab stop (`tabindex="-1"`, since the chips provide focus access), and manual scrolling is off by default (`canScrollHorizontal` defaults to `false`).

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Scroll region (from `OverflowScroll`) | `role="region"`, `aria-label` = `ariaLabel` prop | `ariaLabel` is required. |
| Previous/next controls | `IconButton` with accessible names | Localized "Scroll to previous" / "Scroll to next" (`react-intl` messages). Each control is only rendered while scrolling in its direction is possible — see Known issues. |
| Overflow container | `tabindex="-1"` | Set because the chips are treated as interactive children. |
| Chips | `role="button"`, `tabindex="0"` | Only when the chip has an `onClick` (provided by `Chip`). |

The start/end opacity masks are purely visual (CSS `mask-image`) and do not affect the accessibility tree, though they do reduce the visible contrast of partially masked chips.

### Screen reader expectations

- Entering the component announces the region with your `ariaLabel`. *(Verification pending per the policy's AT matrix.)*
- The controls announce as "Scroll to previous/next, button"; chips announce as buttons with their text content.

### Application responsibilities

- Provide a meaningful, localized `ariaLabel` describing the chip collection (it is the region's accessible name).
- `ChipCarousel` assumes interactive children: give chips an `onClick` (as in the example above). A carousel of static, non-clickable chips leaves the overflowed chips unreachable by keyboard except via the scroll controls, and its content unreachable by <kbd>Tab</kbd>.
- Chip labels are the accessible names — keep them short and distinct.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Scrollable content can be reached with sequential focus navigation](https://www.w3.org/WAI/standards-guidelines/act/rules/0ssw9k/) — overflowed chips are reached by focusing them
- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — the scroll controls and interactive chips
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — chip text under the start/end opacity masks

### Known issues

- The scroll controls unmount when their direction is exhausted (`!isScrolledToStart` / `!isScrolledToEnd` conditional rendering). A keyboard user activating a control until the row reaches its start/end has the focused button removed from the DOM, dropping focus back to the page body instead of moving it to a sensible neighbor (WCAG 2.4.3 focus order). Identified in the August 2026 accessibility audit.
