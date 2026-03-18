---
name: domain-search
description: Finds all publicly available email addresses and contacts at a company domain. Use when the user asks who works at a company, wants to find contacts at a domain, or needs email addresses for an organization.
user-invocable: true
argument-hint: stripe.com
---

# Domain Search

Find all contacts and email addresses associated with a company domain.

## Examples

- `/hunter:domain-search stripe.com`
- `/hunter:domain-search Notion`
- `"Who works at figma.com?"`
- `"Find me contacts at Salesforce"`
- `"Show me the marketing team at hubspot.com"`
- `"List executives at acme.com"`

## Steps

1. **Parse the input.** Extract the `domain`.
   - "stripe.com" -> use directly
   - "Stripe" or "stripe" -> infer domain as "stripe.com"
   - If unsure about the domain, ask the user to confirm.

2. **Call `Domain-Search`** with the `domain`. The API supports these optional parameters for server-side filtering (more efficient than filtering after the fact):
   - `type`: `personal` or `generic`
   - `seniority`: comma-separated from `junior`, `senior`, `executive`
   - `department`: comma-separated from `executive`, `it`, `finance`, `management`, `sales`, `legal`, `operations`, `customer_success`, `business_development`, `marketing`, `hr`, `support`, `logistics`, `procurement`, `engineering`, `product`, `design`, `healthcare`, `media_communication`, `teaching`, `security`
   - `limit`: 1-100 (default 10)
   - `offset`: for pagination

   **Note:** The MCP tool currently only exposes the `domain` parameter. Use client-side filtering for now on the returned results:
   - "marketing team" -> show only contacts with marketing-related positions
   - "executives" / "C-suite" / "leadership" -> show contacts with senior titles (CEO, CTO, VP, Director, etc.)
   - "engineering" -> show contacts with engineering/technical positions
   - No filter specified -> show the top contacts by confidence score

4. **Present the results:**

```
# Contacts at Stripe (stripe.com)

**Found:** 247 contacts | **Showing:** 10

| Name | Position | Email | Confidence |
|------|----------|-------|------------|
| Patrick Collison | CEO | patrick@stripe.com | 97% |
| John Collison | President | john@stripe.com | 95% |
| David Singleton | CTO | david.singleton@stripe.com | 88% |
| ... | ... | ... | ... |

## Next Actions
1. Show more contacts
2. Filter by department (e.g., "show me the marketing team")
3. Verify all email addresses (uses 0.5 credits per email)
4. Enrich Stripe with company details
```

5. **If the user asks for more,** present the next batch of results.

6. **If zero results,** suggest:
   - Check the domain spelling
   - The company may be too new or too small for our database
   - Try `Discover` to find similar companies

## Success Criteria

At least one contact returned with name, email, and confidence score.
