# Skill: Flash Custom ROM on Any Android Device

## Overview
This skill guides an agent through safely flashing a custom ROM on any Android device via ADB/fastboot. Follow every step in order. Do NOT skip research steps.

---

## Phase 0 — Research Before Touching Anything

Before running any commands, research the specific device:

1. Get device info first:
```bash
adb shell getprop ro.product.model
adb shell getprop ro.product.device
adb shell getprop ro.build.version.release
adb shell getprop ro.boot.flash.locked
```

2. **Search XDA Forums** for the exact device codename to find:
   - Is it A/B partitioned or A-only?
   - Which recovery is recommended (TWRP, OrangeFox, etc.)?
   - Which recovery build is compatible with the current OS version?
   - Any device-specific quirks or warnings?

3. **Never assume** a recovery build works — always verify it matches:
   - Device codename
   - Current OS version (e.g. Android 12 needs a different recovery than Android 11)
   - Partition scheme (A/B vs A-only)

---

## Phase 1 — Check Bootloader Status

```bash
adb shell getprop ro.boot.flash.locked
adb shell getprop ro.boot.verifiedbootstate
```

- `flash.locked = 1` → bootloader is locked, must unlock first
- `flash.locked = 0` + `verifiedbootstate = orange` → already unlocked, skip to Phase 3

---

## Phase 2 — Unlock Bootloader

> ⚠️ This wipes all data. Inform user before proceeding.

1. Enable Developer Options → OEM Unlocking on device
2. Reboot to fastboot:
```bash
adb reboot bootloader
fastboot devices  # verify connection
fastboot flashing unlock
```
3. Confirm on device screen (requires physical button press)
4. Verify unlock:
```bash
fastboot getvar unlocked
# must return: unlocked: yes
```

---

## Phase 3 — Research and Download Recovery

**Do not download the first result. Verify compatibility:**

- Search: `[device codename] [recovery name] [Android version] XDA download`
- Confirm the build is for the **exact Android/OS version** currently installed
- Download the `.img` file (not zip) for fastboot flashing
- For A/B devices: you will need to flash to both slots

**Check partition type:**
```bash
fastboot getvar current-slot  # A/B device if it returns a or b
```

---

## Phase 4 — Prepare vbmeta (A/B devices only)

Extract vbmeta from ROM zip payload and disable verification:

```bash
# Install tools
brew install payload-dumper-go  # macOS
# or: sudo apt install payload-dumper-go  # Linux

# Extract payload from ROM zip
unzip ROM.zip payload.bin -d ~/Downloads/

# Extract vbmeta images
payload-dumper-go -partitions vbmeta,vbmeta_system -o ~/Downloads/extracted ~/Downloads/payload.bin

# Flash with verification disabled to both slots
fastboot --disable-verity --disable-verification flash vbmeta_a ~/Downloads/extracted/vbmeta.img
fastboot --disable-verity --disable-verification flash vbmeta_b ~/Downloads/extracted/vbmeta.img
fastboot --disable-verity --disable-verification flash vbmeta_system_a ~/Downloads/extracted/vbmeta_system.img
fastboot --disable-verity --disable-verification flash vbmeta_system_b ~/Downloads/extracted/vbmeta_system.img
```

---

## Phase 5 — Flash Custom Recovery

### A/B devices (two slots):
```bash
fastboot flash recovery_a recovery.img
fastboot flash recovery_b recovery.img
fastboot reboot recovery
```

### A-only devices (single slot):
```bash
fastboot flash recovery recovery.img
fastboot reboot recovery
```

> ⚠️ If device boots to fastboot instead of recovery, the recovery image is wrong/incompatible. Go back to Phase 3 and find the correct build.

---

## Phase 6 — Wipe in Recovery

Once in custom recovery (TWRP/OrangeFox):
1. **Format Data** → type `yes`
2. Wipe System, Cache, Dalvik if available

---

## Phase 7 — Flash ROM

### Method 1 — ADB Sideload (recommended):
In recovery → Advanced → ADB Sideload → enable, then:
```bash
adb sideload ROM.zip
```

### Method 2 — ADB Push then flash:
```bash
adb push ROM.zip /sdcard/
# Then flash via recovery file manager
```

---

## Phase 8 — Reboot

Tap **Reboot System** in recovery. First boot takes **5–10 minutes** — do not interrupt.

---

## Recovering from Brick ("image destroyed" / bootloop)

If boot partition is corrupted:

1. Force into fastboot — hold **Volume Up + Volume Down** for 15–20 seconds with USB plugged in
2. Extract stock boot.img from ROM zip:
```bash
unzip ROM.zip payload.bin -d ~/Downloads/
payload-dumper-go -partitions boot -o ~/Downloads/extracted ~/Downloads/payload.bin
```
3. Flash boot to both slots:
```bash
fastboot flash boot_a ~/Downloads/extracted/boot.img
fastboot flash boot_b ~/Downloads/extracted/boot.img
```
4. Flash correct recovery to both slots (Phase 5)
5. Continue from Phase 4

---

## Common Mistakes to Avoid

| Mistake | Consequence | Fix |
|---|---|---|
| Using wrong OS-version recovery | Device stuck in fastboot / brick | Research correct build for exact Android version |
| Flashing only one slot on A/B device | Recovery gets overwritten on reboot | Always flash `_a` and `_b` slots |
| Skipping vbmeta disable | Custom recovery rejected / bootloop | Flash vbmeta with `--disable-verity --disable-verification` |
| Flashing recovery zip via fastboot | Brick | Use `.img` for fastboot, `.zip` only inside recovery |
| Not verifying `fastboot getvar unlocked` | Assuming unlock worked when it didn't | Always verify after unlock |
| Rushing without researching the device | Wrong files, bricks, wasted time | Always research Phase 0 before doing anything |

---

## Checklist Before Starting

- [ ] Device codename identified
- [ ] Partition scheme confirmed (A/B or A-only)
- [ ] Current Android/OS version noted
- [ ] Correct recovery build found and verified for this OS version
- [ ] ROM zip downloaded and verified for this device
- [ ] User informed that bootloader unlock wipes data
- [ ] User has backed up important data
