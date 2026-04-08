# Frontend Tooling Recommendations

## Goal

Give the team a pragmatic starting point for frontend implementation, design-system reuse, and AI-assisted UI development.

## Cross-Stack Recommendations

Use these tools or categories regardless of stack:

- Figma for design source of truth and component intent
- Storybook when the frontend has a meaningful component surface
- Playwright for user-flow and UI regression coverage
- axe or equivalent accessibility checks
- a shared token or design-system strategy before scaling AI-generated UI work

## Recommended Stack Choices

### React

Best when:

- the product has a rich interactive UI
- the team already uses TypeScript heavily
- shared component systems matter

Recommended starting choices:

- MUI for enterprise CRUD and data-heavy applications
- `shadcn/ui` plus Tailwind for teams that want stronger custom branding
- Bootstrap only when the app is already Bootstrap-heavy or speed matters more than uniqueness

### Blazor

Best when:

- the team is heavily .NET-oriented
- server-side or internal line-of-business UI is a strong fit
- shared C# models across client and server are valuable

Recommended starting choices:

- MudBlazor for a mature component library
- Bootstrap for lightweight or legacy-friendly work
- Radzen only if the team is comfortable with its patterns and generated surface

### Razor Pages or MVC Views

Best when:

- the app is mostly server-rendered
- the team wants modest interactivity without a full SPA
- simple workflows and fast delivery matter more than client-side complexity

Recommended starting choices:

- Bootstrap for fast consistent layout
- Tailwind if the team already uses it and has conventions
- HTMX or Alpine.js for targeted interactivity

### Python Web UI

Best fit patterns:

- Django templates plus HTMX for server-rendered business apps
- FastAPI backend plus React frontend for richer applications
- Streamlit or Gradio for internal tools and prototypes

Recommended starting choices:

- Bootstrap or Tailwind for server-rendered apps
- React plus MUI or a design-system library for separate frontend apps

### Go Web UI

Best fit patterns:

- `templ` or `html/template` plus HTMX for server-rendered apps
- React frontend if the product truly needs SPA complexity

Recommended starting choices:

- Tailwind for modern server-rendered UI if the team is comfortable with utility CSS
- Bootstrap for faster standardization and lower CSS design overhead

## Process Recommendations

### 1. Design-System First

Before scaling AI-generated UI work:

- pick a component library or system
- define spacing, color, typography, and form conventions
- document common component states

### 2. Figma To Plan To Code

Use this sequence:

1. design review
2. AI-generated component and state plan
3. implementation
4. accessibility and visual review
5. UI test coverage

### 3. Require State Completeness

Every meaningful screen should have:

- loading
- empty
- success
- error
- validation feedback

### 4. Review With Evidence

Frontend review improves when PRs include:

- screenshots or recordings
- responsive evidence
- design comparisons
- accessibility notes

## Suggested Team Defaults

- React team: TypeScript, MUI, Storybook, Playwright
- Blazor team: MudBlazor or Bootstrap, Playwright, shared design guidelines
- Razor/server-rendered team: Bootstrap or Tailwind, HTMX where needed, Playwright
- Python or Go server-rendered team: HTMX plus Bootstrap or Tailwind, Playwright for key flows

## Rule Of Thumb

AI makes frontend work faster only when design intent, component reuse, and review discipline are strong. Without that, it generates polished-looking inconsistency at high speed.
