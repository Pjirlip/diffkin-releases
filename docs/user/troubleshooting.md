# Troubleshooting

Every error in Diffkin names what happened and, where possible, what to do; the technical details sit behind *Technical details* in the message. This page covers the situations that need more than the message itself.

## Logs

Diffkin writes its log to the standard error stream. To collect it:

- **macOS:** `"/Applications/Diffkin.app/Contents/MacOS/diffkin-desktop" 2> ~/Desktop/diffkin.log`
- **Linux:** `diffkin-desktop 2> ~/diffkin.log` (AppImage: `./Diffkin*.AppImage 2> ~/diffkin.log`)
- **Windows:** start Diffkin from PowerShell with `$env:DIFFKIN_LOG="debug"; & "$env:LOCALAPPDATA\Diffkin\diffkin-desktop.exe" 2> "$env:USERPROFILE\Desktop\diffkin.log"`

`DIFFKIN_LOG` controls the detail (`info` by default, `debug` for reports; the syntax is `RUST_LOG`'s). Credentials, tokens and license keys are masked in the log; URLs and page titles are not, so read the log before sharing it.

`diffkin-desktop --check --output report.json` writes a JSON report of the installation (version, data directory, secure store availability, browser runtime, edition) without opening a window; it is the first thing to attach to a support request.

## Browser runtime

| Symptom | Cause and remedy |
| --- | --- |
| *The download could not be verified* | The archive's checksum did not match the pinned value. The download is discarded; retry. If it fails repeatedly, a proxy or filter rewrites the download; try another network. |
| *No usable sandbox* on Linux | Chromium needs unprivileged user namespaces. `sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0` (persist under `/etc/sysctl.d/`). Only in disposable containers set `DIFFKIN_BROWSER_NO_SANDBOX=1`. |
| Runtime reported as *damaged* | A file is missing or was altered. **Repair** reinstalls it. |
| Captures are slow or hang | Check the per-step timings in the capture metadata of a page: a stage that always hits its timeout (network idle, fonts, images, wait selector) points at the cause. Raise the timeout in the capture profile or choose a specific wait selector. |

## Targets and crawling

| Symptom | Cause and remedy |
| --- | --- |
| *blocked by the network policy* | The target is public and the URL resolves to a loopback, private or link-local address. For local development servers choose the *Local development* policy in the target wizard. See [Network Policy](network-policy.md). |
| *no secret is stored for …* | The project was imported on this machine and the credentials have not been entered here. Open the target, step *Authentication*, and enter them again. See [Authentication](authentication.md). |
| Routes missing from a crawl | Check the crawl's stop reason and counts: the URL or depth limit, include/exclude patterns, scope (subdomains, port) or the sitemap. The *Runs* detail lists skipped routes with their reason. |
| The same page appears twice | Route identity treats `?utm_…` and similar parameters as noise but keeps others; configure the ignored query parameters and the trailing-slash policy of the target. See [Crawling](crawling.md). |
| *The secure credential store is not available* | On Linux a Secret Service (GNOME Keyring, KDE Wallet) must be running and unlocked; on macOS the login keychain must be unlocked. Everything except credentials and licensing keeps working. |

## Comparisons

| Symptom | Cause and remedy |
| --- | --- |
| *dimension mismatch* | The screenshots have different widths, usually a different capture profile or browser version between the sides. Compare captures made with the same profile; the capture-configuration mismatch flag shows when settings differed. |
| Everything is reported as changed | Fonts, anti-aliasing or a global style change. Look at the diff image: scattered single-pixel noise across text points at rendering differences between machines; a shift of the whole page at a layout change at the top. Mask dynamic regions, use frozen time for dates and compare captures from the same platform. See [Visual Diff](visual-diff.md). |
| Severity seems wrong | Severity derives from thresholds in the project settings; adjust them and use **Reclassify** on the comparison, which re-derives severities from the stored evidence without capturing again. |
| A comparison is *interrupted* | Diffkin was closed or crashed while comparing. Start the comparison again; pages already compared are recomputed. |

## Projects and data

| Symptom | Cause and remedy |
| --- | --- |
| *was written by a newer Diffkin* | The project (or `settings.json`) comes from a newer version. Update Diffkin; older versions never modify newer data. |
| Project listed as *unavailable* | Its folder was moved or deleted. Move it back or remove the entry and import the folder from its new location. |
| Upgrade of a project failed | The pre-migration copy is in `.diffkin/backups/<timestamp>/`. See [Projects → Recovering from a failed upgrade](projects.md#recovering-from-a-failed-upgrade). |
| A crawl run is *interrupted* | Diffkin was closed during the crawl. Completed captures are kept and can be compared; start a new crawl for the rest. |
| Settings reset to defaults | `settings.json` in the application data directory could not be read; Diffkin logs the reason and overwrites the file on the next change. |

## Licensing and updates

| Symptom | Cause and remedy |
| --- | --- |
| *license service could not be reached* | Activation and deactivation need the connection; an activated license keeps working for 30 days offline. Try again when online. |
| *activation limit* | Both device slots are in use. Deactivate Diffkin on another device or free the activation in your license portal. See [Licensing](licensing.md). |
| *could not be verified and was not installed* | The downloaded update failed the signature check. Nothing was changed. Retry later; if it persists, download the release from the release page and verify its checksum. See [Updates](updates.md). |
| Updates screen says *not updated in place* | A developer build or a build without a signing key; install releases from the release page. |

## Reporting a problem

Use the issue templates on the release repository: bug report, feature request, diff quality issue or documentation issue. Attach the `--check` report and the relevant log lines, and never credentials, license keys or screenshots with personal data.
