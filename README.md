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
3. Create a release `assets-vX.Y.Z` and attach the payloads.
4. Update `feed.json` on `main`: URLs, `sha256`, `size`, bump each changed asset's
   `version` and the global `packVersion`.

Launchers pick the update up on the next launch. To roll back, point `feed.json`
back to the previous release assets.

The manifest format and every validation rule enforced by the launcher are
documented in
[`docs/asset-packs.md`](https://github.com/MzKaxD/rotk-launcher/blob/main/docs/asset-packs.md).
