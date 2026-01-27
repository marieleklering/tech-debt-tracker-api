
# Technical Debt Tracker API v1

Tech Debt Tracker is an API that helps development teams log, measure and monitor technical debt across their projects. Track your technical debt in a scoreboard in a gamifying and easy way. View your technical debt all in one place and track priorities on things like who is taking charge of it and how much time is required to complete it.

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

```
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

**Response:** 🟢 `201 Created`

```
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

**Error:** 🔴 `401 Unauthorized`

```
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

```
GET /v1/debts?severity=4&team=backend-team
```

**Response:** 🟢 `200 OK`

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

**Error:** 🔴 `404 Not Found`

```json
{
  "error": {
    "code": "NO_DEBTS_FOUND",
    "message": "No debt entries match the specified filters"
  }
}
```

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

**Response:** 🟢 `200 OK`

```json
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

**Error:** 🔴 `404 Not Found`

```json
{
  "error": {
    "code": "DEBT_NOT_FOUND",
    "message": "No debt entry found with the specified ID"
  }
}
```

### **List Scoreboard API:**

Your view of the scoreboard containing information like the status, eta to fix, category and team of the technical debt registered.

- GET /v1/scoreboard/global (main view)

**Example request:**

```bash
curl https://api.techdebttracker.io/v1/scoreboard/global \
  -H "Authorization: Bearer YOUR_API_KEY"
```

**Response:** 🟢 `200 OK`

```json
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
**Error:** 🔴 `401 Unauthorized`

```json
{
  "error": {
    "code": "INVALID_API_KEY",
    "message": "The API key provided is invalid or has expired"
  }
}
```

The view of the most recent technical debt fixes.

- GET /v1/scoreboard/recent-fixes (wins)

**Example request:**

```bash
curl https://api.techdebttracker.io/v1/scoreboard/recent-fixes \
  -H "Authorization: Bearer YOUR_API_KEY"
```

**Response:** 🟢 `200 OK`

```json
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
  ]
}
```
**Error:** 🔴 `401 Unauthorized`

```json
{
  "error": {
    "code": "INVALID_API_KEY",
    "message": "The API key provided is invalid or has expired"
  }
}
```

## Troubleshooting

We will look at the main errors you can get when using the Technical Debt Scoreboard API and how to solve it.

### 🔴 `401 Unauthorized` - Invalid API Key

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

### 🔴 `400 Bad Request` - Missing Parameter

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

### 🔴 `404 Not Found` - Debt Not Found

**Error:**

```json
{
  "error": {
    "code": "DEBT_NOT_FOUND",
    "message": "No debt entry found with the specified ID"
  }
}
```

**Solution:**

- Verify the debt ID exists using GET /v1/debts
- Check for typos in the ID (IDs are case-sensitive)

### 🔴 `429 Too Many Requests` - Rate Limited

**Error:**

```json
{
  "error": {
    "code": "RATE_LIMITED",
    "message": "Too many requests. Please wait before retrying.",
    "retry_after": 60
  }
}
```

**Solution:**

- Wait for the time specified in `retry_after` (seconds)
- Implement exponential backoff in your integration
- Consider upgrading your plan for higher rate limits

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
