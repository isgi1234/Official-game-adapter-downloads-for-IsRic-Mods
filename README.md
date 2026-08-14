# Official game adapter downloads for IsRic Mods

This repository is the trusted download catalog used by the IsRic Mods launcher.
The launcher itself stays small. A game adapter is downloaded only after the user
opens that game and selects **Initialize mod config**.

## Current status

The catalog contains beta adapters for all ten launcher games. Every package has
at least 12 controls (Teardown has 14), an exact byte size, and a SHA-256 hash.
These are early compatibility builds: test them one game at a time and remove an
adapter from the launcher if a game update makes it incompatible.

PEAK is not downloaded from this repository because its working adapter is
currently bundled with IsRic Mods.

## Publishing an adapter

1. Build and test the adapter locally.
2. Put its files in a ZIP. The root of the ZIP must contain `adapter.json`.
3. Publish the ZIP under `packages/` or attach it to a GitHub release.
4. Calculate the ZIP's SHA-256 hash and exact byte size.
5. Update the matching entry in `catalog.json`:
   - set `available` to `true`;
   - set `version`;
   - set `downloadUrl` to the GitHub release asset URL;
   - set `sha256` and `size`.

The launcher accepts downloads only from this repository's raw `packages/`
directory or GitHub Releases,
checks the exact size and SHA-256 hash, blocks unsafe ZIP paths, and extracts only
the supported adapter file types.

## Adapter manifest

Example `adapter.json`:

```json
{
  "schemaVersion": 1,
  "key": "repo",
  "appId": "3241660",
  "version": "0.1.0",
  "files": [
    {
      "source": "payload/IsRic.RepoBridge.dll",
      "targetRoot": "game-mod",
      "targetPath": "IsRic.RepoBridge.dll"
    }
  ]
}
```

Supported target roots are deliberately narrow:

- `game-mod` installs only inside the game's approved mod/plugin directory.
- `game-loader` installs a known loader only inside approved game-root paths.
- `documents-mod` is accepted only for Teardown's built-in mod directory.
- `appdata-mod` is accepted only for Raft's `mods\IsRic Mods` directory (the
  historical manifest name is retained for compatibility).
- `localappdata-config` is accepted only for Brick Rigs' Unreal configuration.

R.E.P.O., Lethal Company, Content Warning, Muck, and TABS use the launcher's
bundled BepInEx runtime. People Playground and Teardown use their built-in mod
systems. Raft uses Raft Mod Loader source-mod format. Sons of the Forest includes
the official RedLoader 0.8.6 files in its on-demand package. Brick Rigs uses a
reversible, clearly delimited Unreal Engine settings section rather than code
injection.

The launcher records every installed file in `install-receipt.json`. Existing
files are backed up, removal restores them, and removal stops if an installed
file was edited afterward. Games must be closed during initialization/removal.

Each adapter must be made for the named game and should expose only intentional,
documented mod controls. Do not include anti-cheat bypasses, piracy tools, remote
code loaders, or public-lobby griefing features.
