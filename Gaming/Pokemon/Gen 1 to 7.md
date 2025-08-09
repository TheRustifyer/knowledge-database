# Creating the Ultimate Pokémon Machine, and more! (v2.0) 🎮⚡️

---

> [!] **Purpose:**  
> This recently updated 2024 guide explains how to get **every mainline Pokémon game from Gen 1 through Gen 7** onto your CFW (Custom Firmware) 3DS.  
> While mostly focused on Pokémon games, it covers installing *any* game from Gameboy to 3DS onto your home screen, including useful 3DS CFW modding tips for all users.

---

# Table of Contents Highlights

| Part | Topic |
|-------|--------------------|
| Part 1 | Useful Programs & Sources |
| Part 2 | Getting every game on your Home Screen |
| Part 3 | Migrating saves onto 3DS |
| Part 4 | Cheats & Hacks (PKSM, Checkpoint, etc.) |
| Part 5 | Randomizers, Patches, & Romhacks |
| Part 6 | Migrating Pokémon up and down generations |
| Part 7 | Trading |
| Part 8 | Peripheral Games & Accessories |
| Part 9 | Current unsolved CFW 3DS Pokémon mysteries |
| Part 10 | Bonus Stuff |

---

# Part 1: Useful Programs & Sources 🛠️

- **PKSM**: Pokémon save manager/editor for Gen I-VIII. Available in Universal Updater. Allows editing, duplicating, generating Pokémon, injecting Mystery Gifts and more.  
- **PKHex**: More powerful PC-based version of PKSM for all mainline Pokémon, including Switch games.  
- **HShop / 3hs App**: Website & app to download and install any 3DS & official Virtual Console (VC) games.  
- **GodMode9 / Godmode9i**: Tool to dump and restore cartridge saves and perform other system tasks.  
- **NDSForwarder**: Tool to put DS ROM files to your 3DS home screen (use version by MechanicalDragon from Universal Updater).  
- **TwilightMenu++**: Interface for DS games on 3DS, prettier than standard forwarders.  
- **FBI app**: Used to install .CIA files for 3DS and VC games.  
- **FTPD & 3DShell**: For quick file transfer and SD card file management via 3DS.  

> [!] *Do not use the Ghost eShop as their games tend to be buggy.*

---

# Part 2: Get Every Game on Your Home Screen 🎮

### Gameboy & Gameboy Color (Gen 1 & 2)

- Download Virtual Console (VC) games via HShop (Region Free section).  
- Include Japanese Red and Green for authentic experience.  

### Gameboy Advance (Gen 3)

- Download CIA files and install via FBI app, or use the "New Super Ultimate Injector 3DS" Windows app.  

### Nintendo DS (Gen 4 & 5)

- Obtain ROMs via ripping cartridges or downloading from trusted sources.  
- Store ROMs in `/ROMS/NDS` on SD card.  
- Use NDSForwarder or YANBF (up to 300 games supported) to install DS games to home screen.  
- **Important:** Do *not* rename `.nds` files once forwarders are created—this breaks the installation.  
- TwilightMenu++ is recommended as an alternative launcher.  

> [!] If you encounter "DSi binaries missing" error on Gen 5 games, replace the ROM with a new dump to restore full features.

### 3DS (Gen 6 & 7)

- Download via HShop/3hs app; don’t use Debug Builds.  
- Download updates for these games separately from HShop.

---

# Part 3: Migrating Saves 💾

- **Backup frequently** using Checkpoint and back up your SD card regularly.  
- Save files for each system should be placed in their respective folders, e.g., `/roms/nds/saves/` for Nintendo DS saves matching the ROM filename.  
- For VC and 3DS games, use Checkpoint to backup & restore saves.  
- If save files are not recognized, create a new save, then overwrite it with the old save on your PC.

---

# Part 4: Cheats & Hacks 🎰

- **PKSM** is the go-to 3DS app for managing and editing Pokémon across generations 1–7, including creation, duplication, event injection, and legality checks. Accessible via the Universal Updater.  
- Gen 3 GBA VC games require loading a save first to activate PKSM features.  
- **PKHex** is a powerful desktop editor supporting batch editing for all mainline Pokémon games including newer generations.  
- **Checkpoint** manages saves for 3DS and official VC games but not DS/GBA. Cheats here may sometimes cause crashes.  
- DS game cheats are available via NDSForwarder and TwilightMenu++ with an up-to-date NDS(i) Cheat Database.  
- Dream Radar has a portable save editor as well for hacking.  

> [!] **Banning and legitimacy:** No official reports of bans for using these tools, but beware using hacked Pokémon in competitions or trading online to avoid bans.

---

# Part 5: Randomizers, Patches, & Romhacks 🔀

- Universal Pokémon Randomizer ZX supports all games Gen 1 to 7.  
- Process includes loading a legally obtained, unencrypted ROM on a computer, randomizing settings, saving, then transferring to your SD card `/roms/` folder to play on 3DS.

---

# Part 6 to 10: Advanced Topics & Extras

Covers migration of Pokémon up/down generations, trading information, peripherals, current CFW mysteries, and bonus content for enthusiasts.

---

Thank you for being part of the community and enjoy building your ultimate Pokémon collection on the 3DS! 🙌🎉

*If you need help with any steps, programming tools, or troubleshooting, feel free to ask in the subreddit comments.*

