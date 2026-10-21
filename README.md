# SKY TV Player — Beta Updates

Public home for SKY TV Player beta APK downloads, release notes, and update information. App source code, playlists, provider accounts, and viewing history do not belong here.

## Current status

**Prepared for releases — no public beta APK or live update manifest has been published yet.**

The app's online update configuration remains disabled until an approved release and its hosted update path have been verified. A template is not an available update.

## Downloads

[Open beta releases and release notes](https://github.com/sky2135/skytv-updates/releases)

When a beta is published, download the versioned `.apk` listed under that release's Assets. GitHub's automatically generated “Source code” ZIP/TAR files are not Android installers. Read the release's known issues before installing.

## Repository structure

- [Release process](RELEASE_PROCESS.md) — publication, integrity, signing, and TV update checks.
- [Manifest example](beta/latest.example.json) — nonoperational template matching the app's strict update format.
- `beta/latest.json` — reserved for the first approved release; deliberately absent for now.

The planned fixed manifest address is:

```text
https://raw.githubusercontent.com/sky2135/skytv-updates/main/beta/latest.json
```

Do not configure an app to use that address until the real file and download have been verified. APKs belong in versioned GitHub Releases, not ordinary source commits. No GitHub login or personal API key will be embedded in the app for public downloads.

## Release policy

Test the app, approve one exact versioned APK, verify its identity and download, then publish the manifest last. The updater checks the APK's size, SHA-256, package, version, Android requirement, and matching installed signing certificate. Download and installation remain separate viewer actions, with Android's installation confirmation.

This repository is public, not a private customer portal. Never upload passwords, usernames, server/stream addresses, playlists, personal API tokens, signing keys, private app data, or signed-in QA screenshots. Only approved distribution artifacts and sanitized release notes belong here.

GitHub reference: [About releases](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases).
