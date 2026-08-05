---
title: 'Form'
type: 'component'
components:
- Form
- FormGroup
- FormControl
- FormControlFeedback
- FormLabel
- FormText
- FormRow
categories:
- Forms
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

## Floating Labels

```jsx live
<Form>
  <Form.Row>
    <Form.Group as={Col} controlId="formGridEmail">
      <Form.Control
        type="email"
        floatingLabel="Email"
      />
      <Form.Text>
        This is the email that we'll send emails to.
      </Form.Text>
    </Form.Group>

    <Form.Group
      as={Col}
      controlId="formGridPassword"
      isInvalid
    >
      <Form.Control
        type="password"
        floatingLabel="Password"
      />
      <Form.Control.Feedback type="invalid">
        We don't like that password. Try a different one.
      </Form.Control.Feedback>
    </Form.Group>
  </Form.Row>

  <Form.Group
    controlId="formGridAddress1"
    isValid
  >
    <Form.Control floatingLabel="Address" />
    <Form.Control.Feedback type="valid">
      This is a nice place!
    </Form.Control.Feedback>
  </Form.Group>

  <Form.Row>
    <Form.Group as={Col} controlId="formGridCity">
      <Form.Control floatingLabel="City" />
    </Form.Group>

    <Form.Group as={Col} controlId="formGridState">
      <Form.Control floatingLabel="State" as="select">
        <option value="">Choose...</option>
        <option>...</option>
      </Form.Control>
    </Form.Group>

    <Form.Group as={Col} controlId="formGridZip">
      <Form.Control floatingLabel="Zip" />
    </Form.Group>
  </Form.Row>

  <Form.Group controlId="formSwitch">
    <Form.Switch>Check this switch</Form.Switch><br/>
    <Form.Switch disabled>disabled switch</Form.Switch>
  </Form.Group>

  <Form.Group controlId="whichColor">
    <Form.Label>
      Which color would you like?
    </Form.Label>
    <Form.RadioSet name="color">
      <Form.Radio value="red">Red</Form.Radio>
      <Form.Radio value="green">Green</Form.Radio>
      <Form.Radio value="blue">Blue</Form.Radio>
    </Form.RadioSet>
    <Form.Control.Feedback>
      The color you choose.
    </Form.Control.Feedback>
  </Form.Group>

  <Form.Group id="formGridCheckbox">
    <Form.Checkbox>Check me out</Form.Checkbox>
  </Form.Group>
  <div className="d-flex">
    <Button variant="primary" type="submit">
      Submit
    </Button>
  </div>
</Form>
```



## Regular Labels

```jsx live
<Form>
  <Form.Row>
    <Form.Group as={Col} controlId="formGridEmail-2">
      <Form.Label>Email</Form.Label>
      <Form.Control type="email" placeholder="Enter email" />
      <Form.Text muted>
        This is the email that we'll send emails to.
      </Form.Text>
    </Form.Group>

    <Form.Group isInvalid as={Col} controlId="formGridPassword-2">
      <Form.Label>Password</Form.Label>
      <Form.Control type="password" placeholder="Password" />
      <Form.Control.Feedback type="invalid">
        We don't like that password. Try a different one.
      </Form.Control.Feedback>
    </Form.Group>
  </Form.Row>

  <Form.Group controlId="formGridAddress1-2">
    <Form.Label>Address</Form.Label>
    <Form.Control placeholder="1234 Main St" />
    <Form.Control.Feedback type="valid">
      This is a nice place!
    </Form.Control.Feedback>
  </Form.Group>

  <Form.Row>
    <Form.Group as={Col} controlId="formGridCity-2">
      <Form.Label>City</Form.Label>
      <Form.Control />
    </Form.Group>

    <Form.Group as={Col} controlId="formGridState-2">
      <Form.Label>State</Form.Label>
      <Form.Control as="select" defaultValue="Choose...">
        <option>Choose...</option>
        <option>...</option>
      </Form.Control>
    </Form.Group>

    <Form.Group as={Col} controlId="formGridZip-2">
      <Form.Label>Zip</Form.Label>
      <Form.Control />
    </Form.Group>
  </Form.Row>

  <Form.Group controlId="formSwitch-2">
    <Form.Switch>Check this switch</Form.Switch><br/>
    <Form.Switch disabled>disabled switch</Form.Switch>
  </Form.Group>

  <Form.Group id="formGridCheckbox-2">
      <Form.Checkbox>Check me out</Form.Checkbox>
  </Form.Group>
  <div className="d-flex">
    <Button variant="primary" type="submit">
      Submit
    </Button>
  </div>
</Form>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Paragon forms build on native HTML form elements: `Form` renders a `<form>` (via React-Bootstrap
1.6.5) and `Form.Control` renders a native `<input>`, `<textarea>`, or `<select>`. No single
[APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) covers forms; accessibility here rests on
native semantics plus the label/description wiring that `Form.Group` provides, described below.
Individual field types (checkbox, radio set, switch, autosuggest) are covered on their own pages.

### Keyboard support

All keyboard interaction is the browser's native behavior for the underlying elements. Paragon adds
no custom key handling at the form level; for example, <kbd>Enter</kbd> in a text input submits the
form per the native rules.

### ARIA roles, states, and properties

`Form.Group` provides a context (`FormGroupContext`) that generates and distributes ids to its
children; this association logic is the accessibility core of the family:

| Element | Attribute | Notes |
| --- | --- | --- |
| `Form` | *(none)* | Plain `<form>`; it is exposed as a `form` landmark only if you give it an accessible name. |
| `Form.Control` | `id` = `controlId` | Your explicit `controlId` prop on the group, or an autogenerated id. |
| `Form.Control` | `aria-describedby` | Merged list of every `Form.Control.Feedback` id in the group, plus any value you pass yourself. |
| `Form.Label` | `htmlFor` = `controlId` | Renders a native `<label>` associated with the control. |
| `Form.Control.Feedback` | `id` | Generated (or your explicit id), registered into the control's `aria-describedby`. |
| Grouped controls (`Form.RadioSet` / `Form.CheckboxSet`) | `role="radiogroup"` / `role="group"`, `aria-labelledby` | In this mode `Form.Label` renders as `<p id="...">` instead of `<label>`, and the set references it via `aria-labelledby`. |

Validation state (`isInvalid` / `isValid` on a group) reaches the control and feedback as **styling
only** (`is-invalid` / `is-valid` classes, provided by React-Bootstrap); no `aria-invalid` is
rendered — see Known issues.

### Screen reader expectations

- Focusing a field announces the label, the role ("edit text", "combo box", ...), the current
  value, and then the feedback/help text via `aria-describedby`. *(Verification pending per the
  policy's AT matrix.)*
- Because no `aria-invalid` or live region is rendered, a field entering an error state produces no
  announcement by itself; users encounter error text only when they refocus the field.

### Application responsibilities

- Wrap every field in a `Form.Group` with a `Form.Label` (or a `floatingLabel` on the control).
  Outside a `Form.Group`, no ids are generated and label, control, and feedback are not associated.
- Write feedback text whose meaning is carried by the words — the valid/invalid color and icon are
  visual only (the icon is `aria-hidden`).
- On failed submit, move focus to the first invalid control (or to an error summary) so keyboard
  and screen reader users discover the errors; the components do not do this for you.
- Set `autocomplete` on fields that collect personal data (WCAG 1.3.5); extra props pass through to
  the native element.
- Do not use `placeholder` as the only label.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Form field has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/e086e5/) — every control needs a label
- [Autocomplete attribute has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/73f2c2/)
- [ARIA state or property has valid value](https://www.w3.org/WAI/standards-guidelines/act/rules/6a7281/) — generated `aria-describedby` / `aria-labelledby` references
- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — help and feedback text, including `muted`
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

Identified in the August 2026 accessibility audit:

- `isInvalid` renders only the `is-invalid` styling class on the control; `aria-invalid="true"` is
  not set, so the error state is not exposed to assistive technology (see Form.Control).
- `Form.Control.Feedback` is not a live region, so error text that appears dynamically (e.g. on
  submit) is not announced (see Form.Control.Feedback).
