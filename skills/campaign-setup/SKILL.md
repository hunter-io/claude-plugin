---
name: campaign-setup
description: Prepares an email campaign by adding recipients from your leads. Use when the user wants to set up a campaign, add recipients to a campaign, or prepare for outreach.
user-invocable: true
argument-hint: Add my fintech leads to the Q2 Outreach campaign
---

# Campaign Setup

Add recipients to a Hunter campaign and prepare it for sending.

## Examples

- `/hunter:campaign-setup Add my fintech leads to the Q2 Outreach campaign`
- `"Set up my campaign with these contacts"`
- `"Add the leads from my SaaS list to campaign 12345"`
- `"Prepare the outreach campaign"`

## Workflow

### Step 1: Identify the Campaign

- If the user provides a campaign ID or name, use it directly.
- Otherwise, call `List-Campaigns` to show available campaigns and ask the user to choose.

```
# Your Campaigns

| ID | Name | Status | Recipients |
|----|------|--------|------------|
| 123 | Q2 Outreach | draft | 0 |
| 456 | Product Launch | running | 150 |

Which campaign would you like to add recipients to?
```

### Step 2: Identify Recipients

Determine the source of recipients:

- **From a leads list** — call `List-Leads` with `leads_list_id` to get the emails.
- **From specific emails** — the user provides email addresses directly.
- **From lead IDs** — the user provides lead IDs.
- **From a previous search** — use contacts already found.

### Step 3: Add Recipients

Call `Add-Campaign-Recipients` with the campaign ID and email addresses or lead IDs. Max 50 per request — batch larger lists automatically.

Report progress: "Adding batch 1 of 3 (50 recipients)..."

### Step 4: Present Summary

```
# Campaign Ready: [Campaign Name]

**Recipients added:** [count]

View campaign: https://hunter.io/campaigns/{campaign_id}

## Important
Before starting the campaign, verify in Hunter that:
- Email subject and body are configured
- A sending email account is connected
- Follow-up steps are set up (if desired)

Campaign creation and editing must be done in the Hunter UI:
https://hunter.io/campaigns/{campaign_id}

## Next Steps
1. Review and edit the campaign in Hunter
2. Start the campaign (Start-Campaign) — once subject, body, and sender are configured
3. Check campaign status (List-Campaigns)
4. Add more recipients
```

## Credit Cost

Free — adding recipients to a campaign does not consume credits.

## Important Notes

- Max 50 recipients per `Add-Campaign-Recipients` call — batch larger lists
- Campaign creation, subject/body editing, and follow-up configuration are done in the Hunter UI (not available via API)
- `Start-Campaign` will fail if the campaign lacks a subject, body, or connected email account
- Use `List-Campaign-Recipients` to check who is already in the campaign
- Use `Remove-Campaign-Recipients` to remove contacts if needed
