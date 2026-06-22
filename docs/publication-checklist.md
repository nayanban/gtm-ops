# Publication Checklist

Use this before pushing a public version.

## Private Source Repos

- Private source repositories remain private.
- No private git history is imported.
- Public repo starts with a fresh first commit.

## Content Review

Confirm the public copy does not include:

- private company or client names;
- customer, prospect, referral, or employee names;
- personal emails, phone numbers, or profile URLs;
- CRM record IDs, workflow IDs, account IDs, user IDs, or module-specific private metadata;
- OAuth, API, refresh-token, Keychain, local secret, or credential-storage details;
- raw meeting notes, transcripts, call payloads, exports, snapshots, or state files;
- private account strategy, target-account lists, court lists, campaign names, or segmentation findings;
- local machine paths from private workspaces;
- old private repository remotes.

## Repository Setup

- Add `README.md`.
- Add `LICENSE`.
- Add `.gitignore`.
- Add only sanitized docs and examples.
- Keep examples synthetic.
- Run a local scan before publishing.

## Suggested Scan

Search for risky terms before the first public commit:

```bash
rg -n -i "token|secret|oauth|refresh|password|api[_ -]?key|client_secret|private|transcript|export|snapshot|record id|workflow id|user id|account id|email|phone|profile url|private_org_name|private_client_name|private_tool_name" .
```

Some vendor names may be fine in a public repo, but any hit should be reviewed deliberately.

## Publish

1. Create a new empty personal GitHub repo.
2. Initialize git locally in this sanitized folder.
3. Commit the sanitized files.
4. Push to the new personal repo.
5. Make the repo public only after the pushed contents are reviewed on GitHub.
