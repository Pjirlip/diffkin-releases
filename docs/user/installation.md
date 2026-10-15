# Installation

Diffkin is distributed as installable packages from its public release page: <https://github.com/Pjirlip/diffkin-releases/releases>. Every release lists the packages below together with `SHA256SUMS.txt` for verification.

| Platform | Package | Notes |
| --- | --- | --- |
| Windows (x64) | `*-setup.exe` (recommended), `*.msi` | The setup installs for the current user without administrator rights. WebView2 is downloaded on first install if missing. |
| macOS (Apple silicon, Intel) | `*.dmg` | Drag Diffkin to Applications. Requires macOS 11 or later. |
| Linux (x86_64) | `*.deb`, `*.AppImage` | The `.deb` installs with `sudo apt install ./diffkin_*.deb`; the AppImage runs after `chmod +x`. |

The browser runtime (Chrome for Testing) is not part of the package. Diffkin downloads the pinned version on first use from Settings → Browser Runtime and verifies its checksum; see [Browser Runtime](browser-runtime.md).

## Verifying a download

Compare the checksum of the downloaded file with the line in `SHA256SUMS.txt` of the same release:

```sh
shasum -a 256 Diffkin_0.1.0_aarch64.dmg      # macOS
sha256sum Diffkin_0.1.0_amd64.deb            # Linux
certutil -hashfile Diffkin_0.1.0_x64-setup.exe SHA256   # Windows
```

## Signing status

Updates are always verified with Diffkin's own signature before they are installed. Operating-system code signing (Apple Developer ID with notarization, Windows Authenticode) is added as soon as the certificates are available; until then macOS shows a Gatekeeper warning (open with right-click → Open the first time) and Windows SmartScreen asks for confirmation. The release notes state when a release is signed.

## Where Diffkin stores data

| Platform | Application data |
| --- | --- |
| macOS | `~/Library/Application Support/dev.fips.Diffkin/` |
| Windows | `%LOCALAPPDATA%\fips\Diffkin\data\` |
| Linux | `~/.local/share/diffkin/` |

The directory holds the project registry, the browser runtime, logs and `settings.json`. Projects live where you created them. Credentials and the license are in the operating system's secure store, never in these files.

## Uninstalling

Use the platform's uninstaller (Windows), drag the app to the Bin (macOS) or `sudo apt remove diffkin` / delete the AppImage (Linux). The Linux package installs the binary as `/usr/bin/diffkin-desktop`; the Windows setup installs to `%LOCALAPPDATA%\Diffkin`. Project folders and the application data directory are left in place so nothing is lost by accident; delete them yourself if you no longer need them.

## Checking an installation

The application binary supports two flags for support cases and automated smoke tests: `--version` prints the version and build identifier, `--check --output report.json` starts the application services without a window and writes a JSON report (data directory, secure store availability, browser runtime version, edition). On Windows use the `--output` file, because the application has no console window.
