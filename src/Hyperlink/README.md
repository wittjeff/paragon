---
title: 'Hyperlink'
type: 'component'
components:
- Hyperlink
categories:
- Buttonlike
status: 'Needs Work'
designStatus: 'Done'
devStatus: 'Done'
notes: |
  Improve prop naming. Deprecate content prop.
  Use React.forwardRef for ref forwarding.
---

## minimal usage

```jsx live
<Hyperlink destination="https://www.edx.org">
  edX.org
</Hyperlink>
```

## with blank target

```jsx live
<Hyperlink destination="https://www.edx.org" target="_blank">
  edX.org
</Hyperlink>
```

### blank target without Icon

```jsx live
<Hyperlink destination="https://www.edx.org" target="_blank" showLaunchIcon={false}>
  edX.org
</Hyperlink>
```

## with onClick

```jsx live
<Hyperlink
  destination="https://www.edx.org"
  target="_blank"
  onClick={e => {
    e.preventDefault();
    console.log('click');
  }}
>
  edX.org
</Hyperlink>
```

## with icon

```jsx live
<Hyperlink destination="https://www.edx.org">
  <Icon
    id="SampleIcon"
    src={Add}
    className="fa fa-book"
    screenReaderText="Visit edX Home"
  />
</Hyperlink>
```

## color variants

```jsx live
<div className="d-flex flex-column">
    <Hyperlink destination="https://www.edx.org">
      Default
    </Hyperlink>
    
    <Hyperlink variant="muted" destination="https://www.edx.org">
      Muted
    </Hyperlink>
    
    <Hyperlink variant="brand" destination="https://www.edx.org">
      Brand
    </Hyperlink>
</div>
```

## link variants

```jsx live
<div className="row">
  <div className="col-2">
    <Hyperlink destination="https://www.edx.org">
      Standalone
    </Hyperlink>
  </div>
    
  <div className="col-2">
    <Hyperlink isInline destination="https://www.edx.org">
      Inline
    </Hyperlink>
  </div>
</div>
```

## with custom link element (e.g., using a router)

``Hyperlink`` typically relies on the standard HTML anchor tag (i.e., ``a``); however, this behavior may be overriden when the destination link is to an internal route where it should be using routing instead (e.g., ``Link`` from React Router).

```jsx live
<Hyperlink
  as={GatsbyLink}
  to="/components/button"
>
  Button
</Hyperlink>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Hyperlink` renders a native `<a>` element (or the `as` component) — the [APG link pattern](https://www.w3.org/WAI/ARIA/apg/patterns/link/) via native semantics, with no custom keyboard handling.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Enter</kbd> | Follows the link (native `<a>` behavior). |

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Link | `<a href>` = `destination` | Native link role; `target` renders explicitly (`_self` by default). |
| Link with `target="_blank"` | `rel="noopener noreferrer"` | Added (or merged into your `rel`) to prevent reverse tabnabbing. |
| External-link icon (`target="_blank"`, `showLaunchIcon` default) | `aria-hidden="true"` on the `<svg>`; sibling `.sr-only` text | The visually hidden text (default "in a new tab", localized) is rendered inside the anchor, so it joins the link's accessible name. The wrapping span also carries `title` (default "Opens in a new tab"). |

### Screen reader expectations

- Announces the link text plus "in a new tab" (or your `externalLinkAlternativeText`) for external links — e.g. "edX.org in a new tab, link". *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Write link text that describes the destination out of context; the component does not fix vague text like "click here".
- Set `isInline` when the link sits inside body text — the default (standalone) style underlines only on hover, which is insufficient to distinguish an inline link from surrounding text by more than color (WCAG 1.4.1).
- If you disable `showLaunchIcon` on a `target="_blank"` link, provide your own "opens in a new tab" indication.
- When you override `externalLinkAlternativeText`/`externalLinkTitle`, localize both.
- With a custom `as` component, ensure it ultimately renders an anchor with an `href` (the `destination` prop is only applied as `href` when set).

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Link has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/)
- [Link in context is descriptive](https://www.w3.org/WAI/standards-guidelines/act/rules/5effbb/) — consumer-supplied link text
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — `default`, `muted`, and `brand` variants
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

None identified in the August 2026 accessibility audit.
