---
name: list-builder
description: Creates and populates a Hunter leads list from a set of contacts or domains. Use when the user wants to build a lead list, organize contacts into a list, or save search results to Hunter.
user-invocable: true
argument-hint: Create a list of marketing leads from SaaS companies
---

# List Builder

Create a Hunter leads list and populate it with contacts from search results, enrichment, or manual input.

## Examples

- `/hunter:list-builder Create a list of marketing leads from SaaS companies`
- `"Save these contacts to a new list called Q2 Outreach"`
- `"Build a list from the domain search results"`
- `"Organize my prospects into a leads list"`

## Workflow

### Step 1: Determine the Source

Parse the user's request to identify where leads come from:

- **From a previous search** — use contacts already found via Domain-Search or Discover.
- **From specific emails/contacts** — the user provides email addresses directly.
- **From a new search** — run Discover + Domain-Search first, then save results.

### Step 2: Create the List

Call `Create-Leads-List` with a descriptive name. If the user doesn't provide a name, suggest one based on the context (e.g., "Fintech CTOs - France - 2026-04-08").

Present the deep-link: "List created: https://hunter.io/leads?leads_list_id={id}"

### Step 3: Add Leads

For each contact, call `Upsert-Lead` with the contact's data and the new `leads_list_id`. Use `Upsert-Lead` (not `Create-Lead`) to avoid duplicates.

Include all available fields: `email`, `first_name`, `last_name`, `position`, `company`, `linkedin_url`, etc.

Report progress: "Adding lead 5 of 20..."

### Step 4: Present Summary

```
# List Created: [List Name]

**Leads added:** [count] | **Duplicates skipped:** [count]

View in Hunter: https://hunter.io/leads?leads_list_id={id}

## Next Steps
1. Add more leads to this list
2. Add leads to a campaign (Add-Campaign-Recipients)
3. Merge with another list (Merge-Leads-Lists)
4. Search for more contacts (Domain-Search or Discover)
```

## Credit Cost

Free — Create-Leads-List and Upsert-Lead do not consume credits. Only the initial search (Domain-Search, Email-Finder) uses credits if leads come from a new search.

## Important Notes

- Use `Upsert-Lead` to avoid creating duplicate leads
- Use `Lead-Exists` to check before adding if unsure
- Max 100 leads per page when listing — use offset to paginate
- Lists can be merged later with `Merge-Leads-Lists`
