# Authentication

Many staging sites sit behind HTTP Basic Auth or expect a special header. Diffkin stores such credentials in your operating system's secure store, never in the project folder.

## Where secrets live

| Platform | Store |
| --- | --- |
| macOS | Keychain |
| Windows | Credential Manager |
| Linux | Secret Service (GNOME Keyring, KWallet via D-Bus) |

The project configuration only keeps a **reference** such as `secret://target/<target-id>/basic-auth/password`. A reference carries no secret material, so project folders can be copied, shared or committed without leaking credentials. The About screen shows which store is active.

## Basic Auth

Configure a username and password on the target. The username is stored in `project.json`; the password goes into the secure store under the target's reference.

## Custom headers

A header value is either **plain** (stored in `project.json`, for example `X-Preview: 1`) or **secret** (stored in the secure store, for example `X-Api-Key`). Diffkin refuses plain values for credential headers such as `Authorization`, `Proxy-Authorization`, `Cookie`, `X-Api-Key`, `X-Auth-Token` and `X-Access-Token`; store those as secrets.

Headers that the browser controls (`Host`, `Content-Length`, `Connection`, `Transfer-Encoding`, …) cannot be overridden.

## After importing a project on another machine

The references come along, the secret values do not. The target shows which credentials are **missing**, and a crawl of that target fails with an authentication error that names them. Enter the credentials again on the new machine; the references stay the same.

## Removing credentials

Clearing Basic Auth, removing a secret header or deleting a target deletes the corresponding entries from the secure store. Removing a project from Diffkin or deleting its data does **not** touch the secure store today; use the target settings to clear credentials first if you want them gone.

## Logs and reports

Diffkin never writes credential values to logs, error messages, reports or exports. Authorization headers, cookies, passwords, tokens and URL credentials are replaced by `[REDACTED]` before anything leaves the application.
