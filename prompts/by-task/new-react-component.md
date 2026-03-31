# Task: New React/Blazor Component

## When to Use
You need to add a new page or component to a frontend application. Covers React (if used) and Blazor Server/WASM (common in .NET shops).

## The Prompt: Blazor Component

```
@Pages/[ExistingPage].razor
@Shared/[ExistingComponent].razor
@Services/I[RelevantService].cs

Create a new Blazor page/component:

**Page route:** /[route]
**Purpose:** [What the user sees and does on this page]
**Data source:** [Which API endpoint(s) it calls]

Layout and behavior:
1. [Describe the page layout — header, form, table, cards, etc.]
2. [Describe user interactions — click, submit, filter, sort]
3. [Describe validation — what fields are required, what formats are enforced]
4. [Describe loading/error states]

Requirements:
- Follow the same component patterns as [ExistingPage]
- Use the existing CSS framework/classes (Bootstrap, Tailwind, or custom)
- Implement loading spinners during API calls
- Handle and display API errors gracefully
- Form validation should match the API's validation rules
- Inject and use [ServiceName] for API calls

Accessibility:
- Proper aria labels on interactive elements
- Keyboard navigation for forms and tables
- Semantic HTML (not div soup)

If this page displays patient data:
- Do not show full SSN (mask as ***-**-1234)
- Do not show full DOB in list views (show age instead)
- Show a "Viewing PHI" indicator per HIPAA UI guidelines
```

## The Prompt: React Component (if applicable)

```
@src/components/[ExistingComponent].tsx
@src/hooks/[ExistingHook].ts
@src/types/[RelevantTypes].ts

Create a new React component:

**Component name:** [Name]
**Location:** src/components/[path]/[Name].tsx
**Purpose:** [What it renders and does]

Props:
- [propName]: [type] — [description]

Behavior:
1. [Describe what the component renders]
2. [Describe user interactions]
3. [Describe API calls (use existing hooks or create new ones)]

Requirements:
- TypeScript with strict typing (no `any`)
- Use existing hooks pattern for API calls
- Follow the component structure in [ExistingComponent]
- Error boundary or error handling for API failures
- Loading state while data is fetching
- Responsive design matching existing pages

Tests:
- Unit tests with React Testing Library
- Test rendering with mock data
- Test user interactions (click, submit, input)
- Test loading and error states
```

## The Prompt: Data Table / Grid

```
Create a data table component for displaying [entity] records:

**Columns:**
| Column | Source Field | Sortable | Filterable | Format |
|---|---|---|---|---|
| [Name] | [field] | Yes/No | Yes/No | [text/date/currency/status badge] |

**Features:**
- Server-side pagination (page, pageSize params to API)
- Column sorting (click header to sort, shift+click for multi-sort)
- Search/filter bar
- Row click navigates to detail page (/[entity]/{id})
- Export to CSV button
- Row count display: "Showing 1-20 of 342 results"

**API endpoint:** GET /api/[entity]?page=1&pageSize=20&sortBy=name&sortDir=asc&search=term

Follow the table patterns in [ExistingTablePage] if one exists.

If displaying PHI columns (patient name, DOB):
- Add column-level visibility toggle
- Default PHI columns to masked/hidden
- Log when a user reveals PHI columns (audit requirement)
```

## The Prompt: Form with Validation

```
Create a form component for [creating/editing] a [entity]:

**Fields:**
| Field | Type | Required | Validation | Notes |
|---|---|---|---|---|
| [fieldName] | text/number/date/select/textarea | Yes/No | [rules] | [notes] |

**Behavior:**
- Client-side validation matching the API's validation rules
- Submit calls [POST/PUT] /api/[endpoint]
- Show inline validation errors per field
- Show a summary error if the API returns validation errors
- Disable submit button while request is in flight
- Success: show toast/notification and navigate to [page]
- Auto-save draft to localStorage every 30 seconds (optional)

**Layout:**
- [Single column / two column / stepped wizard]
- Group related fields with section headers
- Required fields marked with asterisk

If the form collects PHI:
- Mark PHI fields with a visual indicator
- Do not store PHI in localStorage (draft save should exclude PHI fields)
- Clear form data from memory on unmount/navigation
```

## Tips
- Always reference an existing page/component so the AI matches your project's UI patterns
- For Blazor, specify whether you're using Blazor Server or Blazor WASM — the data access patterns are different
- For PHI display, be specific about masking rules — the AI won't know your organization's policy unless you tell it
- Request accessibility features explicitly — the AI won't add aria labels unless asked
- If your project uses a component library (MudBlazor, Radzen, Material UI), mention it in the prompt so the AI uses the right components
