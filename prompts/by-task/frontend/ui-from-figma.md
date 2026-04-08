# Task: UI Slice From Figma

## When to Use

You have a Figma design or structured UI handoff and want the assistant to turn it into a reviewable implementation plan or a frontend slice.

## Use With

- `ui-from-figma.react.md`
- `ui-from-figma.blazor.md`
- `ui-from-figma.server-rendered.md`

## Prompt: Plan From Design

```text
Use this design handoff and the referenced codebase files to plan a UI implementation.

Design context:
- Screen or component name: [name]
- Figma summary: [layout, sections, variants, tokens, states]
- Responsive behavior: [mobile/tablet/desktop expectations]
- Accessibility notes: [focus, labels, keyboard, screen-reader behavior]
- Data dependencies: [API calls, mutations, loaders]

Code context:
@[existing page or component]
@[design-system component]
@[existing UI test file]

Produce:
1. component breakdown
2. state and interaction model
3. accessibility checklist
4. responsive considerations
5. test plan
6. assumptions and ambiguities
```

## Prompt: Build The Slice

```text
Using the approved plan, implement the UI slice.

Requirements:
- reuse the existing design system and components first
- include loading, empty, success, error, and validation states where relevant
- match the existing spacing, typography, and interaction conventions
- keep the code easy to review
- add or update UI tests for the critical flows

Do not invent a new visual language if the product already has one.
```
