# Diffkin

**Your visual testing buddy.** Diffkin crawls your staging and production websites, captures deterministic screenshots and normalized DOM states, and shows you exactly what changed, visually and structurally. Local. Private. No tests to write.

This repository hosts the releases, the documentation and the issue tracker. The source code is maintained privately.

## Download

Packages for every release are on the [releases page](https://github.com/Pjirlip/diffkin-releases/releases):

| Platform | Package |
| --- | --- |
| Windows (x64) | `*-setup.exe` (recommended) or `*.msi` |
| macOS (Apple silicon, Intel) | `*.dmg` |
| Linux (x86_64) | `*.deb` or `*.AppImage` |

Every release lists `SHA256SUMS.txt`; see [Installation](docs/user/installation.md) for verification, data locations and the current code-signing status. Diffkin updates itself from this repository once installed; see [Updates](docs/user/updates.md).

## Documentation

Start with [Installation](docs/user/installation.md) and the [desktop walkthrough](docs/user/desktop.md). The full index is in [docs/user](docs/user/README.md): projects, targets, crawling, capture profiles, comparisons and baselines, visual and DOM diff, authentication, network policy, licensing, updates, security and privacy, troubleshooting.

## Editions

Diffkin Free contains the complete local workflow. Diffkin Plus adds automation and professional workflows (CLI and CI mode, scheduler, MCP server, multiple and custom capture profiles, remote storage, integrations) as they ship. See [Licensing](docs/user/licensing.md).

## Feedback

Use the issue templates for bug reports, feature requests, diff quality reports (false positives, missed changes, wrong severity) and documentation issues. Please never include credentials, license keys or screenshots with personal data. Security issues go through the [security policy](SECURITY.md).

## License

Diffkin is proprietary software; see [LICENSE.md](LICENSE.md). Third-party notices are shipped with every release and shown under Settings → About.
