# TekkenRTTDisplay

Shows your real pre-battle ping (RTT, in milliseconds) on the matchmaking
accept dialog, instead of only the bucketed antenna-bar quality indicator.
The number is read directly from the game's own RTT statistics manager,
before you ever accept or decline.

Requires [UE4SS](https://github.com/UE4SS-RE/RE-UE4SS) (a scripting/modding
framework for Unreal Engine games). If you've never installed UE4SS before,
follow **Part 1** below first. If you already have UE4SS working for Tekken
8, skip straight to **Part 2**.

## Demo
https://github.com/user-attachments/assets/638f9562-2f1d-4c6a-b08f-28c66dbb8b96




---

## Part 1: Installing UE4SS (skip if you already have it)

UE4SS is not made by this mod's author -- it's a separate, widely-used
open-source modding framework that many Tekken 8 mods depend on, similar to
how many Skyrim mods require SKSE.

1. **Find your Tekken 8 install folder.**
   In Steam, go to your Library, right-click **TEKKEN 8** -> **Manage** ->
   **Browse local files**. This opens a folder that looks like:
   `...\steamapps\common\TEKKEN 8`

   From there, open the subfolder: `Polaris\Binaries\Win64`

   You should see `Polaris-Win64-Shipping.exe` in this folder. This exact
   folder (`Win64`) is where everything in this guide gets placed --
   **not** the top-level `TEKKEN 8` folder.

2. **Download UE4SS.**
   Go to the UE4SS releases page and find the **v3.0.1** release:
   https://github.com/UE4SS-RE/RE-UE4SS/releases/tag/v3.0.1

   Under "Assets", download `UE4SS_v3.0.1.zip`.

   (This mod was built and tested against exactly this version. A newer
   UE4SS version may or may not still work -- see "Known limitations"
   below. If in doubt, use this exact version.)

3. **Extract it into the Win64 folder.**
   Open the zip and extract **all of its contents** directly into the
   `Win64` folder from step 1 -- so that `UE4SS.dll`, `dwmapi.dll`,
   `UE4SS-settings.ini`, and a new `Mods` folder all end up sitting right
   next to `Polaris-Win64-Shipping.exe`, not inside a subfolder.

4. **Launch the game once to confirm UE4SS is working.**
   Start Tekken 8 normally through Steam. Then close it again, go back to
   the `Win64` folder, and check for a new file called `UE4SS.log`. If it
   exists and has content, UE4SS is working. If you don't see it at all,
   see Troubleshooting below before continuing.

5. **(Optional) Turn off the other mods UE4SS bundles by default.**
   The stock UE4SS download comes with several utility/debug mods already
   present in `Mods\` (console access, a cheat manager, etc.), some
   enabled out of the box. If you only want this ping display and nothing
   else, open `Win64\Mods\mods.txt` in a text editor and change every
   `: 1` to `: 0` (or just delete those mod folders from `Mods\` entirely).
   This mod doesn't depend on any of them.

## Part 2: Installing TekkenRTTDisplay

1. In the `Win64` folder from Part 1, open the `Mods` folder (UE4SS
   created this for you).
2. Copy the entire `TekkenRTTDisplay` folder (the one in TekkenRTTDisplay-release.zip) into `Mods`, so you end up with:
   `Win64\Mods\TekkenRTTDisplay\enabled.txt`
   `Win64\Mods\TekkenRTTDisplay\Scripts\main.lua`
   `Win64\Mods\TekkenRTTDisplay\dlls\main.dll`

   (A common mistake: some zip tools create an extra nested folder. Make
   sure `enabled.txt` is directly inside `TekkenRTTDisplay`, not inside
   `TekkenRTTDisplay\TekkenRTTDisplay`.)
3. Launch the game. No further setup, no editing any config files.
4. To verify it loaded: check `UE4SS.log` for a line like
   `Mod 'TekkenRTTDisplay' has enabled.txt, starting mod.`
5. Go into matchmaking (Ranked or Quick Match). Once an opponent is found
   and the accept dialog appears, the connection-rate label should now
   read something like `Ping: 67 ms | Disconnection Rate: 0%`.

### Uninstalling

Delete the `Mods\TekkenRTTDisplay` folder. That's it -- nothing else on
your system is touched. (To also remove UE4SS entirely, delete
`UE4SS.dll`, `dwmapi.dll`, `UE4SS-settings.ini`, `UE4SS.log`, and the
`Mods` folder from `Win64`.)

## Troubleshooting

- **No `UE4SS.log` ever appears.** UE4SS isn't loading. Double check the
  files from Part 1 were extracted into `Win64` (the folder containing
  `Polaris-Win64-Shipping.exe`), not the top-level `TEKKEN 8` folder.
- **Antivirus deleted or quarantined `dwmapi.dll`.** This is a known false
  positive -- the technique UE4SS uses to load itself (replacing a system
  DLL with a proxy that forwards to the real one) resembles something
  malware also does, even though this is legitimate open-source software.
  Restore it from quarantine, or add an exclusion for the `Win64` folder
  in your antivirus settings.
- **`UE4SS.log` looks fine but there's no `TekkenRTTDisplay` line in it.**
  Re-check the folder structure from Part 2, step 2.
- **Mod loads (per the log) but no ping ever shows.** The memory offsets
  this mod uses are specific to the exact game build they were found on
  -- see "Known limitations" below.

## Known limitations

- The memory offsets this mod uses are specific to the exact game build
  they were found on. If Bandai Namco patches the game, these offsets can
  shift. The read is wrapped in exception handling, so a broken offset
  should just mean the ping display falls back to the game's own text
  rather than crashing -- but a value that's wrong-but-plausible after a
  patch is a theoretical risk until offsets are re-verified.
- This is unaffiliated fan-made software, not endorsed by Bandai Namco or
  the UE4SS project.

## Source / how it works

Full source (including the native component) is available on GitHub for
anyone who wants to verify what this does or rebuild it themselves:
(https://github.com/mulkmulkmulk/TekkenRTTDisplay/blob/main/TekkenRTTDisplay-source.zip)
