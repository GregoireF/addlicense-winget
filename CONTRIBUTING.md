# Contributing

Thank you for contributing to `addlicense-winget`.

## What this repository does

This repo contains a single GitHub Actions workflow (`submit.yml`) that listens for
`addlicense-released` dispatch events and submits updated WinGet manifests to
[microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) via
[wingetcreate](https://github.com/microsoft/winget-create).

Manifests are NOT stored here — `microsoft/winget-pkgs` is the source of truth.

## Making changes

1. Fork the repository and create a branch from `main`.
2. Edit `submit.yml` — validate syntax with [actionlint](https://github.com/rhysd/actionlint).
3. Open a Pull Request against `main` — fill in the PR template.

## Required secret

The workflow requires `WINGET_SUBMIT_TOKEN` — a GitHub PAT with `public_repo` scope
on `microsoft/winget-pkgs`. This secret is managed via the IaC workspace.

## Release process

Releases are **fully automated**. When `addlicense` core publishes a new release,
this repository receives a `repository_dispatch` event and runs `submit.yml`
automatically. No manual action is needed.
