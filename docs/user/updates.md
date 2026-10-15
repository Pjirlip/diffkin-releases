# Updates

Diffkin updates itself from its public release channel. Every update is signed by the Diffkin release pipeline; a download whose signature does not verify is never installed, whatever the server said.

## How it works

- About twenty seconds after start-up, Diffkin asks the release channel whether a newer version exists (Settings → Updates → *Check for updates when Diffkin starts*, on by default). A found release is announced with a banner; nothing is downloaded or installed without your confirmation.
- **Settings → Updates** shows the new version with its release notes, taken from the changelog of that version. *Download and install* fetches the package with a progress bar, verifies it and installs it. On macOS and Linux you then restart Diffkin with *Restart now*; on Windows the installer restarts Diffkin on its own.
- *Skip this version* hides that release until the next one; *Later* keeps the banner away for the session. Both can be undone on the Updates screen.
- *Check for updates* runs a check at any time.

## Offline use

Without a connection the check fails quietly in the background; the Updates screen explains the error when you check by hand. Everything else keeps working.

## What an update changes

- The application itself. Projects, captures, baselines, comparisons and your settings stay untouched; project files are migrated forward with a backup when a release changes their format (see [Projects](projects.md)).
- Possibly the pinned browser runtime. When a release moves to a new Chrome for Testing version, the release notes say so and Settings → Browser Runtime offers the new download; existing captures keep the version they were taken with.

## Developer builds

Builds that were not produced by the release pipeline (for example `npm run tauri dev`) cannot update in place; the Updates screen says so.
