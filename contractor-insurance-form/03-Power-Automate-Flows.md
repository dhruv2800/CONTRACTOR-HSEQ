# 03 — Power Automate flows

Two flows. Build them at **make.powerautomate.com**. No code required — all standard actions.

- **Flow #1** turns each Form response into a register row + notifies you.
- **Flow #2** runs daily, emails expiry reminders, and flips Status to Non-Compliant when a cert lapses.

---

## Flow #1 — "Contractor form → Register row"

**Trigger:** *Microsoft Forms — When a new response is submitted*
→ select your form.

**Action 1:** *Microsoft Forms — Get response details*
→ Form = your form, Response Id = `Response Id` from the trigger.

**Action 2:** *SharePoint — Create item* (your Contractor Register list)
Map the fields:

| List column | Set to (dynamic content from Get response details) |
|-------------|-----------------------------------------------------|
| Contractor Name | Contractor / Company name |
| Services Provided | Services you provide |
| Transport Operator | Are you a Transport Operator? |
| Contact | Contact person name |
| Email | Contact email |
| Submission Date | `utcNow()` (or the Form submit time) |
| Public Liability Insurer | Public Liability — insurer name |
| Public Liability Expiry | Public Liability — COC expiry date |
| Workers Comp Insurer | Workers Compensation — insurer name |
| Workers Comp Expiry | Workers Compensation — COC expiry date |
| Plant & MV Insurer | Commercial Plant & MV — insurer name |
| Plant & MV Expiry | Commercial Plant & MV — COC expiry date |
| CoR Pre-Qual Completed | CoR question (or set "N/A" if Transport Operator = No) |
| Status | leave blank — Flow #2 will set it |
| Docs Uploaded | No |

> **Tip:** wrap the Plant & MV fields so a blank date doesn't error — use an expression
> like `if(empty(outputs(...)?['plantExpiry']), null, outputs(...)?['plantExpiry'])`.

**Action 3:** *Office 365 Outlook — Send an email (V2)* — to **you / HSEQ inbox**:
> Subject: `New contractor submission — <Contractor Name>`
> Body: list the details + expiry dates, and remind you to check the **Insurances** library
> for their uploaded certificates (named `<Company>_*.pdf`).

**(Optional) Action 4 — auto-folder per contractor:**
*SharePoint — Create new folder* in `Insurances` named `Contractor Name`, then
*Create sharing link* (type: **CreateOnly** / "anyone" upload) and write that URL into the
register row's **Certificates Folder** column. Use this if you skipped the single
Request-files link in file `02`.

---

## Flow #2 — "Daily insurance expiry check"

**Trigger:** *Recurrence* — every **1 day** (e.g. 7:00 AM).

**Action 1:** *SharePoint — Get items* (Contractor Register).
Optionally add an **OData filter** to only pull rows where Status ≠ archived, etc.

**Action 2:** *Apply to each* (over the returned items). Inside the loop:

For **each of the three expiry dates** (Public Liability, Workers Comp, Plant & MV),
add a **Condition** using a `daysUntil` value. Create the value with a **Compose** action:

```
formatNumber(
  div(
    sub(
      ticks(item()?['PublicLiabilityExpiry']),
      ticks(utcNow())
    ),
    864000000000          // ticks per day
  ),
  '0'
)
```

Then branch using the **manager escalation cadence** you asked for.
Set a **Compose** action `ManagerEmail = manager@yourcompany.com` at the top of the flow so
it's easy to change.

| daysUntil | Who gets emailed | What it says |
|-----------|------------------|--------------|
| `= 30` | **Manager** (cc you) | *"30-DAY NOTICE: <Contractor> — <insurance type> expires on <date>."* First heads-up. |
| `= 30` | Contractor (`Email`) | *"Your <insurance type> expires in 30 days — please renew and re-upload via <link>."* |
| `≤ 10` **and** `≥ 0` | **Manager** (cc you) — **EVERY DAY** | *"REMINDER (<daysUntil> days left): <Contractor> — <insurance type> expires on <date>. Action required."* Sends daily from 10 days out down to expiry day. |
| `≤ 10` **and** `≥ 0` | Contractor | Daily renewal nudge with the upload link. |
| `< 0` (expired) | **Manager** (cc you) | *"EXPIRED: <Contractor> — <insurance type> lapsed on <date>. Status set to Non-Compliant."* + **Update item** → Status = **Non-Compliant**. |
| `≥ 0` for all three | — | **Update item** → Status = **Compliant** (only if all current). |

**How the cadence is built (exactly what you described):**

- **30-day mark:** one Condition `daysUntil == 30` → send the manager the 30-day notice.
- **Last 10 days, every day:** one Condition `daysUntil <= 10 && daysUntil >= 0` → send the
  manager a reminder. Because the flow runs **daily** (the Recurrence trigger), this fires
  **once every day** from 10 days out until expiry — no extra setup needed.
- **On/after expiry:** Condition `daysUntil < 0` → expired alert + flip Status.

> You wanted: *30-day alert → reminder at 10 days → then every day until it expires.* The
> `daysUntil <= 10 && >= 0` branch delivers the daily run automatically. If you'd rather the
> very first 10-day reminder read differently from the subsequent daily ones, add a nested
> Condition `daysUntil == 10` for a distinct "10-day reminder" subject line.

> **Stop the daily noise after renewal:** when a contractor re-uploads and you enter a new
> expiry date, `daysUntil` jumps back above 10, so the daily emails stop on their own.

**Compliant roll-up logic** (Action at end of loop): set Status = Compliant only when
*all three* `daysUntil ≥ 0`. Easiest way: compute `minDays = min(pl, wc, plant)` with a
Compose, then one Condition: `minDays < 0 → Non-Compliant`, else `Compliant`.
(Ignore Plant & MV in the `min` if that contractor doesn't operate plant — store a far-future
date or skip it.)

---

## Reusing instead of rebuilding

Both flows are made entirely of standard connectors (Forms, SharePoint, Office 365
Outlook), so anyone in your team can open and tweak them. Keep them in a shared
**Solution** in Power Automate if you want to hand them to IT later.
