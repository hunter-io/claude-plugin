---
name: build-sequences
description: Builds and runs email outreach sequences end to end — create a sequence, add follow-up steps and message templates, add recipients from your leads, then start, pause, resume, or check its stats. Use when the user wants to set up outreach, build an email sequence, add recipients, launch a drip, or check sequence performance.
user-invocable: true
argument-hint: Set up a 3-step outreach sequence for my fintech leads and start it
---

# Build Sequences

Create, configure, and run Hunter email sequences from chat: create the sequence, add follow-up steps and message templates, add recipients, launch, and read stats. **One current limitation:** the sequence's introduction email (step 0) has no API yet, so its subject and body must be written once in the Hunter dashboard before the sequence can start.

## Examples

- `/hunter:build-sequences Set up a 3-step outreach sequence for my fintech leads`
- `"Create a sequence called Q2 Outreach with a follow-up after 3 days"`
- `"Add the leads from my SaaS list to the Q2 Outreach sequence"`
- `"Start the Q2 Outreach sequence"`
- `"Pause my running sequence"`
- `"How is the Product Launch sequence performing?"`

## Workflow

### Step 1: Identify or create the sequence

- Existing sequence named or given by ID → call `Get-Sequence` to load its live status, sender, settings, and recipient count before acting (start/resume/archive/add-recipients should rely on this, not stale list rows). To browse, call `List-Sequences`.
- New sequence → call `Create-Sequence` with a descriptive name. Then configure it (Steps 2–3).

```
# Your Sequences

| ID | Name | Status | Recipients |
|----|------|--------|------------|
| 123 | Q2 Outreach | draft | 0 |
| 456 | Product Launch | running | 150 |
```

### Step 2: Configure steps and templates

- **Message templates** — `Create-Message-Template` (requires a `name` and `body`; `subject` optional), reusable across sequences. `List-Message-Templates` to reuse an existing one (`Get-Message-Template` to inspect); `Update-Message-Template` to edit.
- **Follow-up steps** — `Create-Sequence-Follow-Up` to add each step (delay + template). `List-Sequence-Follow-Ups` to review the cadence (`Get-Sequence-Follow-Up` for one step); `Delete-Sequence-Follow-Up` removes a step, but **only the last step** can be deleted — to change an earlier one, delete from the end back down to it and recreate the rest (confirm first). A sequence holds at most **6 steps total** — the step-0 introduction plus up to 5 follow-ups — so longer drips aren't possible.

Use `{{first_name}}`-style merge fields in templates, but each merge field must carry a **fallback value** — `Create-Sequence-Follow-Up` rejects bare variables, so ask for or generate fallback text for every variable. Follow-up steps (1 and up) are authored here, but the **introduction email (step 0)** cannot be filled via the API yet — its subject and body must be written in the Hunter dashboard before the sequence can start.

### Step 3: Add recipients

Pull recipients from a leads list (`List-Leads` with `leads_list_id` — page through **all** results with `offset`, 100 per page, so leads past the first page aren't dropped), from specific emails/lead IDs, or from a prior search, then call `Add-Sequence-Recipients` with the sequence ID and the emails or lead IDs. Max 50 per call — batch larger lists automatically and report progress ("Adding batch 1 of 3…"). Use `List-Sequence-Recipients` to see who is already in.

### Step 4: Pre-flight and start

Before calling `Start-Sequence`, verify the launch preconditions and surface any gap instead of letting the start fail:

- The **introduction email (step 0)** has a subject and body. This can only be set in the Hunter dashboard today — so if the sequence was built purely through chat, tell the user to open it in Hunter and write the intro email first. `Start-Sequence` fails validation while step 0 is blank. For an existing sequence, verify step 0's subject/body with `List-Sequence-Follow-Ups` (step 0) — `Get-Sequence` only reports step counts, not their contents.
- The sequence has a **sender attached** — the sender is a per-sequence field set via `email_account_ids` on `Create-Sequence` / `Update-Sequence`, not merely "some connected account." Use `List-Email-Accounts` to pick one (`Get-Email-Account` for its details) and `Update-Sequence` to attach it if the sequence has none; `Start-Sequence` validates the sequence's own sender.

If a precondition is missing, say exactly what to fix. Once clear, call `Start-Sequence`.

### Step 5: Run controls and stats

- `Pause-Sequence` / `Resume-Sequence` to hold or continue.
- `Archive-Sequence` to retire a finished sequence.
- `Get-Sequence-Stats` for sent / open / reply / bounce numbers.
- `Update-Sequence` to rename or adjust settings — but once a sequence has **started**, only the name (and unsubscribe-link toggle) stay editable; sender, schedule, BCC, and tracking lock on start, so set those while it's still a draft.

```
# Sequence: Q2 Outreach

**Status:** running | **Recipients:** 120
**Sent:** 118 | **Opened:** 74 (63%) | **Replied:** 19 (16%) | **Bounced:** 2

View: https://hunter.io/sequences/{sequence_id}
```

## Guardrails

Confirm before any destructive, irreversible, or email-sending action — state the operation, the affected count, and the target, then wait for an explicit "yes":

- `Delete-Sequence` — only **draft** sequences can be deleted; a started or archived sequence returns an error (archive/stop it instead). For a draft: "This permanently deletes the '[name]' sequence. Confirm?"
- `Archive-Sequence` — stops a running sequence and can't be undone through the API: "This archives '[name]' and can't be reversed. Confirm?"
- `Remove-Sequence-Recipients` — echo how many recipients and from which sequence; max 50 per call, so batch larger removals in groups of 50 and report progress.
- `Delete-Message-Template`, `Delete-Sequence-Follow-Up` — name what is being removed.
- `Update-Message-Template` — overwrites a saved template's fields with no way to recover the old version; confirm before editing a reusable team template.

**Anything that can send email — confirm first.** `Start-Sequence`, `Resume-Sequence`, and `Add-Sequence-Recipients` on an already-started sequence all schedule real outbound email (Resume and Add-Recipients need no separate start call). Always confirm the recipient count and sending account before any of them.

## Credit Cost

Free — creating, configuring, and running sequences does not consume search or verification credits. (Finding and verifying the recipient emails beforehand does.)

## Important Notes

- A sequence cannot start until the step-0 introduction email (subject + body, authored in the Hunter dashboard — no API yet) and a connected sending account are both in place. Check before calling `Start-Sequence`.
- Max 50 recipients per `Add-Sequence-Recipients` call — batch larger lists.
- Message templates are reusable across sequences — prefer reusing over recreating.
- `List-Email-Account-Sequences` shows which sequences a given sending account is running.
