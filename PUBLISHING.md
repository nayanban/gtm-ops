# Publishing To Personal GitHub

Target repository name:

```text
gtm-ops
```

Recommended GitHub description:

```text
Sanitized GTM and RevOps automation playbooks for CRM enrichment, meeting-note workflows, prospect data cleanup, and signal-based research.
```

## Create The Repo

Create a new empty public or private repository under your personal GitHub account.

Recommended starting setting: private until the first pushed contents are reviewed on GitHub.

Do not initialize it with a README, license, or gitignore, because this folder already includes those files.

## Push This Folder

From this folder, use a fresh git history:

```bash
git init
git add .
git commit -m "Initial sanitized public playbooks"
git branch -M main
git remote add origin https://github.com/nayanban/gtm-ops.git
git push -u origin main
```

## Final Review

Before switching the GitHub repo to public, review the rendered files in GitHub and confirm:

- no private source history was imported;
- no private organization, customer, prospect, referral, or employee names appear;
- no credentials, local auth paths, or implementation IDs appear;
- examples are synthetic;
- the license and README render correctly.
