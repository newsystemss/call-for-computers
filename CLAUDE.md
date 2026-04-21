# Claude Code Project Guide: Call for Computers

## Overview

Custom submission form for casting subjects who are exploring how computers can be used in unique and new ways. Static HTML/CSS/JS frontend posting to a Google Apps Script backend. Intended to be deployed at computers.newsystems.ca via Netlify.

Forked from `new-stadium-brief` (sibling project) on 2026-04-21. Same design system, same architecture, different Step 2 questions and destination address.

## Architecture

```
Browser (computers.newsystems.ca)
  |
  | POST (JSON, text/plain content-type)
  v
Google Apps Script (Web App)
  |
  |--- Append row to Google Sheet
  |--- Send confirmation email to submitter
  |--- Send notification email to tommy@newsystems.ca
```

No build step. No framework. Three files serve the entire frontend.

## Key Files

| File | Purpose | When to modify |
|------|---------|----------------|
| `index.html` | Form structure, field definitions, progress nav | Adding/removing/reordering questions |
| `style.css` | All styles, responsive breakpoints, animations | Visual changes, spacing, colors |
| `form.js` | Validation, step navigation, submission logic, textarea auto-expand | Changing form behavior, adding fields to payload |
| `apps-script/Code.gs` | Backend: Sheet append, email sending, rate limiting, validation | Changing email content, adding Sheet columns, adjusting rate limits |
| `netlify.toml` | Deploy config, security headers | Changing redirects or headers |

## Form structure

**Step 1 (Contact):** firstName, lastName, email (required); twitter, instagram, website, phone, company, referral (optional).

**Step 2 (Your Work):**
1. `work` (required) - what they're working on / how their relationship with computers has changed
2. `tools` (required) - software, hardware, or custom tools central to how they work
3. `links` (optional) - links to work or documentation
4. `recommendations` (required) - others exploring how computers can be used in unique and new ways

**Step 3:** Confirmation.

## Adding a new form field

1. Add the HTML in `index.html` (inside the appropriate `form-step` section)
2. Add a `maxlength` attribute
3. In `form.js`, add the field to the `data` object in the submit handler
4. In `apps-script/Code.gs`:
   - Add the field to the `appendRow` call (maintain column order)
   - Add to `required` array if mandatory
   - Update the team notification email template
   - Update `setupSheet()` headers array
5. Manually add the column header to the existing Sheet, or re-run `setupSheet()` on a fresh Sheet
6. Redeploy both the frontend (git push) and the Apps Script (paste + new version)

## Design System

- **Font:** Inter (weight 400 body, 500 headings, 14px base)
- **Colors:** `--bg: #fafafa`, `--text: #343c3c`, `--text-muted: #5e6666`, `--accent: #abb495`, `--error: #8b3a3a`
- **Layout:** Pinned top-left, max-width 680px
- **Inputs:** Bottom-border only, proportional widths (short 160px, medium 260px, long full-width textarea)
- **Buttons:** Styled as underlined text links, not boxed
- **Transitions:** 150ms ease fade between steps
- **Mobile breakpoint:** 600px (all inputs go full-width)

## Backend Details

- **Apps Script URL:** Hardcoded in `form.js` line 1 (empty until deployed)
- **CSRF token:** `ns-casting-2026` (must match between `form.js` and `Code.gs`)
- **Rate limits:** 5 min cooldown per email, 50 submissions/hour global
- **Field truncation:** 5000 chars max per field (server-side)
- **Emails sent from:** the Google account that deployed the Apps Script
- **Team notifications go to:** tommy@newsystems.ca

## Common Tasks

**Update form copy:** Edit `index.html`, commit, push.

**Update email templates:** Edit `apps-script/Code.gs`, copy contents, paste into Apps Script editor, Deploy > Manage deployments > Edit > New version > Deploy.

**Rotate CSRF token:** Change `FORM_TOKEN` in `Code.gs` and `_token` value in `form.js` data object. Redeploy both.

**Check submissions:** Open the Google Sheet directly, or check tommy@newsystems.ca inbox.

## Do NOT Modify

- The `setupSheet()` function column order without updating `appendRow` to match
- The `Content-Type: text/plain` header in the fetch call (prevents CORS preflight)

## Session Context

### 2026-04-21
Forked from `new-stadium-brief`. Swapped Step 2 questions from event-brief framing to casting-call framing (work, tools, links, recommendations). Changed team email to tommy@newsystems.ca, CSRF token to `ns-casting-2026`, submission ID prefix to `NC-`, source tag to `casting.newsystems.ca`. Removed budget/date/trade fields from the form and backend.

### Outstanding
- Apps Script not yet deployed, so `APPS_SCRIPT_URL` in `form.js` is empty
- Google Sheet not yet created
- Netlify site not yet created
- DNS CNAME `casting.newsystems.ca` not yet set
- GitHub repo not yet created
- See `DEPLOY.md` for step-by-step instructions
