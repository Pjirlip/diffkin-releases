# Security and privacy

Diffkin is local-first: crawling, screenshots, DOM captures, comparisons and baselines happen on your machine and stay there. This page lists what leaves your machine, where sensitive data lives and how crawled websites are kept at arm's length. The developer-facing details are in the repository's security notes.

## What leaves your machine

| Connection | When | What is sent |
| --- | --- | --- |
| The websites you crawl | During connection tests, crawls and previews | Requests from the separate browser process, with the credentials you configured for that target only. |
| Chrome for Testing download (Google) | When you install or repair the browser runtime | A plain download; the archive is verified against a pinned checksum. |
| License service (Lemon Squeezy) | Activation, validation (about once a day), deactivation of Diffkin Plus | License key, device identifier and instance name. Nothing about your projects. |
| Release channel (GitHub) | Update checks and downloads | The current version, as part of the request for the update manifest. |

There is no telemetry, no account and no cloud storage. Free works entirely without the last two connections.

## Where sensitive data lives

- Basic Auth passwords, secret header values, the license key and the installation identifier are stored in the operating system's secure store (macOS Keychain, Windows Credential Manager, Linux Secret Service). Project files contain references only, so a project folder can be copied or shared without leaking credentials.
- Project folders hold configuration, captures, screenshots and comparisons; screenshots and DOM snapshots naturally contain whatever the pages showed, including personal data on authenticated pages. Treat project folders accordingly.
- Logs and exports mask credentials, tokens and keys; URLs and page titles remain.

## Crawled websites are untrusted

- Pages load in a separate, sandboxed Chrome for Testing process with a fresh temporary profile per crawl, never in Diffkin's own window. The browser has no access to your keychain, extensions or other sites.
- Public targets cannot reach your local network: requests to loopback, private, link-local and similar addresses are blocked, including through redirects, subresources and sitemaps. Local targets require the explicit *Local development* policy. See [Network Policy](network-policy.md).
- Credentials are sent to the target's own origin only. Downloads are denied; `file://` and non-web schemes are never followed.

## Updates and releases

Releases are built by an automated pipeline and signed; the application verifies every update before installing it and refuses anything that fails verification. See [Updates](updates.md) and [Installation](installation.md) for checksums and the current code-signing status.

## Reporting a vulnerability

Please report security issues privately through the security policy of the release repository rather than in a public issue. Include the Diffkin version and the steps to reproduce; do not include credentials.
