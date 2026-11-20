# GW1-DeRank

**GW1-DeRank** is a lightweight DLL utility for Plants vs. Zombies: Garden Warfare 1 that detects modified account ranks and restores them to legal values.

It automatically checks your account when you reach the main menu and, if necessary, applies the rank settings defined in the configuration file.

Changes are saved through the game's own persistence system, meaning they remain saved across game restarts and multiplayer sessions.

## Installation

1. Download `dinput8.dll`.
2. Place it in your game directory, next to `PVZ.Main_Win64_Retail.exe`.
3. Launch the game normally.
4. Sign in and wait until the main menu.

GW1-DeRank will automatically detect your account rank and apply the configured settings if necessary.

### First launch

On the first launch, GW1-DeRank automatically creates its configuration and log files.

A message box will display the location where the configuration file was created:

- **OK:** Opens the configuration folder in Windows Explorer and closes the game. You can edit the settings before launching the game again.
- **Cancel:** Continues with the default configuration.

The game will wait until you make a selection.

## Configuration

The configuration file is located at:

`%LOCALAPPDATA%\GW1-DeRank\gw1-derankconfig.ini`

### Available settings

| Setting | Description |
|---|---|
| `AlwaysSetRank` | When enabled, applies your configured ranks at every sign-in, regardless of whether your current ranks are legal. |
| `SetAccountRankTo0` | Sets all eight characters to rank 1. |
| `SetAccountRankTo313` | Sets all eight characters to rank 40 (account rank 313). |
| `SetCustomRankConfig` | Enables individual rank settings for each character, overriding the two account rank options above. |

**Choosing custom ranks**

When `SetCustomRankConfig` is enabled, you can configure each character independently using values between 1 and 40.

For example:

```ini
SetCustomRankConfig = true

SoldierRank = 40
EngineerRank = 30
ScientistRank = 25
AllStarRank = 20
PeashooterRank = 40
ChomperRank = 15
SunflowerRank = 35
CactusRank = 10
```

This configuration will apply the specified rank to each character.

**Important configuration notes:**

- `SetCustomRankConfig` takes priority over `SetAccountRankTo0` and `SetAccountRankTo313`.
- When custom ranks are disabled, exactly one of the two account rank options must be enabled.
- Custom ranks above 40 are automatically limited to 40.
- Values below 1 or invalid values will cause a configuration error.
- If the configuration contains an error, GW1-DeRank will log it and skip all modifications for that session.
- Configuration changes take effect on the next game launch/sign-in.

## Rank Detection

GW1-DeRank considers an account modified if either of the following conditions is met:

- The total account rank exceeds **313**.
- Any individual character exceeds rank **40**.

If an illegal rank is detected, the configured ranks are applied automatically.

Legal ranks are left untouched unless `AlwaysSetRank = true`.

### Extremely high ranks (Account Brick Protection)

Some modified accounts have character ranks set to extremely high values.

These accounts can crash the game during sign-in because the game's progression system attempts to process every level gained.

GW1-DeRank includes a protection mechanism that detects these accounts during sign-in and corrects their character ranks before the game processes the invalid progression data.

This allows accounts that would otherwise crash during sign-in to reach the main menu and have their ranks corrected normally.

**Important:** This protection only works with `dinput8.dll` installed before launching the game. Manual injection cannot provide this functionality because it happens too late in the sign-in process.

##  Logs
A log file is also generated at:

`%LOCALAPPDATA%\GW1-DeRank\GW1-DeRank.log`

The log contains additional information, including errors and details about operations performed by the tool.

If the local application data folder cannot be accessed, GW1-DeRank will attempt to use the Windows temporary folder instead.

## Important Notes

- **Windows:** Requires 64-bit Windows 8 or newer.
- **Antivirus:** The DLLs are unsigned. Windows Defender, third-party antivirus software, or Windows Smart App Control may block or quarantine them. If the tool does not appear to work, check your security software.
- **Rank accuracy:** When repairing extremely high ranks, setting characters to rank 40 or custom ranks may occasionally result in a rank one level lower than requested. This appears to be due to the game’s internal rank calculation and save systems (or at least, that is my understanding). Setting all characters to rank 1 does not present this limitation.

## Uninstallation

To completely remove GW1-DeRank:

1. Close the game.

2. Delete `dinput8.dll` from your game directory

3. Delete the following folder to remove the configuration and logs:

   `%LOCALAPPDATA%\GW1-DeRank`
