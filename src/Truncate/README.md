---
title: 'Truncate'
type: 'component'
components:
- Truncate
categories:
- Content
status: 'Deprecate Soon'
designStatus: 'Done'
devStatus: 'Done'
notes: |
  Plan to replace with native css implementation as per https://github.com/openedx/paragon/issues/3311
---

A Truncate component can help you crop multiline text. There will be three dots at the end of the text.

## Basic Usage

```jsx live
  <Truncate.Deprecated lines={2}>
    Learners, course teams, researchers, developers: the edX community includes groups with a range of reasons 
    for using the platform and objectives to accomplish. To help members of each group learn about what edX 
    offers, reach goals, and solve problems, edX provides a variety of information resources.
    Learners, course teams, researchers, developers: the edX community includes groups with a range of reasons
    for using the platform and objectives to accomplish. To help members of each group learn about what edX
    offers, reach goals, and solve problems, edX provides a variety of information resources.
  </Truncate.Deprecated>
```

### With the custom ellipsis

```jsx live
    <Truncate.Deprecated lines={2} ellipsis="🎉🎉🎉" whiteSpace>
        Learners, course teams, researchers, developers: the edX community includes groups with a range of reasons
        for using the platform and objectives to accomplish. To help members of each group learn about what edX
        offers, reach goals, and solve problems, edX provides a variety of information resources.
    </Truncate.Deprecated>
```

### With the onTruncate

```jsx live
    <Truncate.Deprecated lines={2} onTruncate={() => console.log('onTruncate')}>
        Learners, course teams, researchers, developers: the edX community includes groups with a range of reasons
        for using the platform and objectives to accomplish. To help members of each group learn about what edX
        offers, reach goals, and solve problems, edX provides a variety of information resources.
    </Truncate.Deprecated>
```

### Example usage in Card

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });
  
  return (
    <Card style={{ width: isExtraSmall ? "100%" : "18rem" }} isClickable>
      <Card.ImageCap
        src="https://picsum.photos/360/200/"
        srcAlt="Card image"
      />
      <Card.Header
        title={
          <Truncate.Deprecated lines={2}>
            Using Enhanced Capabilities In Your Course
          </Truncate.Deprecated>}
      />
      <Card.Section>
        <Truncate.Deprecated lines={4}>
          Learners, course teams, researchers, developers: the edX community includes groups with a range of reasons
          for using the platform and objectives to accomplish. To help members of each group learn about what edX
          offers, reach goals, and solve problems, edX provides a variety of information resources.
        </Truncate.Deprecated>
      </Card.Section>
      <Card.Footer
        textElement={
          <Truncate.Deprecated lines={2}>
            Using Enhanced Capabilities In Your Course
          </Truncate.Deprecated>}
      >
        <Button style={{ minWidth: 100 }}>Action 1</Button>
      </Card.Footer>
    </Card>
  )
}
```

### HTML markdown support

**Note**: `Truncate` supports only plain `HTML` children and not `jsx`.

```jsx live
<Truncate.Deprecated lines={1}>
  <a href="#">Learners</a>, course teams, researchers, developers: the edX community includes <strong class="strong-class"><i class="i-class">groups with <u>a range</u> of <q>reasons</q></i></strong> for using the platform and objectives to accomplish.
</Truncate.Deprecated>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Only `Truncate.Deprecated` renders anything (`Truncate` itself renders `null` and logs a console message). `Truncate.Deprecated` measures the text, rewrites the container's contents with a visually truncated copy, and then exposes the **full, untruncated text** on the container via both `title` and `aria-label`. The component renders static content and has no keyboard interaction of its own; no [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) applies.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Container (`elementType`, default `<div>`) | `aria-label` = full original text | Screen readers get the complete text, not the truncated copy — but naming a generic `<div>` is prohibited by the ARIA specification (see Known issues). |
| Container | `title` = full original text | Mouse-hover tooltip only. |

### Screen reader expectations

- The container is expected to be read using its `aria-label`, i.e. the full text without the ellipsis, where the assistive technology honors a label on a generic element. *(Verification pending per the policy's AT matrix.)*
- The `ellipsis` string (including custom values such as emoji) is visual only and is not expected to be announced.

### Application responsibilities

- The full text has **no keyboard- or touch-accessible path for sighted users**: the `title` tooltip requires a mouse hover. If the truncated information matters (not merely repeated elsewhere), provide another way to read it in full — e.g. an expand control or a link to the full content (WCAG 1.4.10 / 2.1.1 in spirit).
- Keep interactive elements out of truncated content where possible: markup that falls past the crop point is removed from the DOM entirely, so a link may be present at some viewport widths and absent at others.
- Truncation decisions rerun on window resize; verify the component at 400% zoom, where far more text is cropped (WCAG 1.4.10).
- Prefer `elementType` values whose role permits naming (or a CSS `line-clamp` approach — see the deprecation note above) when the ARIA-label behavior matters to you.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [ARIA state or property is permitted](https://www.w3.org/WAI/standards-guidelines/act/rules/5c01ea/) — `aria-label` on a generic element is prohibited
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — applies to the truncated text as rendered

### Known issues

Identified in the August 2026 accessibility audit:

- The full text is exposed via `aria-label` (and `title`) on the container, which is a generic `<div>` by default — WAI-ARIA prohibits naming generic elements, so assistive technology support for this label is not guaranteed, and where it is honored the label *replaces* the container's content, hiding any semantics of inline children (e.g. that part of the text is a link) from the container's announcement (WCAG 4.1.2).
- Sighted keyboard-only and touch users have no way to access the full text; the only affordance is the mouse-only `title` tooltip.
