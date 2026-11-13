# Cloudstream Revanced build and release

This repository imports Cloudstream from `recloudstream/cloudstream` at commit
`5d66bbeaf3b35c0c18ad2e0b9ce6b0aea4f5241c`. The settings design is inspired by
Next Player; its source code was not copied. Cloudstream's license and upstream
attribution are retained.

Pushes to the Arena working branch build three debug APKs as a workflow artifact.
For a signed release, the workflow must first exist on the repository's default
branch (GitHub's requirement for `workflow_dispatch`). Set these repository
Actions secrets, then run **Revanced APK build and release** with `publish` checked
and a new tag:

- `REVANCED_KEYSTORE_BASE64`: base64-encoded persistent Android signing keystore
- `REVANCED_KEY_ALIAS`, `REVANCED_KEY_PASSWORD`, `REVANCED_STORE_PASSWORD`
- Optional upstream API keys: `SIMKL_CLIENT_ID`, `SIMKL_CLIENT_SECRET`, `MAL_KEY`, `ANILIST_KEY`

The stable release application ID is `com.cloudstream.revanced`, so it installs
separately from upstream Cloudstream and does not automatically inherit its data.
Use the same keystore for every Revanced release; a different signing key cannot
upgrade an existing Revanced installation. Debug artifacts use the
`com.cloudstream.revanced.debug` application ID and debug signing, and are not
published as releases. The universal APK also includes x86 and x86_64.
The workflow verifies APK signatures before publishing. Release notes are in
`RELEASE_NOTES.md`; update them to reflect future changes before publishing.
