# Publishing a beta update

This process prepares a public, sideloaded Android TV beta. It does not authorize publication of an unreviewed APK. The first release needs owner approval of the exact artifact and a completed hosted-update acceptance check.

## 1. Approve and identify the APK

- Complete the required compile, local unit, debug APK, and Android-test APK gates. Execute the affected emulator tests and inspect actual 1920×1080 UI. Keep outstanding playback, safety, startup, and update checks explicit.
- Confirm that no provider credentials, server configuration, personal TMDB token, private QA images, or signing key are packaged for public distribution. Never publish a personal-token experiment build.
- Record the APK's actual package name, version code/name, minimum SDK, exact byte count, SHA-256, and public signer fingerprint. Do not copy example values.
- Use a version code greater than every installation targeted by this update. Preserve the installed signing certificate; the current updater deliberately rejects signing-key rotation. A switch from a debug key to a different release key is not an in-place compatible update.

## 2. Publish the approved download

Create a versioned GitHub Release marked as a pre-release. Use a unique tag and APK asset name, attach the exact approved APK plus its SHA-256 checksum, and include concise changes and known issues. GitHub's automatic source ZIP/TAR files are not APKs.

Publish the approved asset before advertising it in the update manifest. Use that release's version-specific download link, not a moving “latest” asset link. Never replace different APK bytes at an existing release URL.

Verify the real download and its complete HTTPS redirect chain. The app permits at most three APK redirects, and every initial/redirect host must be explicitly allowlisted. Derive those hosts from the actual approved asset; do not guess CDN names, use wildcards, or commit temporary signed redirect URLs. Recheck the downloaded bytes, hash, package, version, minimum SDK, and signing certificate.

## 3. Publish the manifest last

Copy `beta/latest.example.json` to `beta/latest.json` only after the real release is approved and available. Replace every example identity field with values from that exact APK.

The strict UTF-8 object accepts only:

| Field | Type / requirement |
|---|---|
| `schema` | Integer `1` |
| `packageName` | Exact installed application package |
| `versionCode` | Positive integer; greater than the targeted installed version |
| `versionName` | Actual APK version name, 1–64 characters |
| `minSdk` | Actual APK minimum SDK, integer |
| `apkUrl` | Verified fixed HTTPS release-asset URL |
| `apkBytes` | Exact integer byte count, at most 200 MiB |
| `sha256` | Exact 64-character hexadecimal APK digest |
| `notes` | Optional release notes, at most 2,000 characters |

Keep the complete manifest at most 32 KiB. Unknown/duplicate fields, wrong types, or trailing JSON are rejected. The fixed raw manifest endpoint must return HTTP 200 directly, without redirects, credentials, or query parameters. Do not use a GitHub `blob` HTML page as its URL.

There is no inactive-release marker in the current app schema. `{}`, `null`, `available:false`, HTTP 204, and HTTP 404 are errors, not “up to date.” Until the first real approved release exists, leave `beta/latest.json` absent and the app's update configuration disabled. Afterwards, a genuine full release descriptor with a version code at or below the installed version is the supported no-update response.

## 4. Wire and accept the hosted update

Configure the fixed manifest address and reviewed exact APK/redirect hosts in a later reviewed app build. Do not alter a candidate whose source is frozen for QA. Older installations without this configuration need one manually supplied compatible APK first.

On the assigned test emulator, verify the real lower-to-higher-version flow: actual TV UI, download cancel/retry, invalid hash/identity/old-version rejection, explicit download/install choices, Android installer cancellation and confirmation, and preserved settings/history. Returning from install-source permission settings must not silently install. API 23 uses the manual update path. These are acceptance requirements, not claims that hosting alone has tested the updater.

## 5. Maintain the channel

Publish only approved higher-version fixes. If an announcement must be withdrawn, revert the manifest to the previous genuine approved release rather than inventing an empty descriptor. Already upgraded devices will not downgrade; repair those with a higher version code. Keep prior release bytes and sanitized release notes traceable.

Do not put account details, provider addresses, API keys, private screenshots, or viewing data in issues or release notes. Public beta distribution is not private access control.
