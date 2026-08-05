---
title: 'Bubble'
type: 'component'
components:
- Bubble
status: 'New'
categories:
- Status & metadata
designStatus: 'Done'
devStatus: 'Done'
notes: |
---

Represents the filled circle with a number of an icon, supporting the usual color variants

## Basic Usage

```jsx live
() => {
  return (
    <>
      <Stack direction="horizontal" gap={4} className="mb-2">
        <Bubble>
          1
        </Bubble>
        <Bubble variant="error">
          1
        </Bubble>
        <Bubble variant="warning">
          1
        </Bubble>
        <Bubble variant="success">
          1
        </Bubble>
      </Stack>
      <Stack direction="horizontal" gap={4} className="mb-2">
        <Bubble>
          <Icon src={Check} />
        </Bubble>
        <Bubble variant="error">
          <Icon src={Check} />
        </Bubble>
        <Bubble variant="warning">
          <Icon src={Check} />
        </Bubble>
        <Bubble variant="success">
          <Icon src={Check} />
        </Bubble>
      </Stack>
    </>
  );
}
```

### Disabled variant

```jsx live
() => {
  return (
    <Stack direction="horizontal" gap={4} className="mb-2">
      <Bubble disabled>
        1
      </Bubble>
      <Bubble disabled>
        <Icon src={Check} />
      </Bubble>
    </Stack>
  );
}
```

### Expandable

In the case of long content use `expandable` prop. It adds padding to `Bubble`. Padding value is configurable through `scss` variables.

```jsx live
() => {
  {/* start example state */}
  const [isExpandable, setIsExpandable] = useState(true);
  {/* end example state */}

  return (
    <>
      {/* start example form block */}
      <ExamplePropsForm
        inputs={[
          { value: isExpandable, setValue: () => setIsExpandable(!isExpandable), name: 'isExpandable' },
        ]}
      />
      {/* end example form block */}
      <Bubble variant="error" expandable={isExpandable}>
        1234
      </Bubble>
    </>
  );
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Bubble` is a small status indicator (a count or an icon). It renders a plain `<div>` with **no ARIA of its own** — every accessibility attribute must be supplied by the consumer (all extra props are spread onto the root element). Paragon's [Tabs](/components/tabs) does exactly this for its notification bubbles, passing `role="status"` and `aria-live="polite"`. No [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) applies. The bubble is not interactive and has no keyboard behavior.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Root `<div>` | *(none by default)* | `role`, `aria-live`, and other attributes pass through — e.g. `role="status"` + `aria-live="polite"` as used by `Tabs`. |
| Icon children | `aria-hidden="true"` on the `svg` (Icon default) | An icon-only bubble exposes no content unless you give the `Icon` a `screenReaderText`. |
| Root `<div>` | `disabled` prop | Adds a CSS class only; nothing is conveyed programmatically (the bubble is non-interactive, so no `aria-disabled` is warranted). |

### Screen reader expectations

- By default, a bubble is read in place as its bare content (e.g. "1") with no role or context. *(Verification pending per the policy's AT matrix.)*
- With `role="status"` / `aria-live="polite"` supplied, changes to the count are announced politely without moving focus — but only as the bare number unless you add context.

### Application responsibilities

- For dynamic counts, pass `role="status"` (and `aria-live="polite"` for broader support) so updates are announced.
- A bare number is ambiguous — pair it with visually hidden context, e.g. `4<span className="sr-only"> unread notifications</span>`.
- Icon-only bubbles: give the `Icon` a `screenReaderText`, or hide the whole bubble with `aria-hidden="true"` if it is purely decorative.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — count text against each variant background
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — any `aria-live` or other attributes you pass through
