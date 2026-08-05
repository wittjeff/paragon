---
title: 'Figure'
type: 'component'
categories:
- Content
components:
- Figure
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

<p className="lead">
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/figures/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for documentation.
  </a>
</p>

## Basic Usage

```jsx live
<Figure>
  <Figure.Image
    alt="nature flower"
    src="https://picsum.photos/200/200/"
  />
  <Figure.Caption>
    Nulla vitae elit libero, a pharetra augue mollis interdum.
  </Figure.Caption>
</Figure>
```

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Figure` is a pass-through to React-Bootstrap 1.6.5 and renders native HTML semantics: `Figure` renders a `<figure>` element, `Figure.Image` renders an `<img>` (via React-Bootstrap's `Image`), and `Figure.Caption` renders a `<figcaption>`. It renders static content and has no keyboard interaction of its own; no [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/) applies.

### ARIA roles, states, and properties

No ARIA attributes are added; the accessibility semantics are the native `figure` / `img` / `figcaption` roles. The `alt` prop on `Figure.Image` is passed through to the `<img>` and is **not required by the component**, so an image with no `alt` prop renders with no accessible name.

### Application responsibilities

- Always set `alt` on `Figure.Image`: a text alternative when the image conveys information, or `alt=""` if it is purely decorative (WCAG 1.1.1).
- The caption is not a substitute for `alt` — `<figcaption>` provides context or attribution, while `alt` describes the image content; avoid making them identical, which produces duplicate announcements.
- Text inside the image itself is not accessible; put essential text in the caption or surrounding content (WCAG 1.4.5).

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Image has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/23a2a8/) — `Figure.Image`
- [Image accessible name is descriptive](https://www.w3.org/WAI/standards-guidelines/act/rules/qt1vmo/)
- [Image not in the accessibility tree is decorative](https://www.w3.org/WAI/standards-guidelines/act/rules/e88epe/) — decorative figures need `alt=""`

### Known issues

None identified in the August 2026 accessibility audit.
