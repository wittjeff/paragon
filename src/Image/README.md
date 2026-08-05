---
title: 'Image'
type: 'component'
categories:
- Imagery & Iconography
components:
- Image
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
notes: |

---

<p className="lead">
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/images/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for documentation.
  </a>
</p>

## Basic Styles

```jsx live
<>
  <Image
    className="mr-2"
    src="https://picsum.photos/100/100/"
    rounded
    alt="Image description"
  />
  <Image
    className="mr-2"
    src="https://picsum.photos/100/100/"
    roundedCircle
    alt="Image description"
  />
  <Image
    className="mr-2"
    src="https://picsum.photos/100/100/"
    thumbnail
    alt="Image description"
  />
</>
```

## Fluid Sizing

```jsx live
<Image src="https://picsum.photos/1600/800/" fluid alt="Image description" />
```



## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

`Image` is a pass-through of React-Bootstrap 1.6.5's `Image`, which renders a plain `<img>` and adds only styling classes. There is no matching [APG pattern](https://www.w3.org/WAI/ARIA/apg/patterns/); the `alt` attribute is the whole accessibility story.

Image renders static content and has no keyboard interaction of its own.

### ARIA roles, states, and properties

No ARIA attributes are rendered. React-Bootstrap does not set `alt` for you (the upstream source explicitly suppresses its own alt-text lint rule) — the accessible name is entirely the `alt` you pass.

### Screen reader expectations

- With a non-empty `alt`, the image announces as that text plus "image". *(Verification pending per the policy's AT matrix.)*
- With `alt=""`, the image is skipped. With **no** `alt` attribute at all, behavior is undefined and some screen readers fall back to reading the file name — always pass `alt`, even when empty.

### Application responsibilities

- Always set `alt`: meaningful text for informative images, an empty string (`alt=""`) for purely decorative ones.
- Write `alt` for what the image means in context, not what it looks like; keep it concise and don't prefix it with "image of".
- Avoid images of text — use real text with CSS instead (WCAG 1.4.5).
- For complex images (charts, diagrams), pair a short `alt` with a fuller text alternative in the page.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [Image has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/23a2a8/) — every informative rendering
- [Image accessible name is descriptive](https://www.w3.org/WAI/standards-guidelines/act/rules/qt1vmo/) — `alt` quality
- [HTML images contain no text](https://www.w3.org/WAI/standards-guidelines/act/rules/0va7u6/)
- [Image not in the accessibility tree is decorative](https://www.w3.org/WAI/standards-guidelines/act/rules/e88epe/) — the `alt=""` case

### Known issues

None identified in the August 2026 accessibility audit.
