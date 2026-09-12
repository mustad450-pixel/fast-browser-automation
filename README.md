# Fast Browser Automation

Private automation repository for Fast Browser release security.

## Current purpose

- Keep the heavy Chromium build on the existing Windows build machine.
- Use a GitHub Actions self-hosted Windows runner only for release automation.
- Sign the final installer with Sigstore keyless signing using GitHub Actions OIDC.
- Verify the exact GitHub workflow identity after signing.
- Generate a SHA-256 checksum beside the installer.

## Workflow

`.github/workflows/sign-fast-browser.yml`

The workflow expects the existing installer by default at:

`C:\src\browser-project\Fast-Browser-v1.2-Final\Fast-Browser-v1.2-Final-Setup.exe`

It creates:

- `Fast-Browser-v1.2-Final-Setup.exe.github-actions.sigstore.json`
- `Fast-Browser-v1.2-Final-Setup.exe.sha256.txt`

## Required runner

A Windows x64 self-hosted runner registered to this repository with the custom label:

`fast-browser`

The machine must already have Sigstore Cosign v3.1.3 available on PATH.

## Security boundaries

- No Chromium source is stored in this repository.
- No long-lived signing key is stored in GitHub.
- The workflow requests only `contents: read` and `id-token: write`.
- Signing uses GitHub Actions workload identity.
- The workflow verifies the exact workflow identity and GitHub Actions OIDC issuer.
- The workflow refuses to sign if the expected Cosign version is not present.
- Signing is manual via `workflow_dispatch` until the local AutoSecurity integration is deliberately enabled later.
