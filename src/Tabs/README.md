---
title: 'Tabs'
type: 'component'
components:
- Tabs
- Tab
categories:
- Navigation
status: 'Stable'
designStatus: 'Done'
devStatus: 'Done'
---

<p>
  This is a pass through component from React-Bootstrap.<br/>
  <a href="https://react-bootstrap-v4.netlify.app/components/tabs/" target="_blank" rel="noopener noreferrer">
    See React-Bootstrap for documentation.
  </a>
</p>

## Uncontrolled usage

```jsx live
<Tabs
  variant="tabs"
  defaultActiveKey="profile"
  id="uncontrolled-tab-example"
>
  <Tab eventKey="home" title="Home">
    Hello I am the first panel.
  </Tab>
  <Tab eventKey="profile" title="Profile">
    Hello I am the second panel.
  </Tab>
  <Tab eventKey="contact" title="Contact" disabled>
    Hello I am third first panel.
  </Tab>
</Tabs>
```

## Controlled usage

```jsx live
() => {
  const [key, setKey] = useState('home');

  return (
    <Tabs
      id="controlled-tab-example"
      activeKey={key}
      onSelect={(k) => setKey(k)}
    >
      <Tab eventKey="home" title="Home">
        Hello I am the first panel.
      </Tab>
      <Tab eventKey="profile" title="Profile">
        Hello I am the second panel.
      </Tab>
      <Tab eventKey="contact" title="Contact" disabled>
        Hello I am third first panel.
      </Tab>
    </Tabs>
  );
}
```

## Button group usage

```jsx live
<Tabs
  defaultActiveKey="profile"
  id="uncontrolled-pills-tab-example"
  variant="button-group"
>
  <Tab eventKey="home" title="Home">
    Hello I am the first panel.
  </Tab>
  <Tab eventKey="profile" title="Profile">
    Hello I am the second panel.
  </Tab>
  <Tab eventKey="contact" title="Contact" disabled>
    Hello I am third first panel.
  </Tab>
</Tabs>
```

## Pills usage

```jsx live
<Tabs
  defaultActiveKey="profile"
  id="uncontrolled-pills-tab-example"
  variant="pills"
>
  <Tab eventKey="home" title="Home">
    Hello I am the first panel.
  </Tab>
  <Tab eventKey="profile" title="Profile">
    Hello I am the second panel.
  </Tab>
  <Tab eventKey="contact" title="Contact" disabled>
    Hello I am third first panel.
  </Tab>
</Tabs>
```

## Inverse-Pills usage

```jsx live
<Stack className="bg-dark-700 p-4">
  <Tabs
    variant="inverse-pills"
    defaultActiveKey="profile"
    id="uncontrolled-pills-tab-example"
  >
    <Tab eventKey="home" title="Home">
      Hello I am the first panel.
    </Tab>
    <Tab eventKey="profile" title="Profile">
      Hello I am the second panel.
    </Tab>
    <Tab eventKey="contact" title="Contact" disabled>
      Hello I am third first panel.
    </Tab>
  </Tabs>
</Stack>
```

## Inverse Tabs usage

```jsx live
<Stack className="bg-dark-700 p-4">
  <Tabs
    variant="inverse-tabs"
    defaultActiveKey="profile"
    id="uncontrolled-pills-tab-example"
  >
    <Tab eventKey="home" title="Home">
      Hello I am the first panel.
    </Tab>
    <Tab eventKey="profile" title="Profile">
      Hello I am the second panel.
    </Tab>
    <Tab eventKey="contact" title="Contact" disabled>
      Hello I am third first panel.
    </Tab>
  </Tabs>
</Stack>
```

## With notification

```jsx live
<Tabs defaultActiveKey="profile" id="tab-example-with-notification">
  <Tab eventKey="home" title="Home">
    Hello I am the first panel.
  </Tab>
  <Tab eventKey="profile" title="Profile" notification={1}>
    Hello I am the second panel.
  </Tab>
</Tabs>
```

### With screen reader text in notification

```jsx live
<Tabs defaultActiveKey="profile" id="tab-example-with-sr-notification">
  <Tab eventKey="home" title="Home">
    Hello I am the first panel.
  </Tab>
  <Tab
    eventKey="profile" 
    title="Profile" 
    notification={(
      <span>
        1
        <span className="sr-only">notification</span>
      </span>
    )}
  >
    Hello I am the second panel.
  </Tab>
</Tabs>
```

### Responsive support

```jsx live
<Tabs
  variant="tabs"
  defaultActiveKey="profile"
  id="uncontrolled-tab-example"
>
  <Tab eventKey="home" title="Home">
    Hello I am the first panel.
  </Tab>
  <Tab eventKey="dashboard" title="Dashboard">
    Hello I am the second panel.
  </Tab>
  <Tab eventKey="profile" title="Profile" notification={1}>
    Hello I am the third panel.
  </Tab>
  <Tab eventKey="products" title="Products">
    Hello I am the fourth panel.
  </Tab>
  <Tab eventKey="help" title="Help">
    Hello I am the fifth panel.
  </Tab>
  <Tab eventKey="about" title="About">
    Hello I am the sixth panel.
  </Tab>
  <Tab eventKey="updates" title="Updates">
    Hello I am the seventh panel.
  </Tab>
  <Tab eventKey="forum" title="Forum">
    Hello I am the eighth panel.
  </Tab>
  <Tab eventKey="job" title="Job">
    Hello I am the ninth panel.
  </Tab>
  <Tab eventKey="team" title="Our team">
    Hello I am the tenth panel.
  </Tab>
</Tabs>
```

## Conditional rendering

```jsx live
() => {
  const librariesEnabled = true;
  const visibleTabs = useMemo(() => {
    const tabs = [];
        
    tabs.push(
      <Tab
        key="courses"
        eventKey="courses"
        title="Courses"
      >
        Hello I am the courses panel.
      </Tab>
    );

    tabs.push(
      <Tab
        key="programs"
        eventKey="programs"
        title="Programs"
      >
        Hello I am the programs panel.
      </Tab>
    );
    
    if (librariesEnabled) {
      tabs.push(
        <Tab
          key="libraries"
          eventKey="libraries"
          title="Libraries"
        >
          Hello I am the libraries panel.
        </Tab>
      );   
    }

    return tabs;
  }, [librariesEnabled]);

  return (
    <Tabs
      id="tabs"
      defaultActiveKey="courses"
    >
      {visibleTabs}
    </Tabs>
  );
}
```

***

## Accessibility

**Status:** documented from source (Paragon 23.x, August 2026). Assistive technology verification pending — see the [Accessibility policy](/guides/accessibility-policy).

Paragon's `Tabs` wraps [React-Bootstrap's Tabs](https://react-bootstrap-v4.netlify.app/components/tabs/) and adds responsive overflow handling (hidden tabs collapse into a "More..." dropdown). It follows the [APG tabs pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/) with several deviations noted below.

### Keyboard support

| Key | Action |
| --- | --- |
| <kbd>Tab</kbd> | Moves focus to the next tab. Note: every tab is in the page tab sequence (see deviations). |
| <kbd>Arrow Left</kbd> / <kbd>Arrow Up</kbd> | Moves focus to the previous tab and selects it (selection follows focus). |
| <kbd>Arrow Right</kbd> / <kbd>Arrow Down</kbd> | Moves focus to the next tab and selects it. |
| <kbd>Enter</kbd> | Activates the focused tab. In the "More..." overflow dropdown, activates the focused hidden tab. |

<kbd>Home</kbd>/<kbd>End</kbd> are not currently supported (see deviations).

### ARIA roles, states, and properties

| Element | Attribute | Notes |
| --- | --- | --- |
| Tab list container (`<nav>`) | `role="tablist"` | |
| Each tab | `<a href="#">` with `role="tab"` | |
| Each tab | `aria-selected` | `true` on the active tab. |
| Tab panel | `role="tabpanel"` | |
| Tab panel | `aria-hidden` | `true` on inactive panels. |
| Notification bubble (`notification` prop) | `role="status"`, `aria-live="polite"` | Count changes are announced without stealing focus. |
| "More..." overflow toggle | `<button>` with `aria-haspopup="true"`, `aria-expanded` | Rendered inside an element with `role="tab"` (see deviations). |

**Deviations from the APG pattern (identified in the August 2026 accessibility audit):**

- Tab panels have no `aria-labelledby` and tabs have no `aria-controls` — tabs and their panels are not programmatically associated.
- Tabs are anchors that all remain in the page tab sequence; the APG pattern expects roving tabindex (one tab stop for the tab list). <kbd>Space</kbd> does not activate a tab (anchor semantics).
- The responsive "More..." toggle is a button nested inside an element with `role="tab"` that has no `aria-selected`, and the hidden tabs it reveals are dropdown items rather than tabs.

### Screen reader expectations

- A focused tab announces its label, "tab", position ("2 of 4"), and selected state. *(Verification pending per the policy's AT matrix; the missing tab↔panel association above may affect what panels announce.)*
- Notification counts (the `notification` prop) announce politely on change, e.g. "4".

### Application responsibilities

- Give every `Tab` a concise, distinct `title` — it is the tab's accessible name.
- Use the `notification` prop (rather than a hand-rolled badge) so counts are announced via the built-in live region; pair it with visually-hidden context (e.g. `<span className="sr-only">new messages</span>`) when the number alone is ambiguous.
- Don't place additional interactive elements inside tab titles.
- If tabs drive URL navigation rather than switching in-page panels, use links styled as tabs (`variant="tabs"` navigation) rather than this component, so semantics match behavior.

### Relevant ACT rules

[W3C Accessibility Conformance Testing rules](https://www.w3.org/WAI/standards-guidelines/act/rules/)
give objective, tool-neutral procedures for testing the claims above:

- [ARIA required context role](https://www.w3.org/WAI/standards-guidelines/act/rules/ff89c9/) — `tab` must be owned by a `tablist`
- [ARIA required owned elements](https://www.w3.org/WAI/standards-guidelines/act/rules/bc4a75/) — `tablist` must own `tab` elements
- [Element with role attribute has required states and properties](https://www.w3.org/WAI/standards-guidelines/act/rules/4e8ab6/)
- [Link has non-empty accessible name](https://www.w3.org/WAI/standards-guidelines/act/rules/c487ae/) — tabs render as anchors
- [Element in sequential focus order has visible focus](https://www.w3.org/WAI/standards-guidelines/act/rules/oj04fd/)

### Known issues

Tracked together in [#4404](https://github.com/openedx/paragon/issues/4404), identified in the August 2026 accessibility audit:

- Missing tab↔panel association (`aria-controls`/`aria-labelledby`).
- No roving tabindex: all tabs are tab stops, and <kbd>Space</kbd> does not activate a tab — deviation from the APG tabs pattern.
- The responsive "More..." overflow control nests a button inside `role="tab"` and exposes hidden tabs as dropdown items.
