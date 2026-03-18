---
name: discover
description: Searches for companies matching specific criteria like industry, size, location, and technologies. Use when the user wants to find companies, build a target list, or search for businesses in a market segment. This is a free operation that does not consume credits.
user-invocable: true
argument-hint: fintech startups in France with 50-200 employees
---

# Discover

Search for companies matching any criteria. This is completely free -- no credits consumed.

## Examples

- `/hunter:discover fintech startups in France`
- `/hunter:discover SaaS companies using Salesforce`
- `"Find healthcare companies in Germany with 100+ employees"`
- `"Companies similar to Notion"`
- `"Series B startups in Europe"`
- `"Tech companies in San Francisco with 50-200 people"`

## Steps

1. **Pass the user's query directly** to the `Discover` tool as the `query` parameter. The tool accepts natural language and handles parsing.

2. **Present the results:**

```
# Discover: Fintech Startups in France

**Found:** 43 companies | **Showing:** 10

| Company | Domain | Industry | Size | Location |
|---------|--------|----------|------|----------|
| Qonto | qonto.com | Fintech | 150 | Paris, FR |
| Pennylane | pennylane.com | Fintech | 120 | Paris, FR |
| Swan | swan.io | Fintech | 95 | Paris, FR |
| Spendesk | spendesk.com | Fintech | 180 | Paris, FR |
| ... | ... | ... | ... | ... |

## Next Actions
1. Show more results
2. Find contacts at one of these companies
3. Enrich a company for more details
4. Narrow the search (e.g., "only companies using React")
5. Find similar companies (e.g., "companies like Qonto")
```

3. **If results are too broad** (hundreds of companies), suggest narrowing: "That's a broad search. Try adding filters like industry, company size, location, or technology to narrow the results."

4. **If zero results,** suggest loosening criteria: "No companies found matching those exact criteria. Try broadening your search -- for example, expand the location or employee range."

5. **Remind users this is free.** Encourage exploration: "Discover is free -- feel free to refine your search as many times as you'd like."

## Success Criteria

At least one company returned matching the user's criteria.
