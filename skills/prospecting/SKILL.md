---
name: prospecting
description: Runs end-to-end B2B prospecting by chaining company discovery, contact search, email verification, and enrichment. Use when the user wants to build a prospect list, find and qualify leads, or run a full prospecting pipeline.
user-invocable: true
argument-hint: Find CTOs at fintech startups in Europe
---

# Prospecting

Chain Hunter tools into a complete prospecting workflow. Discover companies, find contacts, verify emails, and enrich -- all in one go.

## Examples

- `/hunter:prospecting Find CTOs at fintech startups in France`
- `/hunter:prospecting decision-makers at Stripe, Notion, and Figma`
- `"Build me a list of marketing leads at SaaS companies in Germany"`
- `"I need 20 VPs of Sales at mid-size tech companies"`
- `"Find people to reach out to at companies using Salesforce in healthcare"`

## Workflow

### Step 1: Identify Companies

Parse the user's request to determine the starting point:

- **Specific companies provided** (e.g., "Stripe, Notion, Figma") -- skip to Step 2.
- **Criteria provided** (e.g., "fintech startups in France") -- call `Discover` with the criteria as the `query` parameter.

If `Discover` returns more than 10 companies, present the full list and ask:

> "I found [N] companies matching your criteria. Here are the top results. Which ones should I search for contacts? You can select specific companies or say 'proceed with all' (note: each Domain Search uses 1 credit per 10 results returned)."

### Step 2: Find Contacts

For each company, call `Domain-Search` with the company's `domain`.

Filter the results based on the user's request:
- "CTOs" or "engineering leaders" -- filter by position keywords
- "marketing team" -- filter by marketing-related positions
- "executives" or "C-suite" -- filter by seniority-related titles

Report progress for multi-company searches: "Searching stripe.com... found 15 contacts. Moving to notion.so..."

### Step 3: Verify Emails

> Before verifying, confirm credit usage: "I found [N] contacts across [M] companies. Verifying all emails will use [N × 0.5 = X] credits. Proceed?"

Only verify after the user confirms. Call `Email-Verifier` for each contact's `email`.

If the user says "skip verification," present unverified results instead.

### Step 4: Enrich (Optional)

If the user asks for more company context, call `Company-Enrichment` for each company's `domain`. Only run this step if requested -- do not run by default.

### Step 5: Present Results

Present a consolidated table grouped by company:

```
# Prospect List: [Description]

**Companies:** [count] | **Contacts:** [count] | **Verified:** [deliverable] deliverable, [risky] risky

## [Company Name] (domain.com)
**Industry** | **Size** | **Location**

| Name | Position | Email | Verified |
|------|----------|-------|----------|
| ... | ... | ... | valid / accept_all / invalid / unknown |

## Next Steps
1. Copy this list to your CRM
2. Export as CSV
3. Verify the risky addresses again later
4. Search for more companies with different criteria
```

## Important Notes

- Each `Domain-Search` call uses 1 credit per 10 results returned
- Each `Email-Verifier` call uses 0.5 credits
- `Discover` is free -- encourage the user to explore and refine criteria
- `Company-Enrichment` uses 0.2 credits per call
- Always confirm before running verification on large batches
- If a company returns zero contacts, skip it and note it in the output
- If the user interrupts mid-workflow, present partial results gathered so far
