
# 📱 Flashing Guide for Lenovo TB125FU: Step-by-Step with Fastboot Logs

Hey there! Here's a curated Markdown version of your fastboot flashing steps, formatted nicely for Obsidian. I've organized it into clear sections with emojis for quick scanning (e.g., 🚀 for starts, ✅ for successes, ❌ for failures). Each step includes the command, output, and a brief note. This is based on the logs you provided—perfect for saving as a note to track your LineageOS installation process. Feel free to tweak it!

## 🚀 Preparation Notes

- **Device:** Lenovo TB125FU (stuck in fastboot issues during LineageOS flashing).
- **Goal:** Unlock bootloader, flash vbmeta and boot with disabled verity/verification, set active slot, and delete logical partitions.
- **Tools:** Fastboot (ensure latest platform-tools).
- **Tip:** Always back up data before flashing! ⚠️


## Step 1: Unlock Bootloader ✅

🚀 Command to unlock the bootloader flow.

```bash
fastboot flashing unlock
```

**Output:**

```
(bootloader) Start unlock flow

OKAY [  6.312s]
Finished. Total time: 6.312s
```

**Note:** Success! Bootloader unlocked. Took about 6 seconds. 🎉

## Step 2: Flash vbmeta with Disabled Verity/Verification ✅

🚀 Flashing a blank vbmeta image to disable checks.

```bash
fastboot flash --disable-verity --disable-verification vbmeta Telegram\ Desktop/vbmeta.img
```

**Output:**

```
Rewriting vbmeta struct at offset: 0
Sending 'vbmeta_a' (4 KB)                          OKAY [  0.042s]
Writing 'vbmeta_a'                                 OKAY [  0.049s]
Finished. Total time: 0.191s
```

**Note:** Went smoothly. This bypasses boot verification for custom ROMs. 🔒

## Step 3: Flash Boot Image with Disabled Verity/Verification ✅

🚀 Flashing the boot image to the active slot.

```bash
fastboot --disable-verity --disable-verification flash boot Telegram\ Desktop/boot.img
```

**Output:**

```
Sending 'boot_a' (32768 KB)                        OKAY [ 20.520s]
Writing 'boot_a'                                   OKAY [  0.471s]
Finished. Total time: 21.138s
```

**Note:** Success, but it took ~21 seconds due to file size. Patience is key! ⏳

## Step 4: Set Active Slot to 'a' ✅

🚀 Ensuring we're using slot A.

```bash
fastboot --set-active=a
```

**Output:**

```
Setting current slot to 'a'                        OKAY [  0.040s]
Finished. Total time: 0.045s
```

**Note:** Quick win! Slot A is now active. 🔄

## Step 5: Delete Logical Partition 'product_a' (Initial Attempt) ❌

🚀 Trying to delete product_a partition.

```bash
fastboot delete-logical-partition product_a
```

**Output:**

```
Deleting 'product_a'                               FAILED (remote: 'unknown command')
fastboot: error: Command failed
```

**Note:** Failed due to unknown command. This often happens in standard fastboot mode—need to switch to fastbootd. 😕

## Step 6: Reboot into Fastbootd Mode ✅

🚀 Switching to fastbootd for dynamic partition operations.

```bash
fastboot reboot fastboot
```

**Output:**

```
Rebooting into fastboot                            OKAY [  0.031s]
< waiting for any device >
Finished. Total time: 23.102s
```

**Note:** Success! Device is now in fastbootd mode after ~23 seconds wait. 🛠️

## Step 7: Delete Logical Partition 'product_a' (in Fastbootd) ✅

🚀 Retrying deletion in fastbootd.

```bash
fastboot delete-logical-partition product_a
```

**Output:**

```
Deleting 'product_a'                               OKAY [  0.008s]
Finished. Total time: 0.008s
```

**Note:** Worked this time! Fastbootd handles dynamic partitions better. 👍

## Step 8: Delete Logical Partition 'product_b' (in Fastbootd) ✅

🚀 Deleting the B slot partition for completeness.

```bash
fastboot delete-logical-partition product_b
```

**Output:**

```
Deleting 'product_b'                               OKAY [  0.008s]
Finished. Total time: 0.008s
```

**Note:** Quick success. Partitions cleared—ready for system flashing next! 🧹

## Step 8: Flash the OS 🚀

```
fastboot flash system_a <img_name>.img
```

## 📝 Next Steps \& Tips

- **Reboot \& Test:** After all flashes, run `fastboot reboot` and check for boot animation. If stuck, verify active slot with `fastboot getvar current-slot`.
- **Troubleshooting:** If errors persist, try a different GSI like LineageOS 21 (Android 14) from the list we discussed. Always use matching firmware images. ❗
- **Save This Note:** Copy-paste into Obsidian for easy reference. Add your own tags like \#Android \#Flashing \#Lenovo.

