---
title: 'ProductTour'
type: 'component'
components:
- ProductTour
categories:
- Overlays
status: 'New'
designStatus: 'Done'
devStatus: 'Done'
---
`ProductTour` displays a sequence of checkpoints with anchors around the DOM.
A `ProductTour` takes a list of tour objects and will only support one enabled tour at a time. If multiple
tours are enabled, `ProductTour` will only render the first enabled in the `tours` list.

`Checkpoints` are rendered in the order they're listed in the checkpoint array.
The checkpoint objects themselves have additional props that can override the props defined in `ProductTour`.

## Usage guidelines
Best practices for ProductTour includes not overloading the user with a large amount of steps. 
Paragon recommends keeping to no more than 5 steps, as well as any overriden button names to be 
descriptive and readable. Also, we recommend using a title at every step or only at the first step for a
consistent and accessible experience.

## Basic Usage

```jsx live
() => {
    const [isTourEnabled, setIsTourEnabled] = useState(false);
    const myFirstTour = {
      tourId: 'myFirstTour',
      advanceButtonText: 'Next',
      backButtonText: 'Back',
      endButtonText: 'Okay',
      enabled: isTourEnabled,
      onDismiss: () => setIsTourEnabled(false),
      onEnd: () => setIsTourEnabled(false),
      dismissAltText: '',
      checkpoints: [
        {
          advanceButtonText: 'Onward', // Override the default advanceButtonText above
          body: "Here's the first checkpoint!",
          placement: 'top',
          target: '#checkpoint-1',
          title: 'First checkpoint',
          dismissAltText: '',
        },
        {
          body: "Here's the second checkpoint!",
          onDismiss: () => {
            console.log('Dismissed the second checkpoint');
            setIsTourEnabled(false);
          }, // Override the default onDismiss above
          placement: 'right',
          target: '#checkpoint-2',
          title: 'Second checkpoint',
          backButtonText: 'Rewind', 
        },
        {
          body: "The third checkpoint without a title",
          placement: 'bottom',
          target: '#checkpoint-3',
          onEnd: () => {
            console.log('Ended the third checkpoint');
            setIsTourEnabled(false);
          } // Additional logic for the onEnd callback to be called on the last checkpoint
        }
      ],
    };

    return (
      <>
        <ProductTour
          tours={[myFirstTour]}
        />
        <Button onClick={() => setIsTourEnabled(true)}>Open a product tour</Button>
        <Row className="w-100 m-0 mt-3 p-2 justify-content-around">
          <div id="checkpoint-1"><Icon src={Check}/></div>
          <div id="checkpoint-2"><Icon src={Check}/></div>
          <div id="checkpoint-3"><Icon src={Check}/></div>
        </Row>
      </>
    );
}
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`ProductTour` renders one `Checkpoint` at a time as a **non-modal** `role="dialog"` anchored to a target element. There is no APG pattern for product tours; the closest reference is the [APG dialog pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/) in its non-modal form — background content stays reachable and there is no focus trap.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus through the checkpoint's controls (dismiss, back, advance/end). Focus is **not** trapped — it continues into the underlying page. |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Activates the focused control (native button activation). |
| <kbd>Escape</kbd> | Ends the tour and calls `onEscape`. Handled by a document-level listener, so it works wherever focus is. |

When a checkpoint opens (and on each advance/back), focus moves automatically to its advance/end button, and the page scrolls the checkpoint into view. When the tour is dismissed, ended, or escaped, focus is **not** restored anywhere — see Known issues.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Checkpoint container (`#pgn__checkpoint`) | `role="dialog"` | Non-modal; `aria-modal` is correctly absent. |
| Checkpoint container | `aria-labelledby="pgn__checkpoint-title"` | The referenced `h2` is rendered **only when the checkpoint has a `title`** — the reference dangles otherwise (see Known issues). |
| Checkpoint title | `<h2 id="pgn__checkpoint-title">` | Rendered from the `title` prop. |
| Step position | visible text "\{step\} of \{totalSteps\}" | Localized; plus visually hidden "Top of step N" / "Bottom of step N" boundary markers at either end of the dialog. |
| Dismiss icon button | `<button>` with `aria-label` = `dismissAltText` | Defaults to a localized "Close tour"; asserted in `Checkpoint.test.jsx`. |
| Back / advance / end buttons | `<button>` | Accessible names come entirely from `backButtonText` / `advanceButtonText` / `endButtonText` (see Known issues). |

### Screen reader expectations

- When a checkpoint opens, focus lands on the advance/end button, so users hear the dialog context and the button — e.g. "First checkpoint, dialog, Next, button" when a `title` is set. *(Verification pending per the policy's AT matrix.)*
- The visually hidden "Top of step N" / "Bottom of step N" markers signal the checkpoint's boundaries when reading linearly. *(Verification pending per the policy's AT matrix.)*
- Without a `title`, the dialog has no accessible name and its announcement is unpredictable across screen readers.

### Application responsibilities

- Give **every** checkpoint a `title`. Without one the dialog's `aria-labelledby` points at nothing and the dialog is unnamed (note: this page's older suggestion that a title "only at the first step" is acceptable predates the August 2026 accessibility audit).
- Always provide non-empty, descriptive `advanceButtonText`, `backButtonText`, and `endButtonText` (at tour or checkpoint level) — the defaults are empty strings, which produce buttons with no accessible name.
- Restore focus in your `onDismiss` / `onEnd` / `onEscape` handlers (e.g. back to the element that started the tour); the component does not do it.
- Keep checkpoint `body` content non-essential and concise; the tour is easy to dismiss and is not persistent UI.
- Provide a localized `dismissAltText` when your app is not using Paragon's bundled translations.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — dismiss, back, and advance/end buttons
- [Role attribute has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/674b10/) — `role="dialog"`
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — the `aria-labelledby` reference
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/) — checkpoint controls
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — checkpoint text and step indicator

### Known issues

Identified in the August 2026 accessibility audit:

- `aria-labelledby="pgn__checkpoint-title"` is always rendered, but the `h2` it references exists only when the checkpoint has a `title`; untitled checkpoints are dialogs with a dangling label reference and no accessible name (WCAG 4.1.2).
- Focus is not restored when the tour is dismissed, ended, or escaped — it falls back to the document body, disorienting keyboard and screen reader users (WCAG 2.4.3).
- `advanceButtonText`, `backButtonText`, and `endButtonText` default to empty strings and are not required, so nothing prevents rendering tour controls with empty accessible names (WCAG 4.1.2).
