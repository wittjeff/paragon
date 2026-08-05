---
title: 'SearchField'
type: 'component'
components:
- SearchField
- SearchFieldAdvanced
- SearchFieldClearButton
- SearchFieldInput
- SearchFieldLabel
- SearchFieldSubmitButton
categories:
- Forms
status: 'Stable'
designStatus: 'Needs Review'
devStatus: 'Done'
notes:
---

Search allows users to quickly find content. The ``Search field`` is made up of the ``Text field`` component and an optional ``Button`` component.

## Basic Usage

```jsx live
<SearchField
  onSubmit={value => console.log(`search submitted: ${value}`)}
/>
```

## With an initial value

```jsx live
<SearchField
  onSubmit={value => console.log(`search submitted: ${value}`)}
  value="foobar"
/>
```

## With a placeholder

```jsx live
<SearchField
  onSubmit={value => console.log(`search submitted: ${value}`)}
  placeholder="foobar"
/>
```

## With callbacks

```jsx live
<SearchField
  onSubmit={value => console.log(`search submitted: ${value}`)}
  onChange={value => console.log(`value changed: ${value}`)}
  onFocus={event => console.log(`input focused: ${event}`)}
  onBlur={event => console.log(`input blurred: ${event}`)}
  onClear={() => console.log('search cleared')}
/>
```

## With a custom label

```jsx live
<SearchField
  onSubmit={value => console.log(`search submitted: ${value}`)}
  label="Search:"
/>
```

## With custom screenreader text

```jsx live
<SearchField
  onSubmit={value => console.log(`search submitted: ${value}`)}
  screenReaderText={{
    label: 'buscar',
    clearButton: 'borrar búsqueda',
    submitButton: 'enviar búsqueda',
  }}
/>
```

## With the submit button outside the input

```jsx live
<SearchField
  submitButtonLocation="external"
  onSubmit={value => console.log(`search submitted: ${value}`)}
/>
```

## Advanced Usage

For needs that deviate from the basic usage above, use `<SearchField.Advanced />`. The `children` elements must contain the `SearchField.Label` and `SearchField.Input` components at a minimum.

## With a custom label

```jsx live
<SearchField.Advanced
  onSubmit={value => console.log(`search submitted: ${value}`)}
>
  <SearchField.Label>Search:</SearchField.Label>
  <SearchField.Input />
  <SearchField.ClearButton />
  <SearchField.SubmitButton />
</SearchField.Advanced>
```

## With an initial value

```jsx live
<SearchField.Advanced
  onSubmit={value => console.log(`search submitted: ${value}`)}
  value="foobar"
>
  <SearchField.Label>Search:</SearchField.Label>
  <SearchField.Input />
  <SearchField.ClearButton />
  <SearchField.SubmitButton />
</SearchField.Advanced>
```

## With a placeholder

```jsx live
<SearchField.Advanced
  onSubmit={value => console.log(`search submitted: ${value}`)}
>
  <SearchField.Label>Search:</SearchField.Label>
  <SearchField.Input placeholder="foobar" />
  <SearchField.ClearButton />
  <SearchField.SubmitButton />
</SearchField.Advanced>
```

## With no clear button

```jsx live
<SearchField.Advanced
  onSubmit={value => console.log(`search submitted: ${value}`)}
>
  <SearchField.Label />
  <SearchField.Input />
  <SearchField.SubmitButton />
</SearchField.Advanced>
```

## With no submit or clear buttons

```jsx live
<SearchField.Advanced
  onSubmit={value => console.log(`search submitted: ${value}`)}
>
  <SearchField.Label />
  <SearchField.Input />
</SearchField.Advanced>
```

## Advanced usage with the submit button outside the input

Use class `pgn__searchfield_wrapper` to group input elements apart from the submit button.

```jsx live
<SearchField.Advanced
  onSubmit={value => console.log(`search submitted: ${value}`)}
  submitButtonLocation="external"
>
  <div className="pgn__searchfield_wrapper">
    <SearchField.Label/>
    <SearchField.Input/>
  </div>
  <SearchField.SubmitButton submitButtonLocation="external" />
</SearchField.Advanced>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`SearchField` renders a `<form role="search">` landmark containing a labeled `role="searchbox"`
input with submit and clear buttons. There is no APG widget pattern for a search field; the
relevant conventions are the [search landmark](https://www.w3.org/WAI/ARIA/apg/patterns/landmarks/examples/search.html)
and native form semantics, which the component follows.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus between the input and whichever buttons are currently rendered. |
| <kbd>Enter</kbd> | In the input: submits the search (calls `onSubmit` with the current value). |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | On the submit or clear button: native button activation. |

Focus management: activating the clear button clears the field and returns focus to the input;
submitting moves focus to the submit button when one is rendered. Note that with the default
`submitButtonLocation="internal"`, the submit icon button is removed once the field contains text
(the clear button takes its place), so submission is then <kbd>Enter</kbd>-only.

### ARIA roles, states, and properties

What the shipped code renders (confirmed by unit test snapshots):

| Element | Attribute | Notes |
| --- | --- | --- |
| Form (`<form>`) | `role="search"` | A search landmark. `aria-label` is set from the `formAriaLabel` prop when provided. |
| Label (`<label>`) | `for` = input id | Renders the `label` prop; without one, a visually hidden span with `screenReaderText.label` (default "search") keeps the input named. |
| Input | `<input type="text">` with `role="searchbox"` | Auto-generated unique id (`pgn-searchfield-input-*`). |
| Submit button (internal) | `<button type="submit">` with `aria-label` = `screenReaderText.submitButton` | Default "submit search"; icon is `aria-hidden`. |
| Submit button (external) | `<button type="submit">` | Visible `buttonText` plus visually hidden `screenReaderText.submitButton`. |
| Clear button | `<button type="reset">` with `aria-label` = `screenReaderText.clearButton` | Default "clear search"; rendered only while the field has a value. |

### Screen reader expectations

- The input announces its label and a search-input role — e.g. "search, search box, edit". The
  landmark lets users jump directly to the search form. *(Verification pending per the policy's AT
  matrix.)*
- The buttons announce as "submit search, button" and "clear search, button" (or your
  `screenReaderText` values). *(Verification pending per the policy's AT matrix.)*
- The clear button appearing/disappearing as the value changes is not itself announced.

### Application responsibilities

- Localize `label` and every `screenReaderText` value (and `buttonText` for external submit): the
  defaults are untranslated English strings (see Known issues).
- Set `formAriaLabel` when a page contains more than one search landmark, so users can tell them
  apart when navigating by landmark.
- Do not rely on `placeholder` as the only visible cue; prefer a visible `label` where the design
  allows.
- In `SearchField.Advanced` compositions, always include `SearchField.Label` — omitting it leaves
  the input unnamed.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Form field has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/) — the search input
- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — submit and clear buttons
- [Visible label is part of accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/2ee8b8/) — the external submit button combines visible text with hidden text
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — placeholder and button text in both `light` and `dark` variants

### Known issues

Identified in the August 2026 accessibility audit (code-verified, not yet filed):

- The default accessible-name strings ("search", "submit search", "clear search", "Search") are
  hardcoded English constants, not routed through Paragon's `react-intl` messages like other
  components — non-English users hear English names unless every consumer passes
  `screenReaderText`/`label` translations.
