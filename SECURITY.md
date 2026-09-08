# Security policy

## Reporting a vulnerability

Please report security issues privately through
[GitHub's private vulnerability reporting](https://github.com/ianswope/calix/security/advisories/new)
rather than opening a public issue. I'll acknowledge the report within a week.

## What Calix holds, and where

Calix stores credentials in the system keyring (Secret Service — GNOME Keyring,
KWallet, and compatible implementations), never in a file:

- Google OAuth refresh tokens, one per connected account.
- iCloud app-specific passwords and generic CalDAV account passwords.

Two things are kept in files instead, both under `~/.config/calix` and
`~/.local/share/calix` with owner-only permissions:

- `config.toml` holds the **Google OAuth client ID and client secret you
  created**. A desktop OAuth client secret is not a confidential credential in
  the OAuth sense — Google issues it to installed apps that cannot keep
  secrets, and the sign-in flow is protected by PKCE — but it identifies your
  Google Cloud project, so treat it as you would an API key.
- The SQLite database holds calendar and event data, including event titles,
  locations, notes and attendee addresses, unencrypted at rest. It is protected
  by file permissions, not by a passphrase.

## Network destinations

Calix talks only to servers you configure: Google's OAuth and Calendar API
endpoints, the CalDAV host for each account you add, and — if location
suggestions are left on — the Photon geocoder for the prefix you type into a
Location field. `[places] enabled = false` in `config.toml` turns that last one
off; `[places] endpoint` points it at a geocoder you host. There is no
telemetry, analytics, or crash reporting of any kind.

## Scope

Calix is a desktop application with no privileged component and no network
listener, other than the short-lived loopback listener that receives the OAuth
redirect during a Google sign-in.
