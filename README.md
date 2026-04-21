# New Casting Call Submission Form

A custom multi-step web form for collecting submissions from people exploring how computers can be used in unique and new ways. Built as a standalone static site intended to be deployed at casting.newsystems.ca.

## How it works

1. Submitter fills out a 2-step form (Contact Info > Your Work)
2. On submit, data posts to a Google Apps Script endpoint
3. The script appends a row to a Google Sheet, sends a confirmation email to the submitter, and sends a formatted notification to tommy@newsystems.ca
4. Submitter sees a confirmation screen

## Stack

- **Frontend:** Static HTML, CSS, JS (no framework, no build step)
- **Backend:** Google Apps Script (Web App deployment)
- **Data store:** Google Sheet
- **Hosting:** Netlify, auto-deploys from `main` branch
- **Domain:** casting.newsystems.ca (CNAME to Netlify)
- **Font:** Inter via Google Fonts CDN

## File structure

```
new-casting-call/
  index.html          # The form (3 steps: Contact, Your Work, Confirmation)
  style.css           # All styles (Inter, proportional inputs, mobile responsive)
  form.js             # Form logic (validation, navigation, submission, auto-expand textareas)
  netlify.toml        # Netlify deploy config + security headers
  apps-script/
    Code.gs           # Google Apps Script (paste into Script editor, not deployed from here)
  DEPLOY.md           # Step-by-step deploy instructions
  CLAUDE.md           # Claude Code project guide
  README.md           # This file
```

## Form fields

**Step 1: Contact Information**
- First name, Last name, Email (required)
- Twitter, Instagram, Personal website/portfolio, Phone, Company/Organization, Referral Source (optional)

**Step 2: Your Work**
1. What you are working on / how your relationship with computers has changed (required)
2. Software, hardware, or custom tools central to how you work (required)
3. Links to work or documentation (optional)
4. Who else is exploring how computers can be used in unique and new ways (required)

**Step 3: Confirmation**
- On-screen confirmation with email notice

## Google Sheet columns

Timestamp, Submission ID, First Name, Last Name, Email, Phone, Twitter, Instagram, Website, Company/Organization, Referral Source, Work / Relationship with Computers, Tools, Links, People Exploring New Uses, Status, Notes, Source

## Security

- CSRF token validation on the backend
- Per-email rate limiting (5 min cooldown)
- Global rate limiting (50 submissions/hour)
- Server-side input validation and field truncation (5000 char max)
- All frontend inputs have `maxlength` attributes
- HTML team notification with escaping on every interpolated field
- `noscript` fallback for JS-disabled browsers
- Netlify security headers (X-Frame-Options, X-Content-Type-Options, Referrer-Policy)

## Emails

Emails are sent from the Google account that deployed the Apps Script. The confirmation email reply-to is set to tommy@newsystems.ca.

- **To submitter:** Brief confirmation mirroring the on-screen text
- **To tommy@newsystems.ca:** Formatted summary with all submission data

## Making changes

1. Edit files locally
2. Commit and push to `main`
3. Netlify auto-deploys within seconds

To update the Apps Script backend:
1. Edit `apps-script/Code.gs` locally
2. Copy the contents
3. Paste into the Apps Script editor (Extensions > Apps Script from the Google Sheet)
4. Deploy > Manage deployments > Edit > New version > Deploy
