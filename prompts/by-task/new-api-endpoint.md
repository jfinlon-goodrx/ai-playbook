# Task: New API Endpoint

## When to Use
You need to add a new REST endpoint to an existing ASP.NET Core API. Covers controller, service, DTOs, DI registration, validation, and tests.

## Prerequisites
- [ ] `.cursorrules` file in the project root
- [ ] At least one existing controller to reference for patterns
- [ ] Database context or repository interfaces available

## The Prompt

```
@Controllers/[ExistingController].cs
@Services/I[ExistingService].cs
@Program.cs

Add a new API endpoint with the following specification:

**Endpoint:** [HTTP Method] /api/[resource]/[action]
**Purpose:** [What this endpoint does in business terms]
**Request body:** [Describe the expected input, or "none" for GET]
**Response:** [Describe the expected output]
**Authorization:** [Who can call this — authenticated users, specific roles, anonymous]

Implementation requirements:
1. Create a new controller (or add to [ExistingController] if it belongs there)
2. Create request and response DTOs with validation attributes
3. Create a service interface and implementation
4. Register the service in DI (Program.cs or the appropriate extension method)
5. Add XML doc comments on the controller action and DTOs
6. Follow the exact same patterns as [ExistingController] for:
   - Return types
   - Error handling
   - Logging
   - Response wrapping

Security:
- Validate all inputs
- Use parameterized queries for any database access
- [Add HIPAA requirements if PHI is involved]

Testing:
- Unit tests for the service logic using xUnit and Moq
- Name tests: MethodName_Scenario_ExpectedResult
- Include happy path and at least 2 error/edge cases
```

## Example: Prescription Refill Endpoint

```
@Controllers/PrescriptionController.cs
@Services/IPrescriptionService.cs
@Models/Prescription.cs
@Program.cs

Add a new API endpoint for submitting a prescription refill request.

**Endpoint:** POST /api/prescriptions/{prescriptionId}/refill
**Purpose:** Patient or pharmacy staff requests a refill of an existing prescription
**Request body:**
{
  "pharmacyId": "string (required)",
  "requestedQuantity": "int (optional, defaults to original quantity)",
  "notes": "string (optional, max 500 chars)"
}
**Response:** 201 Created with the refill request ID and status
**Authorization:** Authenticated users with role "Patient" or "PharmacyStaff"

Implementation requirements:
1. Add to existing PrescriptionController
2. Create RefillRequestDto and RefillResponseDto with FluentValidation
3. Add SubmitRefillRequest method to IPrescriptionService and implementation
4. Verify the prescription exists and belongs to the authenticated user (or the pharmacy has access)
5. Verify the prescription is eligible for refill (not expired, has remaining refills)
6. Create audit log entry for the refill request

HIPAA requirements:
- Do not log the prescription drug name, dosage, or patient identifiers
- Log only the prescription ID, refill request ID, requesting user ID, and timestamp
- Do not include prescription details in error messages
- The response should include only the refill request ID and status, not full prescription details

Testing:
- Happy path: valid refill request returns 201
- Expired prescription returns 400 with appropriate error
- No remaining refills returns 400
- Unauthorized user (wrong patient) returns 403
- Invalid prescription ID returns 404
```

## Variations

### Minimal (quick scaffolding)
```
Add a GET /api/[resource] endpoint that returns all [resources] for the
authenticated user. Follow the patterns in [ExistingController]. Include
service, DI registration, and unit tests.
```

### With Pagination
```
Add the standard endpoint but include pagination:
- Accept "page" and "pageSize" query parameters
- Default to page 1, pageSize 20, max pageSize 100
- Return a PagedResult<T> with items, totalCount, page, pageSize, totalPages
- Follow the pagination pattern in [ExistingController] if one exists
```

### With Caching
```
Add the endpoint with response caching:
- Cache for 5 minutes for authenticated users
- Vary by user ID
- Include ETags for conditional requests
- Invalidate cache on any mutation to the same resource
```

## Tips
- Always reference an existing controller so the AI matches your patterns
- If the AI uses a different return type or error pattern than your project, correct it immediately — "We use Result<T> not throwing exceptions for business logic failures"
- For PHI endpoints, always include the HIPAA block — the AI won't remember between sessions
- Request validation attributes AND FluentValidation if your project uses both
