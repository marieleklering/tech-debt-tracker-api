# Context and Design Decisions

*This section documents the content design decisions for portfolio review purposes.*


## Research foundation:

- A mockup API gives the right balance of creativity without constraint when it comes to documentation.
- Most teams don't have a clear visibility of their technical debt nor knowledge of how much time to fix it.
- Technical debt is a present reality in all companies that work with coding, and its extremely important that it’s tracked and understood.


## Design rationale:

- Developers generally prefer visual overviews before diving into technical details
- Severity scales beyond 5 levels tend to cause decision fatigue
- Technical debt estimates become less accurate over time—this informed the auto-growing estimate feature
- Authentication is a common pain point in API documentation


## Content Strategy Decisions:

*This section documents the API decisions and the rationale behind every statement.*

| **Decision** | **Rationale** | **Evidence** |
| --- | --- | --- |
| Severity scale limits 1-5 | A range of numbers bigger than that causes analysis paralysis.  | Industry best practice |
| Growing estimate time based on severity and date | The older the technical debt, the harder it is to fix it.  | Common developer experience |
| Quick Visual section placed early | Developers want to see value before reading details | Documentation best practice |
| Consistent endpoint structure | Reduces learning time across endpoints | Standard API doc pattern |
| curl examples only | Works for all developers regardless of language | Universal compatibility |


## Goals, signals and measures

| **Goal** | **Measure** | **Measure** |
| --- | --- | --- |
| Fast onboarding | Time to first successful API call | < 5 minutes |
| Self-service | Users resolve errors without support | 80% self-resolution |
| Findability | Users locate information quickly | < 30 seconds |
| Comprehension | Users understand estimate growth concept | > 85% accuracy |


## Information Architecture Choices:


```bash
Tech Debt Tracker API Documentation
│
├── 1. Overview
│   ├── Key Features
│   └── Who is this for
│
├── 2. Quick Visual
│   └── Scoreboard preview
│
├── 3. How It Works
│   ├── Step 1: Log a Debt Entry
│   ├── Step 2: Track Automatically
│   └── Step 3: Watch Scoreboard
│
├── 4. Getting Started
│   ├── Requirements
│   ├── Authentication
│   └── Example request
│
├── 5. API Reference
│   ├── Create Debt (POST /v1/debts)
│   │   ├── Parameters
│   │   ├── Example request
│   │   ├── Response
│   │   └── Error
│   │
│   ├── List Debts (GET /v1/debts)
│   │   ├── Query parameters
│   │   ├── Example request
│   │   └── Response
│   │
│   ├── Update Debt (PUT /v1/debts/{id})
│   │   ├── Example request
│   │   └── Response
│   │
│   └── Scoreboard
│       ├── Global (GET /v1/scoreboard/global)
│       └── Recent Fixes (GET /v1/scoreboard/recent-fixes)
│
├── 6. Troubleshooting
│   ├── 401 Unauthorized
│   └── 400 Bad Request
│
└── 7. FAQ
    ├── How is estimate growth calculated?
    ├── Can I delete a debt entry?
    ├── Severity levels explained
    ├── Rate limits
    └── Exporting data
```
Link to image diagram here.
[Diagram for information architecture](../images/information-architecture.png)


## Accessibility Considerations

This section documents the accessibility considerations when deciding the documentation design.  

| **Element** | **Approach** |
| --- | --- |
| Error messages | Include actionable steps, not just code. |
| Hierarchy | Semantic heading levels (H1 → H2 → H3) |
| Code blocks | High contrast syntax highlighting; not colour-dependent |
| Coded colours | Helps identify error and success messages  |
|  |  |


## Journey Map : Developer Onboarding

![Journey Map for Developer Onboarding image of the API documentation.](../images/journey-map.png)


## API Workflow
placeholder


## Voice Principles

| Principle | Application |
| --- | --- |
| **Direct** | Lead with the action: "Use POST /v1/debts to create..." not "You can create a debt by using..." |
| **Confident** | State facts without hedging: "The API returns..." not "The API should return..." |
| **Technically precise** | Use correct terminology: "endpoint" not "link", "parameter" not "field" (in query strings) |
| **Helpful** | Anticipate problems: Every error shows a solution, not just the error code |


## Tone Variations

| Context | Tone | Example |
| --- | --- | --- |
| Getting Started | Encouraging | "Execute the command below to see the scoreboard" |
| API Reference | Neutral, precise | "Returns a JSON object containing..." |
| Troubleshooting | Supportive | "Check that your API key is correct" |
| Errors | Clear, actionable | "Include all required fields: title, severity, estimated_fix_time" |


## Iteration and improvements

V1 > V2
