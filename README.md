# Hunter for Claude

Find and verify professional email addresses, search contacts by domain, enrich company data, and run full prospecting workflows -- all through natural language in Claude.

## Installation

Install from the Claude plugin directory, or upload manually via **Settings > Plugins > Upload custom plugin** in Claude Cowork.

## Authentication

When you first use a Hunter tool, you'll be prompted to connect your Hunter account. The MCP server handles authentication -- just follow the prompts to authorize access with your Hunter API key.

Don't have an account? [Sign up for free](https://hunter.io/users/sign_up) -- the Free plan includes 25 monthly searches and 50 verifications.

## Skills

Hunter provides 7 skills that trigger automatically based on your requests:

| Skill | What it does | Try saying... |
|-------|-------------|---------------|
| **Email Finder** | Find someone's email from their name and company | "Find Jane Smith's email at Stripe" |
| **Domain Search** | List all contacts at a company | "Who works at notion.com?" |
| **Email Verifier** | Check if an email is deliverable | "Is jane@stripe.com valid?" |
| **Company Enrichment** | Get company details (industry, size, location) | "Tell me about acme.com" |
| **Person Enrichment** | Get person details from their email | "What do you know about jane@stripe.com?" |
| **Discover** | Find companies by criteria (free!) | "SaaS companies in France with 50-200 employees" |
| **Prospecting** | Full end-to-end workflow | "Build me a prospect list of CTOs at fintech startups" |

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

## Credits

| Operation | Credit Cost |
|-----------|------------|
| Domain Search | 1 credit per 10 results |
| Email Finder | 1 credit |
| Email Verifier | 1 credit |
| Company Enrichment | 1 credit |
| Person Enrichment | 1 credit |
| **Discover** | **Free** |

Claude will always confirm before running operations that consume credits in bulk.

## Links

- [Hunter.io](https://hunter.io)
- [API Documentation](https://hunter.io/api-documentation)
- [Support](https://hunter.io/support)
