# Licensing

Diffkin comes in two editions. **Diffkin Free** contains the complete local workflow: targets, crawling, deterministic captures, visual and DOM diffs, comparisons, review and baselines. **Diffkin Plus** adds automation and professional workflows: the command-line interface and CI mode, scheduled crawls, the MCP server, custom and multiple capture profiles, advanced crawl rules, proxies, remote storage, webhooks, GitHub and GitLab integrations, trends, report export and optional analysis features. Features that are not available yet are listed under Settings → License as they ship.

Free never produces less trustworthy screenshots or diffs than Plus.

## What a purchase covers

- One named purchaser (the email address of the purchase).
- Up to two personal desktop devices at the same time.
- The purchased major version, for example every 1.x release. A later major version may be a paid upgrade; the versions you bought stay usable with their own releases. Pre-release 0.x builds belong to the 1.x entitlement.

## Activate

Open **Settings → License**, enter the email address of the purchase and the license key, and click **Activate**. Diffkin validates the key online, registers this device and stores the key in your operating system’s secure credential store (Keychain, Credential Manager, Secret Service). Nothing license-related is written to project files, so projects stay portable.

If both device slots are in use, deactivate Diffkin on one of the other devices (Settings → License → Deactivate this device) or free the activation in your license portal, then try again.

## Validation and offline use

Diffkin validates the license again about once a day while online. When the license service cannot be reached, Plus stays active for 30 days after the last successful validation; the license screen shows when that period ends. After it, Plus features pause until a validation succeeds; your projects and settings stay as they are. **Validate now** runs a check immediately.

## When a license becomes invalid

If the key is disabled, expired or revoked, or the device activation was removed in the license portal, Diffkin switches to Free and explains why on the license screen. Nothing is deleted: projects open, captures and comparisons remain readable, and Plus configuration (for example a target with several capture profiles) is preserved. Starting a crawl that needs Plus is refused with the explanation until the license is valid again or the configuration is reduced to Free limits. You can validate again after the issue is resolved, activate another key, or remove the license from the device.

## Deactivate

**Settings → License → Deactivate this device** frees the device slot for another computer and removes the license from this device. Your data stays untouched. If the license service cannot be reached, you can **forget the license on this device**; the slot then remains in use until you free it in the license portal or deactivate once you are online again.

## CI and automation

Build agents do not use desktop activations. The CLI (Diffkin Plus) reads `DIFFKIN_LICENSE_EMAIL` and `DIFFKIN_LICENSE_KEY` from the environment, validates them for the running process only and stores nothing. Both variables must be set together. Keys are masked in logs and reports.

## Builds without a storefront

Developer builds are made without storefront configuration. They run as Diffkin Free, and the license screen explains that activation is unavailable in that build.
