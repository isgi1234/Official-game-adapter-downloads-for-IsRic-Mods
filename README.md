# Official game adapter downloads for IsRic Mods

This repository is the trusted download catalog used by the IsRic Mods launcher.
The launcher itself stays small. A game adapter is downloaded only after the user
opens that game and selects **Initialize mod config**.

## Current status

The catalog contains the first ten planned games. They are intentionally marked
as unavailable until each adapter has been built and tested. This prevents the
launcher from installing placeholders or incomplete mods.

PEAK is not downloaded from this repository because its working adapter is
currently bundled with IsRic Mods.

## Publishing an adapter

1. Build and test the adapter locally.
2. Put its files in a ZIP. The root of the ZIP must contain `adapter.json`.
3. Create a GitHub release in this repository and attach the ZIP.
4. Calculate the ZIP's SHA-256 hash and exact byte size.
5. Update the matching entry in `catalog.json`:
   - set `available` to `true`;
   - set `version`;
   - set `downloadUrl` to the GitHub release asset URL;
   - set `sha256` and `size`.

The launcher accepts downloads only from this repository's GitHub Releases,
checks the exact size and SHA-256 hash, blocks unsafe ZIP paths, and extracts only
the supported adapter file types.

## Adapter manifest

Example `adapter.json`:

```json
{
  "schemaVersion": 1,
  "key": "repo",
  "name": "R.E.P.O. adapter",
  "version": "0.1.0",
  "gameAppId": "3241660",
  "entryAssembly": "IsRic.RepoBridge.dll"
}
```

Each adapter must be made for the named game and should expose only intentional,
documented mod controls. Do not include anti-cheat bypasses, piracy tools, remote
code loaders, or public-lobby griefing features.
