# addlicense-winget

> WinGet automation — submits [addlicense](https://github.com/GregoireF/addlicense) releases to [microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs).

[![Submit workflow](https://github.com/GregoireF/addlicense-winget/actions/workflows/submit.yml/badge.svg)](https://github.com/GregoireF/addlicense-winget/actions/workflows/submit.yml)
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/GregoireF/addlicense-winget/badge)](https://securityscorecards.dev/viewer/?uri=github.com/GregoireF/addlicense-winget)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

---

## Installation

Once approved by the WinGet maintainers:

```powershell
winget install GregoireF.addlicense
```

> **Note:** The initial submission is a one-time manual step (see below). After acceptance, all subsequent releases are submitted automatically.

---

## How it works

```
addlicense release (GoReleaser)
        │
        │  repository_dispatch: addlicense-released
        ▼
  submit.yml
        │
        ├─ Download wingetcreate from GitHub releases
        ├─ Build installer URLs (amd64 + arm64 .zip)
        └─ wingetcreate update GregoireF.addlicense
                │
                └─▶ Pull Request on microsoft/winget-pkgs
```

When a new version of `addlicense` is released, this repo receives a `repository_dispatch` event and runs `wingetcreate update`. The tool generates the YAML manifests and opens a pull request against `microsoft/winget-pkgs` automatically.

Manifests are **not** stored here — `microsoft/winget-pkgs` is the source of truth.

---

## First submission (one-time manual step)

`wingetcreate update` requires the package to already exist in the WinGet index. The **initial submission** must be done manually using `wingetcreate new`:

```powershell
# Install wingetcreate
Invoke-WebRequest `
  -Uri "https://github.com/microsoft/winget-create/releases/latest/download/wingetcreate.exe" `
  -OutFile "$env:TEMP\wingetcreate.exe"

# Submit the initial manifest
& "$env:TEMP\wingetcreate.exe" new `
  --urls "https://github.com/GregoireF/addlicense/releases/download/v1.0.1/addlicense_windows_amd64.zip,https://github.com/GregoireF/addlicense/releases/download/v1.0.1/addlicense_windows_arm64.zip" `
  --token <WINGET_SUBMIT_TOKEN> `
  --submit
```

After the WinGet team accepts the PR (typically 3–5 business days), all subsequent releases are automated.

---

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| `Failed to connect to GitHub` | `WINGET_SUBMIT_TOKEN` expired or wrong scope | Regenerate PAT with `public_repo` scope, update secret |
| `Package not found` on `update` | First submission not done yet | Run `wingetcreate new` (see above) |
| `wingetcreate.exe` not found | Download step failed | Check runner internet access and GitHub releases availability |
| PR not opened | Token lacks access to `microsoft/winget-pkgs` | PAT must have `public_repo` scope on the target repo |

---

## Required secrets

| Secret | Scope | Description |
|---|---|---|
| `WINGET_SUBMIT_TOKEN` | `public_repo` on `microsoft/winget-pkgs` | GitHub PAT used by `wingetcreate` to open PRs. Rotate before expiry. |

Configure via the IaC workspace (`GregoireF/iac`) or the GitHub repo settings.

---

## Ecosystem

| Package | Description |
|---|---|
| [addlicense](https://github.com/GregoireF/addlicense) | Core CLI — the Go binary |
| [addlicense-action](https://github.com/GregoireF/addlicense-action) | GitHub Action |
| [addlicense-npm](https://github.com/GregoireF/addlicense-npm) | npm package — `@gregoiref/addlicense` |
| **addlicense-winget** | This repo — WinGet submission |
| [homebrew-tap](https://github.com/GregoireF/homebrew-tap) | Homebrew — `brew install GregoireF/tap/addlicense` |

---

## Reference

- [wingetcreate](https://github.com/microsoft/winget-create) — official Microsoft submission tool
- [microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) — the package index
- [Submission guidelines](https://github.com/microsoft/winget-pkgs/blob/master/CONTRIBUTING.md)

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) and [SECURITY.md](.github/SECURITY.md).

---

## License

MIT — see [LICENSE](LICENSE).
