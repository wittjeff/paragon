---
title: 'Breadcrumbs'
type: 'component'
components:
- Breadcrumb
categories:
- Navigation
status: 'Needs Work'
designStatus: 'DONE'
devStatus: 'To Do'
notes: |
  Replace with react-bootstrap shim
---

Use as a secondary navigation pattern to help convey hierarchy and enable navigation.

## Basic Usage

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <Breadcrumb ariaLabel="Breadcrumb basic"
      links={[
        { label: 'Link 1', href: '#link-1' },
        { label: 'Link 2', href: '#link-2' },
        { label: 'Link 3', href: '#link-3' },
      ]}
      isMobile={isExtraSmall}
    />
  )
}
```

### Basic Usage (Mobile View)

```jsx live
<Breadcrumb ariaLabel="Breadcrumb mobile view"
  links={[
    { label: 'Link 1', href: '#link-1' },
    { label: 'Link 2', href: '#link-2' },
    { label: 'Link 3', href: '#link-3' },
  ]}
  isMobile
/>
```

### Basic Usage (Inverse Palette)

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <div className="bg-dark-700 p-4">
      <Breadcrumb ariaLabel="Breadcrumb inverse pallete"
        links={[
          {label: 'Link 1', href: '#link-1'},
          {label: 'Link 2', href: '#link-2'},
          {label: 'Link 3', href: '#link-3'},
        ]}
        variant="dark"
        isMobile={isExtraSmall}
      />
    </div>
  )
}
```

## With custom link element

By default `Breadcrumb` uses `a` tag to render breadcrumbs, which may not always suit your needs. 
This behaviour can be customized with `linkAs` prop, the example below uses Gatsby's `Link` component, but it would also work with [react-router's `Link`](https://reactrouter.com/en/main/components/link) component as well because they share required parts of the component API.

Note that `links` list contains objects with different keys compared to the example above, specifically `href` key is replaced with `to`, that's because Gatsby's `Link` expects its destination to be set through `to` prop (same as react-router's `Link`), internally `Breadcrumb` passes down these objects (except `label` attribute) as props to the `linkAs` element. 

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <Breadcrumb ariaLabel="Breadcrumb basic"
      links={[
        { label: 'Home', to: '/' },
        { label: 'CSS Utilities', to: '/foundations/css-utilities' },
      ]}
      isMobile={isExtraSmall}
      linkAs={GatsbyLink}
    />
  )
}
```

## With active label

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <Breadcrumb ariaLabel="Breadcrumb is active"
      links={[
        { label: 'Link 1', href: '#link-1' },
        { label: 'Link 2', href: '#link-2' },
        { label: 'Link 3', href: '#link-3' },
      ]}
      activeLabel="Link 4"
      isMobile={isExtraSmall}
    />
  )
}
```

## With custom spacer

```jsx live
() => {
  const isExtraSmall = useMediaQuery({ maxWidth: breakpoints.extraSmall.maxWidth });

  return (
    <Breadcrumb ariaLabel="Breadcrumb custom spacer"
      links={[
        { label: 'Link 1', href: '#link-1' },
        { label: 'Link 2', href: '#link-2' },
        { label: 'Link 3', href: '#link-3' },
      ]}
      spacer={<span className="custom-spacer">/</span>}
      isMobile={isExtraSmall}
    />
  )
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Breadcrumb` is Paragon's own implementation and follows the [APG breadcrumb pattern](https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/): a labeled `<nav>` landmark containing an ordered list of links, with `aria-current="page"` available for the final, non-link crumb.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus through the breadcrumb links in order. |
| <kbd>Enter</kbd> | Activates the focused link (native link behavior). |

Breadcrumb adds no keyboard handling of its own; interaction is the browser's native link behavior (or that of the component passed to `linkAs`).

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Container | `<nav>` with `aria-label` = `ariaLabel` prop | Defaults to `"breadcrumb"` (untranslated English). |
| List | `<ol>` with `<li>` items | Ordered list conveys the trail's sequence and length. |
| Each crumb | `<a>` (or the `linkAs` element) | The `label` value is the link text. |
| Spacer | `<li role="presentation">` | Separator chevron; the icon inside is additionally `aria-hidden="true"`. |
| Active crumb (`activeLabel` prop) | `<li aria-current="page">` | Plain text, not a link; only rendered when `activeLabel` is set. |

In the `isMobile` view only the last link is rendered (followed by a presentational chevron); the list and landmark semantics are unchanged.

### Screen reader expectations

- Entering the component announces the landmark, e.g. "breadcrumb, navigation". *(Verification pending per the policy's AT matrix.)*
- Each crumb announces as a link with its label and list position ("Link 2, link, 2 of 4").
- The `activeLabel` crumb announces as "current page" rather than as a link.

### Application responsibilities

- Localize `ariaLabel` — the default `"breadcrumb"` is English-only.
- Mark the current page: either pass `activeLabel` for a non-link final crumb, or, if the final crumb is a link to the current page, include `'aria-current': 'page'` in that link's object (all keys in `links` objects are passed through as props).
- When using `linkAs`, supply a component that renders a real anchor (e.g. react-router's `Link`) so link semantics and keyboard behavior are preserved — a non-interactive element (such as a `div`) removes the crumbs from the tab order.
- If more than one `Breadcrumb` appears on a page, give each a distinct `ariaLabel` so the landmarks are distinguishable.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Link has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/) — every crumb link
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-current="page"` on the active crumb
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — muted link styling and the `dark` variant

### Known issues

No code-verified accessibility defects were identified in the August 2026 accessibility audit.
