# TF Switcher

TF Switcher is a Visual Studio Code extension for installing and switching
workspace-specific Terraform versions without a command-line version manager.
It is designed for managed enterprise devices, teams working across projects,
and users who prefer a visual workflow.

[Install TF Switcher from the Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=345Dave.tfswitcher)

## See it in action

TF Switcher reads the workspace requirement, offers a compatible Terraform
version, activates it for new VS Code terminals, and verifies the result.

![TF Switcher detects, activates, and verifies Terraform on macOS](images/tfswitcher-demo.gif)

## Highlights

- Installs official Terraform releases directly from HashiCorp.
- Does not require access to GitHub when using the extension.
- Detects `required_version` constraints in Terraform workspaces.
- Verifies every downloaded ZIP against HashiCorp's SHA-256 checksum.
- Changes `PATH` only for new or relaunched VS Code terminals.
- Reuses previously downloaded versions from a local cache.
- Does not collect telemetry or request Terraform credentials.

## Supported platforms

| Platform | Architectures |
| --- | --- |
| Windows | x64, ARM64, x86 |
| macOS | Apple Silicon, Intel |
| Linux | x64, ARM64, x86 |

## Network requirements

Installing a Terraform version requires HTTPS access to
`releases.hashicorp.com`. Loading the available-version list requires access to
`api.releases.hashicorp.com`. Previously cached Terraform versions remain
available offline.

## Support and feedback

[Open an issue](https://github.com/345dave/tfswitcher-docs/issues) to report a
defect or request a feature. Do not include Terraform credentials, access
tokens, internal configuration, or other secrets in reports.

This repository contains public documentation and media for TF Switcher. The
extension's implementation repository is maintained separately.
