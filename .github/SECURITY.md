# Security Policy

## Supported Versions

Only the latest revision on `main` is maintained.

## Reporting a Vulnerability

**Do not open a public GitHub issue for security vulnerabilities.**

Use one of:

1. **[GitHub private security advisory](https://github.com/GregoireF/addlicense-winget/security/advisories/new)** _(preferred)_
2. **Email** — [gfavreau.wrprojects@gmail.com](mailto:gfavreau.wrprojects@gmail.com) with subject `[SECURITY] addlicense-winget`.

**Response SLA:** acknowledgement within 48 h · triage within 7 days.

## Scope

This repository only contains a GitHub Actions workflow that calls `wingetcreate`. The actual WinGet manifests live in [microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs). Security issues in the manifests or the `addlicense` binary should be reported to the respective repositories.

The `WINGET_SUBMIT_TOKEN` secret must have `public_repo` scope on `microsoft/winget-pkgs` only. It must never have write access to this repository or any private repository.
