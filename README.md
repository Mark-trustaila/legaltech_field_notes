# Legaltech Field Notes — landing page

Static landing page for LFN. Pure HTML, no build step. Deploys to Vercel via Git.

## Files

- `index.html` — the page (all CSS inline)
- `lfn-logo.jpg` — brand mark, used in nav, as favicon, and as social-share image
- `vercel.json` — caching and security headers
- `package.json` — project metadata
- `.gitignore` — keeps OS and editor files out of the repo

## Deploy

1. Push this folder to a new GitHub repo
2. Import the repo at <https://vercel.com/new>
3. Vercel will detect it as a static project, no build settings needed
4. Every push to `main` (or whichever branch you nominate) auto-deploys

## Custom domain

In the Vercel dashboard, once deployed:

1. Project → Settings → Domains
2. Add the domain (e.g. `legaltechfieldnotes.com`)
3. Vercel will give you DNS records to add at your registrar
4. SSL is provisioned automatically

## Editing the page

All text and styling lives in `index.html`. The structure is:

- `<head>` — meta tags, fonts, all CSS in a single `<style>` block
- `<nav>` — top bar with logo and links
- `<header class="hero">` — top headline and lede
- Then sections in order: facts strip, concept, format, principles, get-involved, FAQ, signup, footer

The italic accent colour is set in the CSS variable `--accent` near the top of the `<style>` block.

## Form handling

The signup form posts to Formspark (a hosted form-handling service — no backend code needed on this site).

- **Provider:** [Formspark](https://formspark.io)
- **Form name:** LFN initial signup list
- **Form ID:** `vVRh02hEd`
- **Action URL:** `https://submit-form.com/vVRh02hEd`
- **Notifications:** email goes to the address set in the Formspark form's Settings tab

### Where submissions go

Submissions appear in the Formspark dashboard under the "LFN initial signup list" form, and trigger an email notification to whichever address is configured under the form's Settings → Email notifications.

To view, export, or change notification settings, log into Formspark and open the form.

### How the form is wired

The form uses JavaScript (`fetch`) to submit asynchronously so the visitor stays on the page and sees an inline "Thank you" message rather than being redirected to a Formspark page. If JavaScript is disabled, the form falls back to a normal POST submission and Formspark will display its own success page.

Field names sent to Formspark:

- `name` — full name
- `email` — email address
- `role` — role and organisation
- `intent` — one or more values from the checkbox group
- `message` — free-text "anything else" field

### Switching providers later

To switch to another form provider (Tally, Mailchimp, Netlify Forms, etc.):

1. Update the `<form action="...">` URL in `index.html`
2. Adjust field `name` attributes if the new provider requires different naming
3. Adjust or remove the `lfnSubmit` JavaScript function if the new provider needs different submission handling
