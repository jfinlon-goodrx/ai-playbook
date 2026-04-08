# Figma and AI Frontend Workflow

## Why This Matters

Frontend teams lose time when design intent lives in one tool, implementation happens in another, and nobody captures the states, constraints, and accessibility rules in a form an AI assistant can reliably use.

Figma can close that gap if it is treated as structured input, not just as a screenshot source.

## Best Use Of Figma In AI-Assisted Development

Use Figma to provide:

- layout and spacing intent
- component structure
- variants and states
- typography and color rules
- responsive behavior
- interaction notes
- content hierarchy
- acceptance criteria for the UI

Do not rely on:

- a single screenshot with no component context
- visual guesswork about spacing or behavior
- AI inferring accessibility requirements you never stated

## Recommended Workflow

### 1. Shape The Design For Handoff

Before using AI, make sure the Figma file includes:

- named frames and components
- variant states
- annotations for loading, error, empty, and success states
- responsive notes for mobile and desktop
- clear content rules and truncation behavior
- accessibility notes where behavior is non-obvious

### 2. Extract The Right Inputs

Give the assistant:

- the screen or component name
- the component hierarchy
- key tokens or style references
- behavior notes
- validation rules
- API or data dependencies
- screenshots only as supporting evidence

The best prompt context is:

- Figma frame or component summary
- one or two screenshots
- existing UI component references from the codebase
- design-system guidance

### 3. Ask For A Plan Before Code

Have the assistant produce:

- implementation plan
- components to create or extend
- state model
- accessibility checklist
- test plan
- risks and open questions

### 4. Generate A Reviewable Slice

Ask the assistant to:

- reuse existing components when possible
- match the design system first
- document assumptions where Figma is ambiguous
- include loading, empty, error, and success states
- include responsive behavior and keyboard accessibility

### 5. Verify Against Design

Review generated UI with:

- side-by-side Figma comparison
- component state checks
- responsive checks
- keyboard and screen-reader checks
- visual regression or screenshot testing where available

## Prompt Pattern

```text
Use this Figma handoff plus the referenced codebase files to implement a frontend slice.

Design context:
- Screen or component: [name]
- Figma summary: [layout, sections, components, tokens, states]
- Responsive behavior: [mobile/tablet/desktop expectations]
- Accessibility notes: [focus order, labels, roles, keyboard behavior]
- Data dependencies: [API calls or state sources]

Code context:
@[existing page or component]
@[design-system component]
@[existing test file]

First produce:
1. component breakdown
2. state and interaction plan
3. accessibility checklist
4. test plan
5. assumptions and ambiguities

After approval, implement the UI using existing patterns and components.
```

## Tools That Pair Well With This Workflow

- Figma Dev Mode for design inspection
- Storybook for component review and shared states
- Playwright for interaction and regression tests
- axe or similar accessibility tooling
- Chromatic or Percy for visual regression if the team already uses Storybook

## Common Failure Modes

- generating a page from visuals only, with no interaction model
- ignoring loading, empty, and error states
- introducing a new component style instead of reusing the system
- matching pixels while missing accessibility or usability
- letting AI invent spacing and typography tokens not in the design system

## Rule Of Thumb

Use Figma to define intent and constraints. Use AI to accelerate implementation and variation work. Use design review and automated checks to keep the result trustworthy.
