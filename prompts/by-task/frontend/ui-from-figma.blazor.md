# Task: UI Slice From Figma for Blazor

## Prompt

```text
@Pages/[ExistingPage].razor
@Shared/[ExistingComponent].razor
@Services/[RelevantService].cs
@[existing UI test or component example]

Use this Figma handoff to implement a Blazor UI slice.

Design context:
- Screen or component: [name]
- Figma summary: [layout, sections, variants, tokens, states]
- Responsive behavior: [requirements]
- Accessibility notes: [requirements]
- Data dependencies: [API calls and state sources]

Requirements:
- specify whether this is Blazor Server or WebAssembly
- reuse the current component library and styling approach
- include loading, empty, error, and validation states
- match existing service-injection and state patterns
- add tests or review notes using the repo's current UI testing approach
- call out any design ambiguities before implementation
```
