---
title: 'Pagination'
type: 'component'
components:
- Pagination
- PaginationReduced
categories:
- Navigation
status: 'Needs Work'
designStatus: 'Done'
devStatus: 'To Do'
notes: |
  TODO: shim from react-bootstrap
  Breaks if given 5000 pages.
  Overly complex implementation.
  Remove baked in english strings
---

Navigation between multiple pages of some set of results. Controls are provided to navigate through multiple pages of related data.

## Default Size

### Uncontrolled Usage

```jsx live
<Pagination
  paginationLabel="pagination navigation"
  pageCount={20}
  onPageSelect={(page) => console.log(`page ${page} selected`)}
/>
```

### Controlled Usage

```jsx live
() => {
  const [currentPage, setCurrentPage] = useState(1);
  
  const handlePageSelect = (page) => setTimeout(() => setCurrentPage(page), 1000);
  
  return (
    <Pagination
      paginationLabel="pagination navigation"
      pageCount={20}
      currentPage={currentPage}
      onPageSelect={(page) => handlePageSelect(page)}
    />
  );
}
```

### Uncontrolled usage with initial page

```jsx live
<Pagination
  paginationLabel="pagination navigation"
  pageCount={20}
  initialPage={5}
  onPageSelect={(page) => console.log(`page ${page} selected`)}
/>
```

### Secondary

```jsx live
<Pagination
  paginationLabel="pagination navigation"
  pageCount={20}
  variant="secondary"
  onPageSelect={(page) => console.log(`page ${page} selected`)}
  icons={{
    leftIcon: ArrowBackIos,
    rightIcon: ArrowForwardIos,
  }}
/>
```

### Reduced

```jsx live
<Pagination
  paginationLabel="pagination navigation"
  pageCount={20}
  variant="reduced"
  onPageSelect={(page) => console.log(`page ${page} selected`)}
/>
```

### Minimal

```jsx live
<Pagination
  paginationLabel="pagination navigation"
  pageCount={5}
  variant="minimal"
  onPageSelect={(page) => console.log(`page ${page} selected`)}
  icons={{
    leftIcon: ArrowBackIos,
    rightIcon: ArrowForwardIos,
  }}
/>
```

## Small Size

### Default variant
```jsx live
<Pagination
  paginationLabel="pagination navigation"
  pageCount={20}
  size="small"
  onPageSelect={(page) => console.log(`page ${page} selected`)}
/>
```

### Secondary (Small Size)

```jsx live
<Pagination
  paginationLabel="pagination navigation"
  pageCount={20}
  variant="secondary"
  size="small"
  onPageSelect={(page) => console.log(`page ${page} selected`)}
/>
```

### Reduced (Small Size)

```jsx live
<Pagination
  paginationLabel="pagination navigation"
  pageCount={20}
  variant="reduced"
  size="small"
  onPageSelect={(page) => console.log(`page ${page} selected`)}
/>
```

### Minimal (Small Size)

```jsx live
<Pagination
  paginationLabel="pagination navigation"
  pageCount={5}
  variant="minimal"
  size="small"
  onPageSelect={(page) => console.log(`page ${page} selected`)}
/>
```

## Inverse Pallete (Default Size)

```jsx live
<div className="bg-dark-700 p-4">
  <Pagination
    paginationLabel="pagination navigation"
    pageCount={20}
    invertColors
    onPageSelect={(page) => console.log(`page ${page} selected`)}
  />
  <Pagination
    paginationLabel="pagination navigation"
    pageCount={20}
    invertColors
    variant="secondary"
    onPageSelect={(page) => console.log(`page ${page} selected`)}
    icons={{
      leftIcon: ArrowBackIos,
      rightIcon: ArrowForwardIos,
    }}
  />
  <Pagination
    paginationLabel="pagination navigation"
    pageCount={20}
    invertColors
    variant="reduced"
    onPageSelect={(page) => console.log(`page ${page} selected`)}
  />
  <Pagination
    paginationLabel="pagination navigation"
    pageCount={5}
    invertColors
    variant="minimal"
    onPageSelect={(page) => console.log(`page ${page} selected`)}
    icons={{
      leftIcon: ArrowBackIos,
      rightIcon: ArrowForwardIos,
    }}
  />
</div>
```

## Inverse Pallete (Small Size)

```jsx live
<div className="bg-dark-700 p-4">
  <Pagination
    paginationLabel="pagination navigation"
    pageCount={20}
    invertColors
    size="small"
    onPageSelect={(page) => console.log(`page ${page} selected`)}
  />
  <Pagination
    paginationLabel="pagination navigation"
    pageCount={20}
    invertColors
    size="small"
    variant="secondary"
    onPageSelect={(page) => console.log(`page ${page} selected`)}
  />
  <Pagination
    paginationLabel="pagination navigation"
    pageCount={20}
    invertColors
    variant="reduced"
    size="small"
    onPageSelect={(page) => console.log(`page ${page} selected`)}
  />
  <Pagination
    paginationLabel="pagination navigation"
    pageCount={5}
    invertColors
    variant="minimal"
    size="small"
    onPageSelect={(page) => console.log(`page ${page} selected`)}
  />
</div>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Pagination` is Paragon's own implementation. There is no dedicated APG pattern for pagination; the component follows the common convention of a labeled `<nav>` landmark containing a list of page buttons, and adds a polite live region that announces the current page.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus through the enabled controls: `Previous`, the visible page buttons, `Next`. Disabled controls (`Previous` on the first page, `Next` on the last) get `tabindex="-1"` plus `disabled` and are skipped. |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Activates the focused button (native button activation). |

Focus management: when activating `Previous` lands you on the first page (or `Next` on the last), the now-disabled button hands focus to its opposite counterpart (`PaginationContext`'s `handlePreviousButtonClick`/`handleNextButtonClick`), so keyboard focus is never stranded on a disabled control.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Container | `<nav>` with `aria-label` = `paginationLabel` prop | The prop is required; there is no baked-in default. |
| Live region | `<div class="sr-only" aria-live="polite" aria-atomic>` | Visually hidden; its text is "`{page}` `{currentPage}`, `{pageOfCount}`" from `buttonLabels` (e.g. "Page 3, Current Page, of 20") and re-announces on every page change. |
| Page list | `<ul class="pagination">` with `<li>` items | |
| Page button | `<button>` with `aria-label` = "Page N" | Labels are built from the `buttonLabels` prop. |
| Current page button | `aria-label` = "Page N, Current Page" | The current page is conveyed through the accessible name (plus button variant styling) — `aria-current` is not rendered. |
| `Previous`/`Next` (default variant) | `<button>` with `aria-label` = "Previous, Page N" / "Next, Page N" | The target page number is included when not on the boundary. |
| `Previous`/`Next` (`reduced`/`minimal`) | icon-only `IconButton` with the same `aria-label`s | The chevron icons themselves are `aria-hidden`. |
| Ellipsis | `<span>` "..." in a disabled `<li>` | Static text, not focusable. |
| Page-of-count (default variant, extra-small viewports) | `<span aria-label="Page N, Current Page, of M">` | The page list collapses to a static "N of M" indicator; the `aria-label` sits on a role-less `<span>`, where ARIA does not permit it (see Known issues). |
| Reduced-variant dropdown | Paragon [Dropdown](/components/dropdown) toggle labeled "N of M" | Opens a list of all pages; inherits Dropdown's `aria-haspopup` deviation. Its hard-coded toggle `id` is `"Pagination dropdown"` (see Known issues). |

### Screen reader expectations

- Entering the component announces the landmark with your `paginationLabel`, e.g. "pagination navigation, navigation". *(Verification pending per the policy's AT matrix.)*
- Page buttons announce as "Page 2, button"; the active page as "Page 1, Current Page, button".
- After a page change, the live region politely announces e.g. "Page 3, Current Page, of 20" without moving focus.

### Application responsibilities

- Supply a meaningful, localized `paginationLabel` (e.g. "search results pages"), and distinct labels if multiple paginations share a page.
- Localize every string in `buttonLabels` — the defaults (`Previous`, `Next`, `Page`, `Current Page`, `of`) are English-only and make up all of the component's announcements.
- When page content updates after `onPageSelect`, ensure the new content is discoverable (e.g. move focus or place the results after the pagination in DOM order); the component only announces the page number.
- If you pass custom `icons`, they are decorative — the accessible names come from `buttonLabels` regardless.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — page, `Previous`, and `Next` buttons
- [ARIA state or property is permitted](https://www.w3.org/WAI/standards-guidelines/act/rules/5c01ea/) — the `aria-label` on the mobile page-of-count `<span>`
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-live`, `aria-label` values
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — tertiary page buttons and the `invertColors` variants

### Known issues

Identified in the August 2026 accessibility audit:

- The mobile page-of-count indicator puts `aria-label` on a role-less `<span>` (`src/Pagination/subcomponents/PageOfCountButton.jsx`), where ARIA labeling is not permitted and is unreliably exposed; the state is still available through the adjacent live region.
- The reduced variant's dropdown toggle uses the hard-coded `id="Pagination dropdown"` (`src/Pagination/subcomponents/PaginationDropdown.jsx`) — an `id` containing a space is invalid HTML, and rendering two reduced paginations duplicates it.
- The effect intended to restore focus to the newly current page button reads `pageButtonRef[currentPage]` instead of `pageButtonRef.current[currentPage]` (`src/Pagination/PaginationContext.jsx`), so it never runs; focus behavior currently relies on the browser keeping focus on the clicked button.
- The current page is conveyed only through the accessible name ("Current Page") rather than `aria-current="page"`; the information is available, but not as a machine-readable state.
