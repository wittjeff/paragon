---
title: 'Container'
type: 'component'
components:
- Container
categories:
- Layout
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

The base container to contain, pad, and center content in the viewport. This component extends the React-Bootstrap `Container`.<br/> <a href="https://react-bootstrap-v4.netlify.app/layout/grid/" target="_blank" rel="noopener noreferrer">See React-Bootstrap for more documentation.</a>

## Basic Usage

```jsx live
<div style={{ overflowX: 'auto' }}>
    <div style={{ width: '1500px', border: 'solid 3px var(--pgn-color-danger-700)' }}>
        <Container className="bg-danger-700 text-white my-4">
          The content in this container doesn't have a max width
        </Container>
        
        <Container size="xl" className="bg-danger-700 text-white my-4">
          The content in this container won't exceed the extra large width.
        </Container>
        
        <Container size="lg" className="bg-danger-700 text-white mb-4">
          The content in this container won't exceed the large width.
        </Container>
        
        <Container size="md" className="bg-danger-700 text-white mb-4">
          The content in this container won't exceed the medium width.
        </Container>
        
        <Container size="sm" className="bg-danger-700 text-white mb-4">
          The content in this container won't exceed the small width.
        </Container>
        
        <Container size="xs" className="bg-danger-700 text-white mb-4">
          The content in this container won't exceed the extra small width.
        </Container>
    </div>
</div>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Container` is a layout wrapper: it renders a `<div>` (or the element given via `as`) with width/padding classes from React-Bootstrap 1.6.5. There is no matching [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/).

Container renders static content and has no keyboard interaction of its own. No ARIA attributes are rendered; the component conveys no semantics.

### Application responsibilities

- Use the `as` prop to give page regions real semantics where appropriate — e.g. `as="main"` for the page's primary content, `as="section"` or `as="nav"` with a label for other regions — rather than leaving everything as anonymous `<div>`s.
- A container constrains width only; heading structure, reading order, and contrast of the content inside are yours.

### Relevant ACT rules

No ACT rule tests a bare layout wrapper; rules apply to the content placed inside it. One document-level rule is worth noting:

- [Document has a landmark with non-repeated content](https://www.w3.org/WAI/standards-guidelines/act/rules/b40fd1/) — satisfied naturally when the page's main container is rendered with `as="main"`

### Known issues

None identified in the August 2026 accessibility audit.
