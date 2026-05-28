# librechat-mac-app-updates

Publishes the latest available version of the **LibreChat Mac app**, so the app itself can poll for updates without requiring authentication.

The actual app source lives at A8C GHE instance. This public repo only holds a small manifest — a pointer to the latest release. The binaries themselves still live behind the VPN-gated GHE instance.

## How it works

On launch and every 6 hours after, the app fetches `latest.json` from this repo. If the version listed here is newer than the running app's bundle version, an "Update available" banner appears in the app window, with a button to open the release page in the user's default browser.

`latest.json` is the only file the app reads. Everything else here is documentation.

## Manifest schema

```jsonc
{
  "version": "0.4.6",                                                                    // bare semver, no leading "v"
  "releaseURL": "https://github.a8c.com/Automattic/librechat-mac-app/releases/tag/v0.4.6", // where to send the user
  "publishedAt": "2026-05-27T15:47:24Z",                                                 // ISO 8601 UTC, optional
  "notes": "Optional one-line summary shown in the update banner."                       // optional
}
```

- `version` is the only required field. The app does a numeric semver compare against `Bundle.main.shortVersion`; if newer, the banner fires.
- `releaseURL` defaults to the github.a8c.com releases listing if absent.
- `notes` and `publishedAt` are optional UI hints.

## Updating the manifest

Eventually this will be automated by the Mac app's release script. For now, after cutting a new release:

1. Cut and publish the release on github.a8c.com.
2. Edit `latest.json` here, bump `version` / `releaseURL` / `publishedAt`.
3. Commit and push to `trunk`. Within ~6 hours every installed copy will see the banner.

raw.githubusercontent.com is CDN-cached for a few minutes; if you want immediate propagation for testing, append a query string in the app's fetch URL (or wait).
