# yoDEV Beta Offer — Discourse Theme Component

A Discourse theme component that shows a site-wide, dismissable, auth-aware
Beta Offer banner on every page, plus a non-dismissable callout panel on the
`/signup` (and `/login`) page. Copy is available in English, Spanish, and
Portuguese; the active Discourse UI language is detected automatically.

## What it does

- **Banner (all pages):** A slim indigo top bar with beta offer copy, a CTA
  button, and a dismiss `×` button. Anonymous visitors see a signup CTA;
  logged-in members see a "your Business access is live" CTA pointing to the
  marketplace/app.
- **Signup panel (`/signup`, `/login`):** A bordered callout inserted above
  the signup/login form explaining the 3-month free Business offer. No dismiss
  button — it's contextual info for the form.
- **Auth awareness:** Reads `/session/current.json` on every page load to
  determine auth state and personalise copy + CTA destination.
- **Dismissal persistence:**
  - Anonymous visitors: localStorage (`yodev_beta_offer_dismissed`).
  - Logged-in members: `PUT /u/<username>.json` to write a Discourse User
    Field (cross-device). Falls back to localStorage if `dismissed_field_id`
    is not configured.

## Install

1. In Discourse Admin: **Customize → Themes → Install → From a git repository**
   and paste the repo URL, OR use **From device** and upload a zip.
2. Enable the component by attaching it to your active theme.

## Settings

| Setting | Default | Description |
|---|---|---|
| `cta_url` | `https://yodev.dev` | CTA destination for anonymous visitors (and members when `marketplace_url` is empty). |
| `marketplace_url` | _(empty)_ | If set, logged-in members' banner CTA points here instead of `cta_url`. E.g. `https://devmarket.yodev.dev`. |
| `dismissed_field_id` | `0` | Numeric ID of a Discourse User Field for cross-device member dismissal. See setup below. |

## `dismissed_field_id` User Field setup

This is optional but recommended for a good logged-in member experience.

**Step 1 — Create the User Field**

1. Go to **Admin → Customize → User Fields**.
2. Click **Add Field**.
3. Set:
   - **Name:** `beta_offer_dismissed` (or any label — users won't normally see it)
   - **Type:** Text (or Dropdown with one option "1")
   - **Editable:** Yes (required for the component's PUT to work)
   - **Public:** Optional (doesn't affect function)
4. Save. Note the numeric ID Discourse assigns it (visible in the URL or the
   field list).

**Step 2 — Enter the ID in component settings**

In **Customize → Themes → yoDEV Beta Offer → Settings**, set
`dismissed_field_id` to the numeric ID from Step 1 (e.g. `3`).

**Step 3 — Expose the field in `/session/current.json`**

1. Go to **Admin → Settings → search "public user custom fields"**.
2. Add `user_field_3` (replace `3` with your actual field ID) to the list.
3. Save.

Without Step 3, the component cannot read the field on page load and will
fall back to localStorage for dismissal state (dismissal won't persist across
devices for logged-in members, but everything else still works).

## Testing on development.yodev.dev

Install the component on `development.yodev.dev` first. Open the browser
console and filter for `[yodev-beta-offer]` to trace each step (settings read,
auth state, dismissal check, DOM insertion). Verify:

1. Banner appears on all pages (anonymous + logged-in).
2. Dismissing as anon sets `yodev_beta_offer_dismissed` in localStorage.
3. Dismissing as a member (with field configured) fires a PUT and the field
   value appears in `/session/current.json` on next load.
4. `/signup` shows the panel above the form.
5. Dismissed banner does not reappear on reload (until localStorage is cleared
   / field is reset).

## Selector notes (if the banner or panel doesn't appear)

- **Banner container:** The script targets `#main-outlet` (Discourse's standard
  main content wrapper). If your theme renames or removes this element, update
  `BANNER_CONTAINER_SELECTOR` in `common/head_tag.html`.
- **Signup form container:** The script tries `.create-account`,
  `.signup-fullpage`, `.login-fullpage`, `.d-modal.create-account`,
  `.auth-form` in order. If none match on your Discourse version, open
  DevTools on `/signup`, find the outermost form wrapper, and add its selector
  to `SIGNUP_CONTAINER_SELECTOR` in `common/head_tag.html`.

Both poll for up to 10 seconds before giving up with a `console.warn`.
