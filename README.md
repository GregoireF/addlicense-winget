# addlicense-winget

> WinGet automation — submits [addlicense](https://github.com/GregoireF/addlicense) releases to [microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs).

---

## Installation (once approved by winget-pkgs maintainers)

```powershell
winget install GregoireF.addlicense
```

---

## How it works

When a new version of `addlicense` is released, this repo receives a `repository_dispatch` event and runs `wingetcreate update`. The tool generates the YAML manifests and opens a pull request against `microsoft/winget-pkgs` automatically.

Manifests are NOT stored here — `microsoft/winget-pkgs` is the source of truth.

---

## First submission (one-time manual step)

The automated workflow uses `wingetcreate update`, which works once `GregoireF.addlicense` is already in the [microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) index.
The **initial submission** must be done manually using `wingetcreate new`:

```powershell
wingetcreate new `
  --urls "https://github.com/GregoireF/addlicense/releases/download/v1.0.1/addlicense_windows_amd64.zip,https://github.com/GregoireF/addlicense/releases/download/v1.0.1/addlicense_windows_arm64.zip" `
  --token <WINGET_SUBMIT_TOKEN> `
  --submit
```

After the PR is accepted by the WinGet team (typically 3–5 business days), all subsequent releases are submitted automatically on each `addlicense-released` dispatch.

## Required secrets

| Secret | Description |
|---|---|
| `WINGET_SUBMIT_TOKEN` | GitHub PAT with `public_repo` scope on `microsoft/winget-pkgs`. Rotate before expiry. |

Configure via the IaC workspace or the GitHub repo settings.

---

## Reference

- [wingetcreate](https://github.com/microsoft/winget-create) — official Microsoft submission tool
- [microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) — the package index
- [Submission guidelines](https://github.com/microsoft/winget-pkgs/blob/master/CONTRIBUTING.md)
