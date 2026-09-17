# Security and verification

This page explains what TF Switcher accesses, the safeguards built into it,
and what the project's automated checks can and cannot prove.

## The short version

TF Switcher downloads official Terraform packages, verifies their SHA-256
checksums, and keeps each version in a local cache. It does not collect
telemetry, request cloud credentials, or change the machine-wide `PATH`.

Every proposed source change is tested on Windows, macOS, and Linux. An
additional security workflow scans the repository's history for accidentally
committed secrets, audits production dependencies, builds the Marketplace
package, checks its file list for common secret-bearing files, and records the
package's SHA-256 checksum.

These controls provide useful evidence and catch common mistakes. They are not
a promise that any software is risk-free.

## What the extension does

TF Switcher can:

- Read Terraform version constraints from the open VS Code workspace.
- Connect by HTTPS to `api.releases.hashicorp.com` for the version list.
- Connect by HTTPS to `releases.hashicorp.com` for Terraform ZIP files and
  checksum files.
- Import an official Terraform ZIP and matching checksum file selected by the
  user for an offline installation.
- Extract only the expected Terraform executable into TF Switcher's cache.
- Run `terraform version` to verify that an installed executable starts and
  reports the expected version.
- Prepend the selected cache folder to the environment of new VS Code
  terminals for the current workspace.

TF Switcher does not:

- Collect telemetry or analytics.
- Ask for, store, or transmit Terraform or cloud credentials.
- Read browser data, cryptocurrency wallets, or the clipboard.
- Mine cryptocurrency or perform background computing.
- Upload workspace files.
- Modify the machine-wide `PATH`; existing applications and terminals are not
  changed.

Like other VS Code extensions, TF Switcher runs with the permissions of VS
Code. Install extensions only from a source you trust and keep them updated.

## Download and ZIP safeguards

- Downloaded and imported Terraform ZIP files must match HashiCorp's official
  filename format for the selected version, operating system, and processor
  architecture.
- The ZIP's SHA-256 hash must match the selected checksum file before
  extraction begins.
- Archive entries are read one at a time. Unexpected paths or filenames,
  excessive entry counts, and oversized executables are rejected.
- The extracted executable is checked by running `terraform version` before it
  is accepted into the cache.
- An imported version is not activated automatically, so the user remains in
  control of the workspace selection.

For offline import, obtain both files directly from HashiCorp or a trusted IT
software repository. A checksum detects a changed ZIP, but it cannot establish
trust if an attacker supplied both the ZIP and a fraudulent checksum file.

## Automated checks

The private implementation repository runs:

| Check | What it helps detect |
| --- | --- |
| Windows, macOS, and Linux tests | Platform-specific regressions |
| Official Windows Terraform integration test | Whether a genuine `terraform.exe` can be imported and executed |
| Gitleaks history scan | Common credential and token patterns accidentally committed to Git |
| `npm audit` of production packages | Known published vulnerabilities in runtime dependencies |
| VSIX content inspection | Common secret-bearing filenames accidentally included in the package |
| VSIX SHA-256 generation | A fingerprint for identifying an exact built package |

Workflow actions are pinned to exact Git commit identifiers so a moving action
tag cannot silently change the code used by the security workflow.

The source repository is currently private. That limits independent public
source review, and GitHub's hosted CodeQL, dependency-review, and private-repo
secret-scanning products require a suitable paid GitHub security plan. The
project therefore uses repository-owned checks that work with the current
account. This page will be updated if that position changes.

## Marketplace checks

Microsoft states that Visual Studio Marketplace extensions are scanned for
malware, secrets, and other security concerns, and that published extension
packages are signed by the Marketplace. See Microsoft's
[Visual Studio Marketplace security documentation](https://code.visualstudio.com/docs/configure/extensions/extension-runtime-security).

Marketplace scanning is another layer of protection; it is not an independent
guarantee that an extension is free from every possible defect or malicious
behavior.

## Verify a downloaded package

The security workflow records a SHA-256 fingerprint for every VSIX it builds.
A release fingerprint is meaningful only when it is published next to the
specific VSIX to which it applies. Do not compare a package against a checksum
from a different version or build.

A QR code can make this page easier to reach, but the QR code itself does not
validate software. The HTTPS address, version number, source of the package,
and matching checksum are what matter.

## Report a concern

Do not publish credentials, access tokens, private Terraform configuration,
exploit details, or other sensitive information in a public issue. Follow the
[security reporting policy](SECURITY.md) to request a private reporting channel.
