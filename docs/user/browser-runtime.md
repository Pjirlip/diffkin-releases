# Browser runtime

Diffkin captures pages with its own, separately installed browser: **Chrome for Testing**, a build of Chromium that never auto-updates. Every Diffkin release pins one exact version so screenshots stay comparable over time. The application window itself is never used to load websites.

## Installation

Open **Settings → Browser Runtime**. The panel shows whether the pinned version is installed. **Install** downloads the archive for your platform (roughly 200 MB) from Google's Chrome for Testing storage, verifies its SHA-256 checksum against the value recorded in Diffkin and unpacks it into Diffkin's application data folder:

| Platform | Location |
| --- | --- |
| macOS | `~/Library/Application Support/dev.fips.Diffkin/browsers/` |
| Windows | `%LOCALAPPDATA%\fips\Diffkin\data\browsers\` |
| Linux | `~/.local/share/diffkin/browsers/` |

A download whose checksum does not match is discarded and nothing is installed. The installation can be cancelled at any time.

## Repair

If the panel reports the runtime as damaged (for example after an interrupted installation or a deleted file), **Repair** removes the installation and downloads it again.

## Isolation

Each crawl runs in a separate browser process with a fresh, temporary profile that is deleted when the process ends. Profiles left behind by a crash are removed on the next start. The browser does not get access to your system keychain, extensions, sync or any Diffkin credentials beyond the ones configured for the target.

Chromium's own sandbox stays enabled. On Linux it needs unprivileged user namespaces. Ubuntu 23.10 and later restrict them through AppArmor, which makes the browser fail to start with "No usable sandbox"; allow them with `sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0` (persist it under `/etc/sysctl.d/`). Only in disposable containers without that option set `DIFFKIN_BROWSER_NO_SANDBOX=1` to disable the sandbox; never do this on a workstation.

## Version changes

A new runtime version can change how pages render, so Diffkin only updates it with an application release, and the release notes say so. Every crawl run records the browser version it was captured with.
