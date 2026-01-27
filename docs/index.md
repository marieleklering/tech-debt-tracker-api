# API Developer Documentation

Learn how to use the Tech Debt Tracker API to log technical debt, monitor estimates, and track your team's progress.

## Context and Design Decisions

*This section documents the research and key findings for the Tech Debt Tracker API and why it was chosen.*

**Research foundation:**

- A mockup API gives the right balance of creativity without constraint when it comes to documentation.
- Most teams don't have a clear visibility of their technical debt nor knowledge of how much time to fix it.
- Technical debt is a present reality in all companies that work with coding, and its extremely important that it’s tracked and understood.

**Design rationale:**

- Developers generally prefer visual overviews before diving into technical details
- Severity scales beyond 5 levels tend to cause decision fatigue
- Technical debt estimates become less accurate over time—this informed the auto-growing estimate feature
- Authentication is a common pain point in API documentation

**Content Strategy Decisions:**

*This section documents the API decisions and the rationale behind every statement.*

| **Decision** | **Rationale** | **Evidence** |
| --- | --- | --- |
| Severity scale limits 1-5 | A range of numbers bigger than that causes analysis paralysis.  | Industry best practice |
| Growing estimate time based on severity and date | The older the technical debt, the harder it is to fix it.  | Common developer experience |
| Quick Visual section placed early | Developers want to see value before reading details | Documentation best practice |
| Consistent endpoint structure | Reduces learning time across endpoints | Standard API doc pattern |
| curl examples only | Works for all developers regardless of language | Universal compatibility |

**Goals, signals and measures**

| **Goal** | **Measure** | **Measure** |
| --- | --- | --- |
| Fast onboarding | Time to first successful API call | < 5 minutes |
| Self-service | Users resolve errors without support | 80% self-resolution |
| Findability | Users locate information quickly | < 30 seconds |
| Comprehension | Users understand estimate growth concept | > 85% accuracy |

**Information Architecture Choices:**

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

**Accessibility Considerations**

This section documents the accessibility considerations when deciding the documentation design.  

| **Element** | **Approach** |
| --- | --- |
| Error messages | Include actionable steps, not just code. |
| Hierarchy | Semantic heading levels (H1 → H2 → H3) |
| Code blocks | High contrast syntax highlighting; not colour-dependent |
| Coded colours | Helps identify error and success messages  |
|  |  |

Journey Map : Developer Onboarding

API Workflow

### Voice and Tone Guidelines

*This section documents the content design decisions for portfolio review purposes.*

### Voice Principles

| Principle | Application |
| --- | --- |
| **Direct** | Lead with the action: "Use POST /v1/debts to create..." not "You can create a debt by using..." |
| **Confident** | State facts without hedging: "The API returns..." not "The API should return..." |
| **Technically precise** | Use correct terminology: "endpoint" not "link", "parameter" not "field" (in query strings) |
| **Helpful** | Anticipate problems: Every error shows a solution, not just the error code |

### Tone Variations

| Context | Tone | Example |
| --- | --- | --- |
| Getting Started | Encouraging | "Execute the command below to see the scoreboard" |
| API Reference | Neutral, precise | "Returns a JSON object containing..." |
| Troubleshooting | Supportive | "Check that your API key is correct" |
| Errors | Clear, actionable | "Include all required fields: title, severity, estimated_fix_time" |

Iteration and improvements

V1 > V2

## Table of Contents

```bash
1. Overview............................1
2. Quick Visual........................1
3. How It Works........................3
4. Getting Started.....................4
5. API Reference.......................5
   - Create Debt.......................5
   - List Debts........................7
   - Update Debt.......................9
   - Scoreboard.......................10
6. Troubleshooting....................11
7. FAQ................................12
```

# Technical Debt Tracker API v1

Tech Debt Tracker is an API that helps development teams log, measure and monitor technical debt across their projects. Track your technical debt in a scoreboard in a gamifying and easy way. View your technical debt all in one place and track priorities on things like who is taking charge of it and how much time is required to complete it.

## Key Features

- Scoreboard showcasing technical debt all in one screen
- Severity levels to prioritise what comes next
- Estimated fix time in hours

## Who is this for

- Developer looking to have control over tasks
- DevOps tracking workflow
- Manager who needs to prioritise tasks for sprints

## **Quick Visual**

The Tech Debt scoreboard gives details as issue name, age, severity and current estimate.

```jsx
╔════════════════════════════════════════════════════════════════════════════╗
║  TECH DEBT SCOREBOARD                                                      ║
╠════════════════════════════════════════════════════════════════════════════╣
║                                                                            ║
║  📊 OVERVIEW                                                               ║
║  ┌────────────────────┬────────────────────┬──────────────────────────┐    ║
║  │  Total Debt        │  Active Issues     │  Resolved This Month     │  ║
║  │  1,247 hours       │  47 entries        │  12 entries              │  ║
║  └────────────────────┴────────────────────┴──────────────────────────┘  ║
║                                                                            ║
║  🏆 BIGGEST DEBTS                                                          ║
║  ┌────────────────────────────────────────────────────────────────────┐  ║
║  │ Issue                          │ Age    │ Severity │ Estimate      │  ║
║  ├────────────────────────────────┼────────┼──────────┼───────────────┤  ║
║  │ Legacy Authentication System   │ 847d   │ ⚫ Level 5│ 412 hours     │  ║
║  │ backend-team                   │        │          │ (was 40h)     │  ║
║  ├────────────────────────────────┼────────┼──────────┼───────────────┤  ║
║  │ Monolithic User Service        │ 612d   │ 🔴 Level 4│ 289 hours     │  ║
║  │ platform-team                  │        │          │ (was 100h)    │  ║
║  ├────────────────────────────────┼────────┼──────────┼───────────────┤  ║
║  │ jQuery Dependencies            │ 523d   │ 🟠 Level 3│ 156 hours     │  ║
║  │ frontend-team                  │        │          │ (was 60h)     │  ║
║  └────────────────────────────────┴────────┴──────────┴───────────────┘  ║
║                                                                            ║
║  🎉 RECENTLY RESOLVED                                                      ║
║  ┌────────────────────────────────────────────────────────────────────┐  ║
║  │ ✅ Migrated to TypeScript                                | 2 days ago │  ║
║  │    Sarah K. • frontend-team                                         │  ║
║  ├────────────────────────────────────────────────────────────────────┤  ║
║  │ ✅ Removed IE11 Support                                  | 5 days ago │  ║
║  │    Mike L. • frontend-team                                          │  ║
║  └────────────────────────────────────────────────────────────────────┘  ║
║                                                                            ║
╚════════════════════════════════════════════════════════════════════════════╝
```

## **How it works**

Below is the flow of use for the API.

The Tech Debt Tracker API follows this workflow:

**Step 1: Log a Debt Entry**
Use POST /v1/debts to create a new technical debt entry. Provide:

- Title and description
- Severity level (1-5)
- Estimated fix time in hours

**Step 2: Track Automatically**
Once created, the API automatically:

- Calculates age in days
- Grows the estimate based on severity
- Updates the scoreboard in real-time

**Step 3: Watch Scoreboard**
View your debt via GET /v1/scoreboard/global to see:

- Biggest debts ranked by current estimate
- Team rankings
- Recently resolved items

```bash
1. Log Debt Entry          2. Track Automatically       3. Watch Scoreboard

         **↓**                          **↓**                	            **↓**

"Legacy auth system"      Estimates grow over time     See team rankings,

40 hours, Severity 3   **→**  based on age & severity  **→**   biggest debts, recent wins
```

## **Getting Started**

To get you up to speed, here is what you need to start:

### Requirements

To use the Tech Debt Tracker API, you will need:

- **API Key** - Sign up for free at app.techdebttracker.io/signup
- **HTTPS support** - All API requests must be made over HTTPS
- **JSON support** - Requests and responses use JSON format

## Authentication

The API uses API key authentication. Include your key in the request header:

```bash
bash

Authorization: Bearer YOUR_API_KEY
```

**Example request to start:**

Execute the command below replacing the YOUR_API_KEY parameter to see the scoreboard.

```bash
curl https://api.techdebttracker.io/v1/scoreboard/global \
  -H "Authorization: Bearer YOUR_API_KEY"
```

## API Reference

### **Create Debt Management API:**

Create a technical debt entry.

- POST /v1/debts (create)

Query parameters

| **Field**               |  **Type**     |  **Required**  |  **Description** |
| --- | --- | --- | --- |
| title               |  string   |  Yes       |  Name of the technical debt |
| description         |  string   |  No        |  Explanation of the issue |
| severity            |  integer  |  Yes       |  Severity level (1-5) |
| estimated_fix_time  |  integer  |  Yes       |  Estimated hours to fix |
| team                |  string   |  No        |  Team responsible |
| category            |  string   |  No        |  Category type |

**Example request:**

```json
curl -X POST https://api.techdebttracker.io/v1/debts \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Replace deprecated AuthLib",
    "description": "Currently using AuthLib v2.1 with security vulnerabilities",
    "severity": 4,
    "estimated_fix_time": 80,
    "team": "backend-team",
    "category": "security"
  }'
```

**Response:**

```json
**Response: 201 Created**
{

"id": "debt_abc123",

"title": "Replace deprecated AuthLib",

"description": "Currently using AuthLib v2.1 with security vulnerabilities",

"severity": 4,

"estimated_fix_time": 80,

"team": "backend-team",

"category": "security",

"status": "active",

"created_at": "2026-01-21T10:00:00Z",

"age_in_days": 0

}
```

**Error:**

```json
{

"error": {

"code": "INVALID_API_KEY",

"message": "The API key provided is invalid or has expired"

}

}
```

### **Get Debt Management API:**

List your technical debt entries to find more information about severity, team and status.

- GET /v1/debts (list)

Query parameters

| **parameter** | type | **description** |
| --- | --- | --- |
| category | string | Filter by category (e.g., "security", "legacy-code"). Optional. |
| severity | integer | Filter by severity level (1-5). Optional. |
| team | string | Filter by team name (e.g., "backend-team"). Optional. |
| status | string | Filter by status: "active" or "resolved". Optional. |

**Example request:**

```json
GET /v1/debts?severity=4&team=backend-team
```

**Response:**

```json

{

"debts": [

{

"id": "debt_abc123",

"title": "Replace deprecated AuthLib",

"severity": 4,

"current_estimate": 85,

"age_in_days": 5,

"team": "backend-team",

"status": "active"

},

{

"id": "debt_xyz789",

"title": "Refactor user service",

"severity": 4,

"current_estimate": 120,

"age_in_days": 45,

"team": "backend-team",

"status": "active"

}

],

"total": 2

}
```

Error:

### **Update Debt Management API:**

Update technical debt to “resolved”.

- PUT /v1/debts/{id} (resolve)

Query parameters

| **parameter** | type | **description** |
| --- | --- | --- |
| category | string | Filter by category (e.g., "security", "legacy-code"). Optional. |
| severity | integer | Filter by severity level (1-5). Optional. |
| team | string | Filter by team name (e.g., "backend-team"). Optional. |
| status | string | Filter by status: "active" or "resolved". Optional. |

**Example request:**

```bash
PUT /v1/debts/debt_abc123

Authorization: Bearer sk_live_abc123xyz

Content-Type: application/json

{

"status": "resolved"

}
```

**Response:**

```bash
{

"id": "debt_abc123",

"title": "Replace deprecated AuthLib",

"status": "resolved",

"resolved_at": "2026-01-21T12:00:00Z",

"resolved_by": "@sarah-k",

"original_estimate": 80,

"final_estimate": 85,

"age_in_days": 5

}
```

Error

```bash

```

### **List Scoreboard API:**

Your view of the scoreboard containing information like the status, eta to fix, category and team of the technical debt registered.

- GET /v1/scoreboard/global (main view)

**Example request:**

```bash
curl https://api.techdebttracker.io/v1/scoreboard/global \
  -H "Authorization: Bearer YOUR_API_KEY"
```

**Response:**

```bash
Response: 200 OK
{
  "overview": {
    "total_debt_hours": 1247,
    "active_issues": 47,
    "resolved_this_month": 12
  },
  "biggest_debts": [...],
  "recently_resolved": [...]
}
```

The view of the most recent technical debt fixes.

- GET /v1/scoreboard/recent-fixes (wins)

**Example request:**

```bash
curl https://api.techdebttracker.io/v1/scoreboard/recent-fixes \
  -H "Authorization: Bearer YOUR_API_KEY"
```

**Response:**

```bash
Response: 200 OK
{
  "recent_fixes": [
    {
      "id": "debt_done001",
      "title": "Migrated to TypeScript",
      "description": "Converted entire frontend codebase from JavaScript to TypeScript for better type safety and developer experience",
      "severity": 4,
      "original_estimate": 120,
      "final_estimate": 125,
      "age_in_days": 89,
      "resolved_by": "@sarah-k",
      "team": "frontend-team",
      "category": "modernization",
      "resolved_at": "2026-01-19T14:00:00Z",
      "created_at": "2025-10-22T10:00:00Z",
      "days_ago": 2
    }
```

## Troubleshooting

We will look at the main errors you can get when using the Technical Debt Scoreboard API and how to solve it.

### 401 Unauthorized - Invalid API Key

**Error:**

```bash
{
  "error": {
    "code": "INVALID_API_KEY",
    "message": "The API key provided is invalid or has expired"
  }
}
```

**Solution:**

- Check that your API key is correct
- Ensure you're using "Bearer YOUR_API_KEY" format
- Verify key hasn't expired in your dashboard

### 400 Bad Request - Missing Parameter

**Error:**

```bash
{
  "error": {
    "code": "MISSING_REQUIRED_FIELD",
    "message": "estimated_fix_time is required",
    "field": "estimated_fix_time"
  }
}
```

**Solution:**

- Include all required fields: title, severity, estimated_fix_time
- Check JSON formatting is correct

## FAQ

**Q: How is the estimate growth calculated?**

A: Estimates grow based on severity level and age. Each severity level has a monthly growth rate: Level 1 (2% per month), Level 2 (5%), Level 3 (10%), Level 4 (15%), and Level 5 (20%). The API automatically recalculates estimates based on how long the debt has existed. For example, a 40-hour Level 3 debt becomes 88 hours after 12 months.

**Q: Can I delete a debt entry permanently?**

A: No, you cannot permanently delete debt entries. Instead, you mark them as "resolved" by updating the status field to "resolved". This preserves the historical record and allows you to track resolved technical debt over time.

**Q: What's the difference between severity levels?**

A: Severity levels range from 1 to 5 and indicate both urgency and how quickly the estimate grows:

- **Level 1 - Minor:** Small issues with minimal impact. Growth rate: 2% per month. Examples: Code style inconsistencies, minor optimisation opportunities.
- **Level 2 - Moderate:** Issues that should be addressed soon. Growth rate: 5% per month. Examples: Outdated dependencies with no security risks, redundant code.
- **Level 3 - Significant:** Issues affecting maintainability. Growth rate: 10% per month. Examples: Missing test coverage, deprecated APIs still in use.
- **Level 4 - Critical:** Issues that impact stability or security. Growth rate: 15% per month. Examples: Security vulnerabilities, architectural problems blocking features.
- **Level 5 - Catastrophic:** Urgent issues requiring immediate attention. Growth rate: 20% per month. Examples: Active security exploits, system-wide architectural failures, blocking production deployments.

**Q: Is there a rate limit?**

A: Yes, the API has rate limits based on your plan:

- **Free tier:** 100 requests per hour
- **Pro tier:** 1,000 requests per hour
- **Enterprise tier:** 10,000 requests per hour or custom limits

**Q: Can I export my data?**

A: Use `GET /v1/debts` to retrieve all debt entries in JSON format. You can filter by team, severity, status, or category to get specific subsets of data.
