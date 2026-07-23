# Hunter for Claude

Find and verify professional email addresses, search contacts by domain, enrich company and person data, discover companies and people, organize leads and lists, run outreach sequences, and push to your CRM -- all through natural language in Claude.

## Installation

Install from the Claude plugin directory, or upload manually via **Settings > Plugins > Upload custom plugin** in Claude Cowork.

## Authentication

When you first use a Hunter tool, you'll be prompted to connect your Hunter account. The MCP server handles authentication -- just follow the prompts to authorize access with your Hunter API key.

Don't have an account? [Sign up for free](https://hunter.io/users/sign_up) -- the Free plan includes 25 monthly searches and 50 verifications.

## Skills

Hunter provides 13 skills that trigger automatically based on your requests:

| Skill | What it does | Try saying... |
|-------|-------------|---------------|
| **Email Finder** | Find someone's email from their name and company | "Find Jane Smith's email at Stripe" |
| **Email Verifier** | Check if an email is deliverable | "Is jane@stripe.com valid?" |
| **Domain Search** | List all contacts at a company | "Who works at notion.com?" |
| **Company Enrichment** | Get company details from a domain | "Tell me about acme.com" |
| **Person Enrichment** | Get person details from an email | "What do you know about jane@stripe.com?" |
| **Discover** | Find companies by criteria and size their contacts (free) | "Fintech startups in France, 50-200 employees" |
| **Prospecting** | Full end-to-end pipeline | "Build a prospect list of CTOs at fintech startups" |
| **Build Sequences** | Create and run email outreach sequences | "Set up a 3-step sequence for my leads and start it" |
| **Manage Leads** | Create, tag, and organize leads | "Tag these contacts 'priority'" |
| **Lead Lists** | Build and organize leads lists | "Save these to a Q2 Outreach list" |
| **Company Lists** | Save and organize target accounts | "Save these companies to Target Accounts" |
| **Push to CRM** | Sync leads to your connected CRM | "Push my Q2 leads to HubSpot" |
| **Check Usage** | See your plan and remaining credits | "How many credits do I have left?" |

## Example Workflows

### Sales Rep Researching a Target Account

> "Tell me about stripe.com, then find their VP of Sales and verify the email."

Claude will chain Company Enrichment -> Domain Search -> Email Finder -> Email Verifier automatically.

### Founder Building an Outbound List

> "Find fintech startups in France with 50-200 employees, then get the CTOs' email addresses."

Claude will use Discover to find companies, Domain Search to find contacts, and filter for CTOs.

### Marketer Finding Contacts for a Campaign

> "Find marketing leaders at SaaS companies in Germany. Verify all their emails."

Claude will chain Discover -> Domain Search -> Email Verifier, confirming credit usage before verification.

### SDR Launching Outreach

> "Add my fintech CTO list to a new sequence with a 3-day follow-up, then start it."

Claude will use Build Sequences to create the sequence, add a follow-up step and message template, and add recipients from your list. Because the introduction email (step 0) has no API yet, Claude points you to the Hunter dashboard to write it, then starts the sequence once it's authored -- confirming before any email goes out.

## Credits

| Operation | Credit Cost |
|-----------|------------|
| Domain Search | 1 credit per 10 results |
| Email Finder | 1 credit |
| Email Verifier | 1 credit |
| Company / Person / Combined Enrichment | 1–2 credits each (plan-dependent) |
| **Discover** (companies & people) | **Free** |
| **Email Count** | **Free** |
| **Leads, lists, sequences, CRM push, saved searches, usage** | **Free** |

Claude will always confirm before running operations that consume credits in bulk, and before any destructive action (deleting leads or lists, or pushing to your CRM).

## Links

- [Hunter for Claude](https://hunter.io/claude-plugin)
- [Hunter.io](https://hunter.io)
- [API Documentation](https://hunter.io/api-documentation)
- [Support](https://hunter.io/support)
