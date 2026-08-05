---
title: 'PageBanner'
type: 'component'
components:
- PageBanner
categories:
- Status & metadata
status: 'New'
designStatus: 'Complete'
devStatus: 'In Progress'
notes: |
---

A ``Page Banner`` displays an important, succinct message, and provides actions for users to address (or dismiss the banner). It requires a user action to be dismissed.

## Basic Usage Variants

```jsx live
() => {
    const [showPageBanner, setShowPageBanner] = useState(true);
    const [variant, setVariant] = useState('light');

    return (
        <>
          <ExamplePropsForm
            inputs={[
              {
                value: variant, setValue: setVariant, options: [
                  {name: 'light', value: 'light'},
                  {name: 'dark', value: 'dark'},
                  {name: 'accentA', value: 'accentA'},
                  {name: 'accentB', value: 'accentB'},
                  {name: 'warning', value: 'warning'}
                ], name: 'Banner Variants'
              },
            ]}
          />

            <div>
              <PageBanner
                show={showPageBanner}
                variant={variant}
                dismissible
                onDismiss={() => setShowPageBanner(false)}
              >
                Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore.
              </PageBanner>
              {!showPageBanner && (
                <Button className="my-2 mie-2" onClick={() => setShowPageBanner(true)}>Show Page Banner</Button>)}

            </div>
        </>
    )
}
```



## Warning Banner

The system warning banner is similar to the ``Alert Banner`` banner in styling, except that the text is always default body (14px) and padding has been modified in order to accommodate the ``Page Banner`` component. It cannot be dismissed.

```jsx live
() => {
  return (
    <>
      <PageBanner
         variant="warning"
      >
        <Icon src={WarningFilled} className="mie-2"/> We will no longer support Internet Explorer 11.
      </PageBanner>
    </>
  )
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`PageBanner` is a live-region announcement, closest to the [APG alert pattern](https://www.w3.org/WAI/ARIA/apg/patterns/alert/), though it explicitly downgrades the announcement to polite (see the ARIA table).

### Keyboard support

The banner itself is not focusable. With `dismissible`, the dismiss icon button is reached with <kbd>Tab</kbd> and activated with <kbd>Enter</kbd> / <kbd>Space</kbd>.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Banner container | `role="alert"` | Rendered whenever `show` is `true`; with `show={false}` nothing renders. |
| Banner container | `aria-live="polite"` | Explicitly overrides the assertive politeness implied by `role="alert"` — the banner announces without interrupting current speech. |
| Banner container | `aria-atomic="true"` | The whole banner is read on announcement, not just the changed node. |
| Dismiss icon button | `aria-label` = `dismissAltText` | Defaults to the untranslated string "Dismiss" (see Known issues). The button's icon `svg` is `aria-hidden="true"`. |

### Screen reader expectations

- When the banner appears after page load (`show` turns `true`), its full content is announced politely — after the current utterance finishes, without moving focus. *(Verification pending per the policy's AT matrix.)*
- Banners present in the initial page render are read in place as ordinary content, not announced.
- The dismiss button announces as "Dismiss, button" (or your `dismissAltText`).

### Application responsibilities

- Pass a translated `dismissAltText` — the default "Dismiss" is hardcoded English.
- The variant (warning, dark, accents) is conveyed by color alone; include severity in the text when it matters (WCAG 1.4.1). Icons placed in `children` (as in the warning example above) are `aria-hidden` by default, so the text must stand on its own.
- You control `show` — after dismissal, move focus somewhere sensible; the focused dismiss button unmounts with the banner.
- Keep content succinct: it is announced as one atomic block.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — the dismiss icon button
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — `aria-live`, `aria-atomic`
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — banner text against variant backgrounds
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/) — the dismiss button

### Known issues

Identified in the August 2026 accessibility audit:

- The default `dismissAltText` ("Dismiss") is a hardcoded, untranslated string — unlike `Alert`, whose default dismiss label is localized via `react-intl`.
- The dismiss button renders inside the `role="alert"` live region; the APG reserves the alert role for non-interactive content.

