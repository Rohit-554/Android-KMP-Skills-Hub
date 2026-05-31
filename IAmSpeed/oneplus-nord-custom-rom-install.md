# Skill: Install Custom ROM on OnePlus Nord (avicii / AC2001 / AC2003)

## Device Info
- **Model**: OnePlus Nord (AC2001 India / AC2003 Europe)
- **Codename**: avicii
- **Chipset**: Qualcomm Snapdragon 765G
- **Partition type**: A/B (dual slot)

---

## Critical Lessons Learned (Do NOT repeat these mistakes)

### 1. Always flash recovery to BOTH slots
OnePlus Nord is an A/B device. Flashing only to `recovery` or one slot causes the device to boot into fastboot instead of recovery.

**Correct commands:**
```bash
fastboot flash recovery_a OrangeFox.img
fastboot flash recovery_b OrangeFox.img
```

### 2. Use the OOS12-specific OrangeFox build
If the device is on **OxygenOS 12 / Android 12**, the standard FBEv1/FBEv2 OrangeFox builds will NOT work. Use the OOS12 variant:

- **Correct build**: `OrangeFox-R11.1_3_OOS12-Unofficial-avicii.img`
- **Download**: https://sourceforge.net/projects/oneplus-nord-recovery-builds/files/Orange%20Fox/OOS12/Flashable_Image/

Using the wrong recovery (e.g. R11.3 zip's recovery.img) will corrupt the boot partition and show:
> "The current image (boot/recovery) have been destroyed and cannot boot"

### 3. Do NOT flash the R11.3 zip's recovery.img directly
The `recovery.img` extracted from `OrangeFox-R11.3_1-Unofficial-avicii.zip` is incompatible with OOS12 and will brick the device (corrupts boot partition).

### 4. Disable vbmeta verification
Flash vbmeta with verification disabled on both slots before flashing recovery:
```bash
fastboot --disable-verity --disable-verification flash vbmeta_a vbmeta.img
fastboot --disable-verity --disable-verification flash vbmeta_b vbmeta.img
fastboot --disable-verity --disable-verification flash vbmeta_system_a vbmeta_system.img
fastboot --disable-verity --disable-verification flash vbmeta_system_b vbmeta_system.img
```
Extract vbmeta images from the ROM's `payload.bin` using `payload-dumper-go`.

### 5. Flash boot.img to both slots when recovering from brick
If boot partition is corrupted, flash to both slots:
```bash
fastboot flash boot_a boot.img
fastboot flash boot_b boot.img
```
Extract `boot.img` from the ROM zip's `payload.bin`:
```bash
brew install payload-dumper-go
unzip ROM.zip payload.bin
payload-dumper-go -partitions boot -o ~/Downloads/extracted payload.bin
```

---

## Correct Step-by-Step Process

### Prerequisites
- Unlocked bootloader (OEM unlock enabled in Developer Options)
- ADB/fastboot installed on Mac/Linux/Windows
- Files needed:
  - `OrangeFox-R11.1_3_OOS12-Unofficial-avicii.img` (for OOS12 devices)
  - Project Infinity X ROM zip (or any custom ROM)

### Step 1 — Unlock Bootloader
```bash
adb reboot bootloader
fastboot flashing unlock
# Confirm on device screen using volume + power buttons
# If power button broken: fastboot oem unlock-go (then confirm on screen)
```

### Step 2 — Extract vbmeta from ROM zip
```bash
brew install payload-dumper-go
unzip ROM.zip payload.bin -d ~/Downloads/
payload-dumper-go -partitions vbmeta,vbmeta_system -o ~/Downloads/extracted payload.bin
```

### Step 3 — Disable vbmeta verification
```bash
fastboot --disable-verity --disable-verification flash vbmeta_a ~/Downloads/extracted/vbmeta.img
fastboot --disable-verity --disable-verification flash vbmeta_b ~/Downloads/extracted/vbmeta.img
fastboot --disable-verity --disable-verification flash vbmeta_system_a ~/Downloads/extracted/vbmeta_system.img
fastboot --disable-verity --disable-verification flash vbmeta_system_b ~/Downloads/extracted/vbmeta_system.img
```

### Step 4 — Flash OrangeFox recovery (OOS12 build) to BOTH slots
```bash
fastboot flash recovery_a OrangeFox-R11.1_3_OOS12-Unofficial-avicii.img
fastboot flash recovery_b OrangeFox-R11.1_3_OOS12-Unofficial-avicii.img
fastboot reboot recovery
```

### Step 5 — In OrangeFox: Wipe data
- Tap **Format Data** → type `yes`

### Step 6 — Flash ROM via ADB Sideload
In OrangeFox → Advanced → ADB Sideload → enable it, then:
```bash
adb sideload ROM.zip
```

### Step 7 — Reboot
Tap **Reboot System** in OrangeFox. First boot takes 5–10 minutes.

---

## Recovering from "current image destroyed" brick

1. Hold **Volume Up + Volume Down** until screen goes black → device enters fastboot
2. Extract boot.img from ROM zip payload.bin (see Step 2 above)
3. Flash boot to both slots:
```bash
fastboot flash boot_a boot.img
fastboot flash boot_b boot.img
```
4. Flash correct OOS12 recovery to both slots (Step 4 above)
5. Follow from Step 3 onwards

---

## Device-specific notes
- **Power button broken**: Use OTG adapter + mouse to confirm bootloader unlock screen, or try `fastboot oem unlock-go`
- **Assistive Touch does NOT work** on bootloader/fastboot screens — Android UI only
- Project Infinity X ROM for this device: https://xdaforums.com/t/rom-official-16-gapps-vanilla-oneplus-nord-avicii-project-infinity-x-v3-x.4753418/
