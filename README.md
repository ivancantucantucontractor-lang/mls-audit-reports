# MLS Audit Reports

Published launch-readiness reports from the [MLS Audit app](https://github.com/ivancantucantucontractor-lang/mls-audit-app) (private).

**Dashboard:** [ivancantucantucontractor-lang.github.io/mls-audit-reports/](https://ivancantucantucontractor-lang.github.io/mls-audit-reports/)

## How it works

This is a data-only repo — no source code. The app is run locally by each user against their own MLS feed credentials. When a user hits the app's **Publish** button, the app:

1. Renders the current audit as a self-contained static HTML file.
2. Writes it to `docs/reports/<slug>/<timestamp>/`.
3. Appends an entry to `docs/manifest.json`.
4. Commits and pushes to this repo.

The dashboard (`docs/index.html`) fetches `docs/manifest.json` on load and renders every historical run, filterable by MLS, verdict, owner, and time range.

## Layout

```
docs/
├─ index.html          ← dashboard (JS reads manifest.json)
├─ manifest.json       ← array of every published run
├─ styles.css          ← copied from the app repo
├─ .nojekyll
└─ reports/
   └─ <slug>/
      └─ <YYYY-MM-DDTHH-MM-SSZ>/
         ├─ index.html
         ├─ galaxy.csv
         └─ listhub.csv
```

## Redaction posture

The Publish flow strips query strings from feed URLs before writing HTML — so if a user pasted a token into a URL by accident, it won't land here. Beyond that, published reports contain field-level coverage against RESO / ListHub checklists, aggregate counts, and MLS-supplied field names. **No credentials, tokens, or user PII should ever reach this repo.**

If you spot something that looks like a secret in a published report: open an issue, and we'll purge the offending files and re-audit the publish pipeline.

## Adding a report

You don't add reports here directly. Get the private MLS Audit app installed on your machine (installation instructions live in that repo's README), run an audit against your feed, hit Publish. The published report appears here within seconds.

## Versioning

Every publish creates a new timestamped directory — nothing is overwritten. This means the dashboard shows every historical run per MLS. Over time this grows; when a purge is needed, we'll add a retention policy at that point.
