---
title: 'ProgressBar'
type: 'component'
components:
- ProgressBar
- ProgressBarAnnotated
categories:
- Status & metadata
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

A bar to indicate the completed progress of a task.

<p>
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/progress/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for documentation.
  </a>
</p>

## Basic Usage

```jsx live
<>
  <ProgressBar now={60} label="60%" variant="primary" />
  <br />
  <ProgressBar now={60} label="60%" variant="success" />
  <br />
  <ProgressBar now={60} label="60%" variant="brand" />
  <br />
  <ProgressBar now={60} label="60%" variant="warning" />
</>
```

## Basic Usage (Inverse Pallete)

```jsx live
<div className="bg-dark-700 p-4">
  <ProgressBar now={60} label="60%" />
  <br />
  <ProgressBar now={60} label="60%" variant="success" />
  <br />
  <ProgressBar now={60} label="60%" variant="brand" />
  <br />
  <ProgressBar now={60} label="60%" variant="warning" />
</div>
```

## Annotated variant

```jsx live
<>
  <ProgressBar.Annotated
    now={20}
    label="20%"
    variant="success"
    threshold={50}
    thresholdLabel="50%"
    thresholdVariant="dark"
    progressHint="Progress"
    thresholdHint="Threshold"
  />
</>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`ProgressBar` renders the ARIA [`progressbar` role](https://www.w3.org/TR/wai-aria-1.1/#progressbar), provided by React-Bootstrap. There is no dedicated [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) for progress bars (the closest relative is the [meter pattern](https://www.w3.org/WAI/ARIA/apg/patterns/meter/)); the role definition itself is the reference. The component renders static, non-interactive content and has no keyboard interaction of its own.

### ARIA roles, states, and properties

Base `ProgressBar` (provided by React-Bootstrap):

| Element | Attribute | Notes |
| --- | --- | --- |
| Bar (`.progress-bar`) | `role="progressbar"` | Inside an unexposed wrapper `<div class="progress">`. |
| Bar | `aria-valuenow` = `now` | Omitted when `now` is not set. |
| Bar | `aria-valuemin` / `aria-valuemax` | `min` / `max` props; default `0` / `100`. |
| Bar | `label` content | Rendered inside the bar; wrapped in `.sr-only` when `srOnly` is set. |

Note that extra props on the base `ProgressBar` (including `aria-label` / `aria-labelledby`) are applied to the **wrapper** `<div>`, not to the `role="progressbar"` element — the progressbar cannot currently be given an accessible name (see Known issues).

`ProgressBar.Annotated` additionally renders:

| Element | Attribute | Notes |
| --- | --- | --- |
| Annotation labels and hints | plain `<span>`s | Visible, but not programmatically associated with the progressbar. |
| Main bar | `role="progressbar"` with an empty `.sr-only` span | The `label` prop is not forwarded to the bar; it renders only as the visual annotation. Extra props (e.g. `aria-label`) *do* reach this element. |
| Threshold bar (`threshold`) | a second `role="progressbar"` | Its `aria-valuenow` is the *difference* `threshold − now`, and it has no accessible name. |

**Deviations identified in the August 2026 accessibility audit:** in the Annotated variant, visible labels are not associated with the progressbar, and the threshold is exposed as a second, unnamed progressbar whose value is a delta rather than the threshold. See Known issues.

### Screen reader expectations

- The base bar announces as a progress bar with its value (e.g. "60 percent"), but with no name saying what the progress measures. *(Verification pending per the policy's AT matrix.)*
- In `ProgressBar.Annotated`, a screen reader encounters two progress bars; the second announces the threshold-minus-progress delta, which has no meaning on its own.
- Changes to `aria-valuenow` are not announced automatically — a progressbar is not a live region.

### Application responsibilities

- Name the bar. For `ProgressBar.Annotated`, pass `aria-label` (it reaches the `role="progressbar"` element). For the base `ProgressBar` there is currently no prop that names the progressbar element itself (see Known issues) — provide adjacent visible text (e.g. a heading) that states what the bar measures.
- Provide the visible `label` (e.g. "60%") so the value is available visually, and don't rely on bar color alone to distinguish progress from threshold — the annotation text carries that distinction.
- Announce completion or important milestones separately (e.g. a `role="status"` message) when they matter; value updates alone are silent.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Element with role attribute has required states and properties](https://www.w3.org/WAI/standards-guidelines/act/rules/4e8ab6/) — `aria-valuenow` on `progressbar`
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — the value attributes
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — `label` text rendered inside the colored bar

### Known issues

Identified in the August 2026 accessibility audit:

- Base `ProgressBar`: pass-through props land on the unexposed wrapper `<div>`, so the `role="progressbar"` element cannot be given an accessible name (inherited from React-Bootstrap 1.6.5).
- `ProgressBar.Annotated`: the visible `label`, `thresholdLabel`, and hint texts are plain spans with no programmatic association to the progressbar; the bar itself receives an empty `.sr-only` span because the `label` prop is not forwarded.
- `ProgressBar.Annotated`: the threshold renders as a second `role="progressbar"` with no accessible name, whose `aria-valuenow` is the difference between threshold and progress — meaningless when announced out of context.
