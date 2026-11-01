# Dirac Audio NP1 — universal build (AIDL shim)

Dirac sound tuning for devices where the original module no longer works (AIDL audio HAL, Android 14+). Requires root (KernelSU / Magisk).

Based on [reiryuki/Dirac-Audio-Nothing-Phone-1-Magisk-Module](https://github.com/reiryuki/Dirac-Audio-Nothing-Phone-1-Magisk-Module) v1.6. Dirac/Nothing blobs belong to their owners.

## Install
1. Download the zip from Releases
2. KernelSU/Magisk → Modules → Install from storage → pick the zip
3. Reboot

Use at your own risk.

## What the installer picks
- AIDL audio effect HAL (`android.hardware.audio.effect.IFactory/default`, most Android 14+ devices):
  `libdiraceffect-afm.so` = AIDL shim, original library kept as `libdiraceffect-afm-legacy.so`.
  Effect entry gets `type="ec7178ec-..."` (required by AIDL factories).
- HIDL effect HAL: original library only, no shim, no space stage.
- Override in `/sdcard/optionals.prop`: `dirac.shim=1` (force shim) or `dirac.shim=0` (force legacy).

## Fixes for Android 17 (all devices)
- sepolicy: `priv_app_36` / `system_app` may read `vendor_default_prop` (else Dirac app ignores forceAfm).
- `BLUETOOTH_CONNECT` granted to com.nothing.dirac_DMP (else crash loop on mode switch).

## Space stage (shim only, applied only while Dirac is active)
```
resetprop vendor.dirac.x15.width 125   # stereo width %, 100 = off
resetprop vendor.dirac.x15.room 10     # room %, 0 = off
```
Then restart the audio HAL or toggle Dirac.

## Known limits
- Shim built against audio effect AIDL V2 (Android 15 vendor). Tested only on Xiaomi 15 (SM8750, HyperOS 4 / Android 17).
- Non-Qualcomm factories (Samsung, MediaTek) untested.
- Use the app from the main profile (work/Island copies write their own settings).

## Removal
KernelSU/Magisk: remove the module, reboot. Bootloop: boot into safe mode (usually volume down during boot; the key combo varies by device).

## Credits
- Original module: [reiryuki](https://github.com/reiryuki)
- AIDL shim and Android 17 fixes: [CapStarCat](https://github.com/CapStarCat), developed with the help of [Claude](https://claude.ai) (Anthropic).
