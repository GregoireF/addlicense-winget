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

## Required secrets

| Secret | Description |
|---|---|
| `WINGET_SUBMIT_TOKEN` | GitHub PAT with `public_repo` scope on `microsoft/winget-pkgs` |

Configure via the IaC workspace or the GitHub repo settings.

---

## Reference

- [wingetcreate](https://github.com/microsoft/winget-create) — official Microsoft submission tool
- [microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) — the package index
- [Submission guidelines](https://github.com/microsoft/winget-pkgs/blob/master/CONTRIBUTING.md)
