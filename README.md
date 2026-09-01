# ROTK asset packs

Custom asset feed for the [ROTK Launcher](https://github.com/MzKaxD/rotk-launcher).
The launcher fetches [`feed.json`](feed.json) from this branch before every game
launch, downloads the packs attached to the GitHub Releases of this repository,
verifies their SHA-256 and installs them into the ROTK client.

## Publishing an update

1. Build the payloads (`.zip` for file trees, plain files otherwise). Zip entries
   must use `/` separators (PowerShell `Compress-Archive` may emit `\` — prefer
   `7z`, `zip` or a Node script). No executable content (`.exe`, `.dll`, scripts…):
   the launcher rejects it.
2. Compute the SHA-256 and byte size of each payload:
   `Get-FileHash -Algorithm SHA256 <file>`.
3. Prepare a draft release `assets-vX.Y.Z` on the commit that contains the
   matching manifests, attach the changed payloads, then download them again and
   verify their exact size, SHA-256 and archive layout.
4. Publish the release before exposing its URLs through `feed.json` on `main`,
   then merge the manifest commit immediately. This avoids a window where
   launchers receive a feed whose payloads still return 404.
5. Update both `feed.json` and `asset-payloads.v1.json`: URLs, archive hashes and
   sizes in the feed; installed-file hashes, sizes and asset owners in the
   payload manifest; changed asset versions and the global `packVersion`.

Launchers pick the update up on the next launch. The latest stable GitHub release
is layered over `feed.json`, so changing the feed alone cannot roll back that
release. Roll back by publishing a higher version that restores the previous
payload and manifests (for example `assets-v1.5.1` after `assets-v1.5.0`), then
publish the matching attestation policy.

The manifest format and every validation rule enforced by the launcher are
documented in
[`docs/asset-packs.md`](https://github.com/MzKaxD/rotk-launcher/blob/main/docs/asset-packs.md).
