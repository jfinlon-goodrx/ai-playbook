# Task: UI Slice From Figma for Server-Rendered UI

## Best Fit

Use this for Razor Pages, MVC views, Django templates, Go templates, `templ`, or similar server-rendered UI stacks.

## Prompt

```text
@[existing page, view, or template]
@[existing partial or component-like template]
@[existing frontend test or browser test]
@[project context or rules file]

Use this Figma handoff to implement a server-rendered UI slice.

Design context:
- Screen or component: [name]
- Figma summary: [layout, sections, variants, tokens, states]
- Responsive behavior: [requirements]
- Accessibility notes: [requirements]
- Data dependencies: [page model, handler, view model, or endpoint inputs]

Requirements:
- keep the current server-rendered architecture
- use existing partials, templates, and UI helpers first
- add targeted interactivity only with the stack's existing approach
  (HTMX, Alpine, unobtrusive JS, or current scripts)
- include loading, empty, error, and validation states where appropriate
- add browser or integration tests for critical flows when the repo supports them
- call out any design ambiguities before implementation
```
