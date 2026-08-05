---
title: 'Annotation'
type: 'component'
components:
- Annotation
categories:
- Status & metadata
status: 'New'
designStatus: 'Done'
devStatus: 'In progress'
notes: |

---

Display informative text related to an object on screen. Unlike the tooltip an annotation is not interactive.

- Don't put links or interactive elements in annotations - use [Popover](/components/popover) instead
- Don’t use annotations if trigger is needed - use [Tooltips](/components/tooltip) instead

## Basic Usage

```jsx live
<>
  <Annotation>
    Success variant
  </Annotation>
  <Annotation 
    variant="dark"
    arrowPlacement="left"
    className="mie-2 mb-2"
  >
    Dark variant
  </Annotation>
  <Annotation 
    variant="error" 
    arrowPlacement="top"
    className="mie-2 mb-2"
  >
    Error variant
  </Annotation>
  <Annotation
    variant="warning"
    arrowPlacement="right" 
    className="mb-2"
  >
    Warning variant
  </Annotation>
  <Annotation
    variant="light"
    className="mb-2"
  >
    Light variant. By default max width is set to 300px with wrapping text.
  </Annotation>
</>
```

## Referring to other elements

```jsx live
() => {
  const [arrowPlacement, setArrowPlacement] = useState('left')
  const wrapperClass = arrowPlacement === 'top' || arrowPlacement === 'bottom' ? 'flex-column' : '';
  return (
    <>
      {/* start example form block */}
      <ExamplePropsForm
        inputs={[
          { value: arrowPlacement, setValue: setArrowPlacement, options: [
            { name: 'left', value: 'right' },
            { name: 'top', value: 'bottom' },
            { name: 'right', value: 'left' },
            { name: 'bottom', value: 'top' }
          ], name: 'arrowPlacement' },
        ]}
      />
      {/* end example form block */}
        <div className={`d-flex align-items-center justify-content-center ${wrapperClass}`}>
            {(arrowPlacement === 'bottom') && (
                <Annotation arrowPlacement={arrowPlacement}>
                    Annotation on top
                </Annotation>
            )}
            {(arrowPlacement === 'right') && (
                <Annotation arrowPlacement={arrowPlacement}>
                    Annotation on left
                </Annotation>
            )}
            <Button>This is an example button</Button>
            {(arrowPlacement === 'left') && (
                <Annotation arrowPlacement={arrowPlacement}>
                    Annotation on right
                </Annotation>
            )}
            {(arrowPlacement === 'top') && (
                <Annotation arrowPlacement={arrowPlacement}>
                    Annotation on bottom
                </Annotation>
            )}
        </div>
    </>
  )
}

```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Annotation` is a non-interactive, always-visible text callout; no [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) applies (unlike a tooltip, it has no trigger). It renders static content and has no keyboard interaction of its own.

### ARIA roles, states, and properties

No ARIA attributes are rendered: the annotation is a plain styled `<span>`, read as ordinary text. The arrow (`arrowPlacement`) is purely visual — there is no programmatic association between the annotation and the element it points at.

### Screen reader expectations

- The annotation's text is read in DOM order as ordinary content; its variant color and arrow direction are not conveyed. *(Verification pending per the policy's AT matrix.)*

### Application responsibilities

- Place the annotation immediately before or after the element it refers to in the DOM, so the relationship survives without the visual arrow; if proximity alone is ambiguous, write text that names its subject.
- Don't rely on the variant color (`error`, `success`, `warning`) to carry meaning — say it in the text (WCAG 1.4.1 Use of Color).
- Keep annotations text-only: no links or interactive elements (use [Popover](/components/popover) instead, as noted above).

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Text has minimum contrast](https://www.w3.org/WAI/standards-guidelines/act/rules/afw4f7/) — annotation text against each variant background
