---
title: 'Stepper'
type: 'component'
status: 'Stable'
components:
- Stepper
- StepperStep
- StepperActionRow
- StepperHeader
- StepperHeaderStep
categories:
- Navigation
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

Steppers display progress through a sequence of separate steps towards a particular goal for more complex tasks. This provides a more manageable experience by allowing the user to focus on smaller pieces of the task at hand.

This stepper can be used not only for initial setup, but also for future configuration changes over time by linking to the relevant step.

## Steps

On page load, all steps will display a number and short label to help the user understand the sequence of actions towards setup or creation.
Steppers should follow a linear progression, requiring the user to navigate between all steps in chronological order for initial setup
Incomplete and active steps will always display a number. Completed steps will display a checkmark icon. Steps missing required input from the user will display the step error state.

## Navigation

The "Next" button, positioned on the right side of the footer, is used to progress to the next step in the process. This button should be active and available at all times. On the final workflow step, this button can display a contextual label related to the action (ex: 'FINISH', or 'APPLY').
When the user has moved past the first step, display a Secondary outline button with a 'Back' label all the way to the left in the footer to allow them to move backwards in a linear progression.
The user can navigate back a single step, or multiple, but can only move forward with the 'Next' button in the footer. (The numbered steps and check marks are not interactive)
The user may move back a step if they have not completed the active step without validation being required for the current step. Any actions that have been taken on the current step should be retained (data should not be cleared unless the user is warned.
Note: Future use cases may require a third action button, positioned directly before the “Next” button, on the right. Examples include saving a draft or launching a preview within the flow.

## Errors and warnings

Display the step error state and do not allow the user to move ahead on any step missing required input from the user. The error state should display a contextual label related to the active step.
Request confirmation from users who choose to exit and discontinue before completing all steps in the workflow. The user should understand that all edits and changes will be lost if they do so.
Display a browser confirmation dialog when closing the tab or navigating with the browser back button before completing all steps in the workflow. The user should understand that all edits and changes will be lost if they proceed.

## Basic Usage

A ``Stepper`` must wrap a set of composed subcomponents:
- ``Stepper.Header``
- ``Stepper.Step``
- ``Stepper.ActionRow``

The order of steps is dictated by the order of ``Stepper.Step`` components in the code.

``Stepper.Step`` and ``Stepper.ActionRow`` are hidden until their ``eventKey`` props match the ``activeKey`` on ``Stepper``.

```jsx live
() => {
  const steps = ['welcome', 'choose-cats', 'review'];
  const [currentStep, setCurrentStep] = useState(steps[0]);

  return (
    <Stepper activeKey={currentStep}>
      <Stepper.Header />

      <Container size="sm" className="py-5">
        <Stepper.Step eventKey="welcome" title="Welcome">
          <h2>Welcome to the cat workflow</h2>
          <HipsterIpsum numParagraphs={2} />
        </Stepper.Step>

        <Stepper.Step eventKey="choose-cats" title="Chooseth a cat">
          <h2>Choose a cat!</h2>
          <HipsterIpsum numParagraphs={2} />
        </Stepper.Step>

        <Stepper.Step eventKey="review" title="Review">
          <h2>Review your work</h2>
          <HipsterIpsum numParagraphs={2} />
        </Stepper.Step>
      </Container>

      <div className="py-3">
        <Stepper.ActionRow eventKey="welcome">
          <Button variant="outline-primary" onClick={() => alert('Cancel')}>
            Cancel
          </Button>
          <Stepper.ActionRow.Spacer />
          <Button onClick={() => setCurrentStep('choose-cats')}>Next</Button>
        </Stepper.ActionRow>

        <Stepper.ActionRow eventKey="choose-cats">
          <Button variant="outline-primary" onClick={() => setCurrentStep('welcome')}>
            Previous
          </Button>
          <Stepper.ActionRow.Spacer />
          <Button onClick={() => setCurrentStep('review')}>Next</Button>
        </Stepper.ActionRow>

        <Stepper.ActionRow eventKey="review">
          <Button variant="outline-primary" onClick={() => setCurrentStep('choose-cats')}>
            Previous
          </Button>
          <Stepper.ActionRow.Spacer />
          <Button onClick={() => alert('Completed')}>Apply</Button>
        </Stepper.ActionRow>
      </div>
    </Stepper>
  )
}
```

## Clickable Header

Use ``Stepper.Step``'s ``onClick`` prop to enable clickable behaviour for step's header. This should primarily be used to
implement navigation between steps by clicking on their headers.

**Note**: this prop takes effect (i.e., header becomes clickable) only after the step has been visited.

### Basic usage

```jsx live
() => {
  const steps = ['introduction', 'benefits', 'finally'];
  const [currentStep, setCurrentStep] = useState(steps[0]);

  return (
    <Stepper activeKey={currentStep}>
      <Stepper.Header />

      <Container size="sm" className="py-5">
        <Stepper.Step onClick={() => setCurrentStep('introduction')} eventKey="introduction" title="Introduction">
          <h2>Introduction</h2>
          <HipsterIpsum numParagraphs={1} />
        </Stepper.Step>

        <Stepper.Step onClick={() => setCurrentStep('benefits')} eventKey="benefits" title="Benefits">
          <h2>Benefits</h2>
          <HipsterIpsum numParagraphs={1} />
        </Stepper.Step>

        <Stepper.Step onClick={() => setCurrentStep('finally')} eventKey="finally" title="Finally!">
          <h2>Finally</h2>
          <HipsterIpsum numParagraphs={1} />
        </Stepper.Step>
      </Container>

      <div className="py-3">
        <Stepper.ActionRow eventKey="introduction">
          <Button variant="outline-primary" onClick={() => alert('Cancel')}>
            Cancel
          </Button>
          <Stepper.ActionRow.Spacer />
          <Button onClick={() => setCurrentStep('benefits')}>Next</Button>
        </Stepper.ActionRow>

        <Stepper.ActionRow eventKey="benefits">
          <Button variant="outline-primary" onClick={() => setCurrentStep('introduction')}>
            Previous
          </Button>
          <Stepper.ActionRow.Spacer />
          <Button onClick={() => setCurrentStep('finally')}>Next</Button>
        </Stepper.ActionRow>

        <Stepper.ActionRow eventKey="finally">
          <Button variant="outline-primary" onClick={() => setCurrentStep('benefits')}>
            Previous
          </Button>
          <Stepper.ActionRow.Spacer />
          <Button onClick={() => alert('Completed')}>Apply</Button>
        </Stepper.ActionRow>
      </div>
    </Stepper>
  )
}
```

### With Error State

```jsx live
() => {
  const steps = ['checkbox', 'success'];
  const [currentStep, setCurrentStep] = useState(steps[0]);
  const [isChecked, check, uncheck, toggleChecked] = useToggle(false);
  const [hasError, setError, removeError] = useToggle(false);
  const [isAlertOpen, openAlert, closeAlert] = useToggle(false)

  const evaluateCheckbox = () => {
    if (isChecked) {
      removeError();
      setCurrentStep('success');
    } else {
      setError();
    }
  };

  const resetCheckbox = () => {
    closeAlert();
    uncheck();
    removeError();
  };

  return (
    <Stepper activeKey={currentStep}>
      <Stepper.Header />

      <AlertModal
        title="Confirm reset"
        isOpen={isAlertOpen}
        onClose={closeAlert}
        footerNode={(
          <ActionRow>
            <Button variant="tertiary" onClick={closeAlert}>Cancel</Button>
            <Button variant="danger" onClick={resetCheckbox}>Confirm</Button>
          </ActionRow>
        )}
      >
        <p>
          Are you sure you wish to reset the checkbox?
        </p>
      </AlertModal>

      <Container size="sm" className="py-5">
        <Stepper.Step
          eventKey="checkbox"
          title="Check the Box"
          index={steps.indexOf('checkbox')}
          description={hasError ? 'Please check the box to continue.' : ''}
          hasError={hasError}
          onClick={() => setCurrentStep('checkbox')}
        >
          <h2>Check the box</h2>
          <Form.Checkbox checked={isChecked} onChange={toggleChecked}>
            Check me!
          </Form.Checkbox>
        </Stepper.Step>

        <Stepper.Step
          eventKey="success"
          title="Success!"
          index={steps.indexOf('success')}
          onClick={evaluateCheckbox}
        >
          <h2>Success!</h2>
          <p>You may now complete this demo.</p>
        </Stepper.Step>
      </Container>

      <div className="py-3">
        <Stepper.ActionRow eventKey="checkbox">
          <Button variant="outline-primary" onClick={openAlert}>
            Reset
          </Button>
          <Stepper.ActionRow.Spacer />
          <Button onClick={() => evaluateCheckbox()}>Next</Button>
        </Stepper.ActionRow>

        <Stepper.ActionRow eventKey="success">
          <Button
            variant="outline-primary"
            onClick={() => setCurrentStep('checkbox')}
          >
            Previous
          </Button>
          <Stepper.ActionRow.Spacer />
          <Button onClick={() => alert('Completed')}>Complete</Button>
        </Stepper.ActionRow>
      </div>
    </Stepper>
  )
}
```

## In a modal

A composition of a stepper with a `FullscreenModal`.

```jsx live
() => {
  const steps = ['welcome', 'choose-cats', 'review'];
  const [currentStep, setCurrentStep] = useState(steps[0]);
  const [isOpen, open, close] = useToggle(false);
  return (
    <>
      <Button variant="outline-primary" onClick={open}>Open fullscreen modal</Button>

      <Stepper activeKey={currentStep}>
        <FullscreenModal
          title="My dialog"
          className="bg-light-200"
          isOpen={isOpen}
          onClose={close}
          beforeBodyNode={<Stepper.Header className="border-bottom border-light" />}
          footerNode={(
            <>
              <Stepper.ActionRow eventKey="welcome">
                <Button variant="outline-primary" onClick={close}>
                  Cancel
                </Button>
                <Stepper.ActionRow.Spacer />
                <Button onClick={() => setCurrentStep('choose-cats')}>Next</Button>
              </Stepper.ActionRow>

              <Stepper.ActionRow eventKey="choose-cats">
                <Button variant="outline-primary" onClick={() => setCurrentStep('welcome')}>
                  Previous
                </Button>
                <Stepper.ActionRow.Spacer />
                <Button onClick={() => setCurrentStep('review')}>Next</Button>
              </Stepper.ActionRow>

              <Stepper.ActionRow eventKey="review">
                <Button variant="outline-primary" onClick={() => setCurrentStep('choose-cats')}>
                  Previous
                </Button>
                <Stepper.ActionRow.Spacer />
                <Button onClick={close}>Apply</Button>
              </Stepper.ActionRow>
            </>
          )}
        >
          <Container size="md">
            <Stepper.Step eventKey="welcome" title="Welcome">
              <h2>Welcome to the cat workflow</h2>
              <HipsterIpsum numParagraphs={2} />
            </Stepper.Step>

            <Stepper.Step eventKey="choose-cats" title="Chooseth a cat">
              <h2>Choose a cat!</h2>
              <HipsterIpsum numParagraphs={2} />
            </Stepper.Step>

            <Stepper.Step eventKey="review" title="Review">
              <h2>Review your work</h2>
              <HipsterIpsum numParagraphs={2} />
            </Stepper.Step>
          </Container>
        </FullscreenModal>
      </Stepper>
    </>
  )
}
```

## Error State

A composition of a stepper with the `hasError` prop. Note that the `index` prop is
also required for steps to rerender correctly here.

```jsx live
() => {
  const steps = ['checkbox', 'success'];
  const [currentStep, setCurrentStep] = useState(steps[0]);
  const [isChecked, check, uncheck, toggleChecked] = useToggle(false);
  const [hasError, setError, removeError] = useToggle(false);
  const [isAlertOpen, openAlert, closeAlert] = useToggle(false)

  const evaluateCheckbox = () => {
    if (isChecked) {
      removeError();
      setCurrentStep('success');
    } else {
      setError();
    }
  };

  const resetCheckbox = () => {
    closeAlert();
    uncheck();
    removeError();
  };

  return (
    <Stepper activeKey={currentStep}>
      <Stepper.Header />

      <AlertModal
        title="Confirm reset"
        isOpen={isAlertOpen}
        onClose={closeAlert}
        footerNode={(
          <ActionRow>
            <Button variant="tertiary" onClick={closeAlert}>Cancel</Button>
            <Button variant="danger" onClick={resetCheckbox}>Confirm</Button>
          </ActionRow>
        )}
      >
        <p>
          Are you sure you wish to reset the checkbox?
        </p>
      </AlertModal>

      <Container size="sm" className="py-5">
        <Stepper.Step
          eventKey="checkbox"
          title="Check the Box"
          index={steps.indexOf('checkbox')}
          description={hasError ? 'Please check the box to continue.' : ''}
          hasError={hasError}
        >
          <h2>Check the box</h2>
          <Form.Checkbox checked={isChecked} onChange={toggleChecked}>
            Check me!
          </Form.Checkbox>
        </Stepper.Step>

        <Stepper.Step
          eventKey="success"
          title="Success!"
          index={steps.indexOf('success')}
        >
          <h2>Success!</h2>
          <p>You may now complete this demo.</p>
        </Stepper.Step>
      </Container>

      <div className="py-3">
        <Stepper.ActionRow eventKey="checkbox">
          <Button variant="outline-primary" onClick={openAlert}>
            Reset
          </Button>
          <Stepper.ActionRow.Spacer />
          <Button onClick={() => evaluateCheckbox()}>Next</Button>
        </Stepper.ActionRow>

        <Stepper.ActionRow eventKey="success">
          <Button
            variant="outline-primary"
            onClick={() => setCurrentStep('checkbox')}
          >
            Previous
          </Button>
          <Stepper.ActionRow.Spacer />
          <Button onClick={() => alert('Completed')}>Complete</Button>
        </Stepper.ActionRow>
      </div>
    </Stepper>
  )
}
```


## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Stepper` is Paragon's own component. The APG has no dedicated stepper/wizard pattern; the closest fit is a step list rendered as an ordinary list, with step state exposed via text and `aria-current="step"`. The shipped implementation renders the list but exposes step **state** only visually — see the deviation and Known issues below.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> / <kbd>Shift + Tab</kbd> | Moves focus through the interactive elements: clickable header steps (if any), the active step's content, and the active `Stepper.ActionRow` buttons. Non-clickable header steps are static text and receive no focus. |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Activates a clickable header step or an action-row button (native button activation). |

A header step is only interactive when its `onClick` prop is set **and** the step has already been visited (and is not the active step); it then renders as a `<button>`. Inactive `Stepper.Step` content and inactive `Stepper.ActionRow`s are unmounted entirely, so hidden steps are never reachable.

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Header step list | `<ul class="pgn__stepper-header-step-list">` | One entry per registered step. |
| Header step (non-clickable) | `<li>` | Contains the number/check bubble, `title`, and `description` as plain text. |
| Header step (clickable) | `<button type="button">` with `aria-label` = "`{title}` step" | Rendered *in place of* the `<li>`, as a direct child of the `<ul>` (see Known issues). The `description` is excluded from the accessible name, and the " step" suffix is hard-coded English. |
| Step separators | `<li aria-hidden="true">` | Decorative connecting lines. |
| Completed-step check icon, error icon | `aria-hidden="true"`, `role="img"` | Hidden from assistive technology with no text alternative (see Known issues). |
| Active/completed/error state | *(CSS classes only)* | `pgn__stepper-header-step-active`, `-complete`, `-has-error`; no `aria-current="step"` or other ARIA state is rendered. |
| Compact header (below `compactWidth`) | single step + "Step X of Y" text | The lone step still renders as an `<li>`, now outside any list (see Known issues); the page count is plain (hard-coded English) text. |
| Active step content (`Stepper.Step`) | `<div class="pgn__stepper-step">` | No role; only the active step renders. |

**Deviation identified in the August 2026 accessibility audit:** which step is active, which are completed, and which have errors is conveyed exclusively by color, icons, and CSS classes. A screen reader user reading the full-width header hears only step numbers, titles, and descriptions — with no indication of progress or errors (WCAG 1.3.1/4.1.2, and use-of-color under 1.4.1).

### Screen reader expectations

- The full-width header reads as a list of step numbers and titles, e.g. "1, Welcome, 2, Chooseth a cat..." with no state information. *(Verification pending per the policy's AT matrix.)*
- The compact header reads the active step's number and title followed by "Step X of Y" — the only place progress is announced.
- A clickable header step announces as "`{title}` step, button".

### Application responsibilities

- Move focus (or at minimum ensure reading order restarts sensibly) when the active step changes — the component performs no focus management, and the previous step's content unmounts under the user. Starting each `Stepper.Step` with a heading, as in the examples above, gives the new step a navigable anchor.
- Because header error states are not announced, surface validation errors inside the step content itself (e.g. with form field feedback or an `Alert`), not only via `hasError`.
- Supply `index` on every `Stepper.Step` when using `hasError` or conditional steps, so header order stays correct.
- Keep `title` (and `description`) concise and localized; remember the compact "Step X of Y" counter and the clickable step's " step" label suffix are currently English-only.
- Provide an always-available forward path via `Stepper.ActionRow` buttons; header-step navigation (`onClick`) is a supplement, not a replacement, since it only works for visited steps.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Button has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/97a4e1/) — clickable header steps and action-row buttons
- [ARIA required owned elements](https://www.w3.org/WAI/standards-guidelines/act/rules/bc4a75/) — the header `<ul>` must own list items, not bare buttons
- [ARIA required context role](https://www.w3.org/WAI/standards-guidelines/act/rules/ff89c9/) — the compact view's `<li>` requires a list parent
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — muted titles/descriptions of inactive steps

### Known issues

Identified in the August 2026 accessibility audit:

- Step states (active, completed, error) are not programmatically exposed: no `aria-current="step"`, and the check/error icons are `aria-hidden` with no text alternative, so progress and errors in the header are invisible to assistive technology.
- A clickable header step replaces its `<li>` with a `<button>` rendered as a direct child of the `<ul>` — invalid list structure that also drops the step's list position ("2 of 3") from what screen readers can report.
- The compact (below `compactWidth`) header renders the active step as an `<li>` with no parent list.
- Clickable header steps wire both `onKeyPress={onClick}` and the native button click, so activating one with <kbd>Enter</kbd> can invoke the `onClick` handler twice.
