# Troubleshooting TF Switcher

## TF Switcher does not appear in VS Code

1. Confirm that TF Switcher is installed and enabled in the Extensions view.
2. Confirm that the workspace is trusted. Restricted Mode can prevent extensions
   from activating normally.
3. Run **Developer: Reload Window** from the Command Palette.
4. Select the **TF Switcher** icon in the Activity Bar.

## No available Terraform versions are shown

Loading the release list requires HTTPS access to
`api.releases.hashicorp.com`. On managed networks, ask your administrator to
allow that hostname for the VS Code extension host.

The last successful release list is retained for six hours and across VS Code
restarts. Previously cached Terraform executables remain usable while offline.

## A Terraform download fails

Installing a version requires HTTPS access to `releases.hashicorp.com`. Check:

- Your organization's outbound proxy and TLS inspection requirements.
- Whether VS Code is allowed to reach the HashiCorp endpoints.
- Whether endpoint security software blocked the ZIP or extracted executable.
- Whether sufficient storage space is available in the selected cache folder.

TF Switcher downloads HashiCorp's checksum list, verifies the ZIP's SHA-256
hash, extracts the executable, and runs it locally to confirm the requested
version. A failed verification is not bypassed.

## The terminal still uses the old Terraform version

Version activation applies to **new** VS Code terminals. Close and reopen the
terminal, or use the relaunch action offered by TF Switcher. Terminals outside
VS Code and the machine-wide `PATH` are not changed.

Check the result in a new terminal:

```text
terraform version
```

On Windows, `where terraform` shows the selected executable. On macOS or Linux,
use `which terraform`.

## A cached version is missing

Changing the cache folder does not move versions from the previous folder.
Select the previous cache folder again, reinstall the version into the current
cache, or choose **Use Default Cache**.

The layout is:

```text
<cache-root>/<version>/<os>_<architecture>/terraform(.exe)
```

## The workspace requirement is not detected

Open the folder containing the `.tf` files and confirm that a Terraform block
contains a valid `required_version` expression. In a multi-root workspace, TF
Switcher displays requirements separately for each folder.

## Requesting help

If the problem remains, use the
[bug report form](https://github.com/345dave/tfswitcher-docs/issues/new?template=bug_report.yml).
Include the operating system, processor architecture, VS Code version, TF
Switcher version, reproduction steps, and redacted error text. Never include
credentials, access tokens, internal Terraform configuration, or other secrets.
