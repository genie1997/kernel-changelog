# Vajra

Custom kernel for Xiaomi `redwood` — the POCO X5 Pro 5G.
Linux 5.4.302, built on the Scarlet base with Neutron Clang 24.

| | |
|---|---|
| Codenames | `redwood`, `redwoodin` — the installer accepts both |
| Defconfig | `vendor/xiaomi-qgki_defconfig` + `vendor/redwood.config` + `vendor/vajra.config` |
| Root | KernelSU-Next, compiled in — manual hooks, no kprobes |
| SuSFS | 2.3.0, compiled in |

---

## 4.1 — 16 October 2026

- Far fewer CPU wakeups while the phone is idle, with the screen on or off.
- Scheduler: a light task now takes an idle little core before an idle big one, and false
  throttle readings no longer push light tasks to the big cores.
- Wakeup reasons are recorded for battery stats, including wakes from the modem and the DSP.
- Vibration and audio no longer flood the kernel log.
- Fixes for binder, input and HID, USB (audio and tethering included), ext4, timers, cpufreq,
  workqueues, cgroups and memory management.
- Networking fixes for IPv4, IPv6, TCP, netfilter and IPsec.
- Use the [KernelSU-Next 3.4.2 manager](https://github.com/KernelSU-Next/KernelSU-Next/releases/download/v3.4.2/KernelSU_Next_v3.4.2_33355-release.apk).

## 4.0 — 14 October 2026

- f2fs, fscrypt and fs-verity updated to the android12-5.4 level, with fixes up to 2025. f2fs takes the
  storage options newer Android sets and the GC pacing controls its idle maintenance uses.
- Camera: fixed a memory leak that kept growing while the camera was open, plus Qualcomm camera driver
  fixes for out-of-bounds reads, use-after-free and races.
- Qualcomm fixes from newer releases for audio (concurrent recording, compressed playback), video,
  Wi-Fi, mobile data, UFS and the Bluetooth UART.
- Fixes for binder use-after-free, a Bluetooth crash on suspend, a hotword session leak and mobile
  data control messages sent with the wrong field size.
- WireGuard: the fixes from mainline that apply to 5.4.
- Touch: the report rate and game mode settings are sent to the panel again when the screen turns on.
- Touch: a gesture with the screen off no longer keeps the phone from going to sleep.
- Root: network connections opened as root are no longer refused.
- Works with the KernelSU-Next 3.4.2 manager. The 3.4.1 manager keeps working too, and there is no
  install order this time.
- Root: the new manager option to give the shell root only from an adb session (off by default) now
  works, and the SELinux policy size reported after root rules are added is exact.
- Same SuSFS 2.3.0. The manager shows the kernel side as v3.4.0-legacy (33309); that is expected.

## 3.2 — 10 October 2026

- Works with the KernelSU-Next 3.4.1 manager, and needs it: the 3.4.0 manager asks for an update and
  modules stay off until it is replaced. Update the manager and open it once before flashing; if the
  kernel went on first, one more reboot brings the modules back.
- Module service scripts start once per boot, even on ROMs that reach that stage twice.
- SELinux fixes in the root code: a second ioctl permission for an existing rule is now merged in
  instead of being dropped, and removing a rule keeps the policy size exact.
- Same SuSFS 2.3.0. The manager shows the kernel side as v3.4.0-legacy (33305); that is expected.

## 3.1 — 4 October 2026

- Trimmed the SELinux footprint further — the root domain dropped an unused rule and narrowed another.
- SuSFS: fixed `sus_mount` hiding to use the same audience as the rest of the hidden-file
  features, instead of a separate one.
- Same KernelSU-Next 3.4.0-legacy and SuSFS 2.3.0; keep the 3.4.0 manager.

## 3.0.1 — 1 October 2026

- Fixes the manager showing as unsupported on 3.0. The kernel's first search for the manager now
  runs at post-fs-data, early in boot, instead of a later stage a clean install could never reach —
  so the manager is recognised on the first boot, and much sooner.
- Same features as 3.0; keep the 3.4.0 manager.

## 3.0 — 30 September 2026

- Several root features that were compiled in but never reachable are now active: safe mode, adb
  root, su exec logging and the audit consumer.
- Haptics: the vibration effect an app asks for is the one that actually plays. The driver was
  ignoring the requested effect id and always falling back to the first one.
- Driver fixes across the root and filesystem code, plus new filesystem options in the redwood
  config — tmpfs xattrs and ACLs, and the overlayfs correctness set.
- A display fix that also closes a crash path on a non-DSI connector, and an erofs read fix. Both
  had been build warnings for a long time; the tree now builds clean.
- Same KernelSU-Next 3.4.0-legacy and SuSFS 2.3.0; keep the 3.4.0 manager.

## 2.3 — 29 September 2026

- Hardening across the root and SELinux code — closes a use-after-free that could hit under memory
  pressure, adds a lock that was missing between the two paths that rewrite the live policy, and
  fixes a context-check hook that could log a bit of kernel memory.
- The granted-app list is no longer rewritten from a partial read, so a bad read can't quietly drop
  your apps.
- Trimmed the SELinux footprint — the root domain dropped a few attributes it never used. Same
  KernelSU-Next 3.4.0-legacy and SuSFS 2.3.0; keep the 3.4.0 manager.

## 2.2 — 28 September 2026

- KernelSU-Next updated to 3.4.0-legacy. Use the KernelSU-Next 3.4.0 manager — the same one 2.0
  and 2.1 used, so no reinstall if you already have it.
- Memory-safety fixes that came with the update — tighter string and buffer handling in the root
  code.

## 2.1 — 27 September 2026

- Adds the kernel side of compressed A/B updates, for ROMs that ship them.

## 2.0 — 24 September 2026

- SuSFS is updated to 2.3.0 — a big chunk of it was reworked this release.
- KernelSU-Next is updated. You'll need the KernelSU-Next 3.4.0 manager for this build; older
  managers won't work with it. Grab it from the KernelSU-Next releases page.
- Fixed a reboot that could panic the phone when you had modules installed.

## 1.0.2 — 19 September 2026

Installer fixes only, same kernel as 1.0.

- Fix the installer not finding the boot partition (the AnyKernel3 vars were lowercase).
- Fix `redwoodin` units being turned away at the device check.
- Pulled the broken `Vajra-1.0.zip` / `1.0.1.zip` — use this one instead.

## 1.0 — 18 September 2026

First release.

- KernelSU-Next compiled in — root the moment you flash, no ramdisk patch or separate boot image.
- SuSFS 2.2.0 compiled in (sus_path, sus_mount, sus_kstat, sus_map, open_redirect, uname/cmdline
  spoof, symbol hiding). The SuSFS module isn't needed.
- Add the `redwood` and `vajra` config fragments.

---

## Flashing

Franco Kernel Manager's built-in flasher, or `adb sideload Vajra-2.3.zip` from recovery.

It is an AnyKernel3 zip, kernel-only: it replaces the Image and leaves your ROM's ramdisk alone, so it
flashes on top of whatever redwood ROM you are already running. No wipe, no data loss.

- AOSP ROMs only — LineageOS, PixelOS and similar, Android 17 and below. Not made for MIUI or HyperOS.
- Flashing this roots your ROM. With root and an unlocked bootloader, banking, payment and some DRM
  apps may refuse to run. Go in knowing that.

## Managers

- KernelSU-Next manager — https://github.com/KernelSU-Next/KernelSU-Next/releases
- SuSFS module, optional — https://github.com/sidex15/susfs4ksu-module/releases/latest
  SuSFS is already in the kernel, so this is not needed for the basics. Add it only if you want the
  WebUI and manual control over what gets hidden.
- Do not stack other root-hiding modules such as Shamiko or Zygisk Assistant on top. They fight
  each other.

## If it does not boot

The platform reserves 4 MB of ramoops — 2 MB for the panic record, 2 MB for the Android log — so a
kernel panic leaves a backtrace behind instead of a mystery.

1. Do not power the phone off. That memory survives a reboot; it does not survive a power-off or a
   battery pull, and the log goes with it.
2. Reboot to recovery or fastboot and restore your previous `boot.img` to get a booting system back.
3. Pull the log from the failed boot and attach it to an issue:

```
adb shell su -c 'cat /sys/fs/pstore/dmesg-ramoops-0'
```

That file is the panic itself — the backtrace and the last thing the kernel did. `/sys/fs/pstore/pmsg-0`
holds the Android-side log from the same boot, if it got that far. There is no `console-ramoops` on
this device, the device tree does not reserve one, so `dmesg-ramoops-0` is the file that matters.

## Credits

Built on [Scarlet](https://github.com/Atom-X-Devs/scarlet_xiaomi_sm7325) by
[Tashfin Shakeer Rhythm](https://github.com/Tashar02) and
[Atom-X-Devs](https://github.com/Atom-X-Devs), from base commit `6697b3ec`. This kernel is my own
modification on top of their tree — anything wrong with this build is mine to fix, not theirs.

SuSFS by [simonpunk](https://gitlab.com/simonpunk/susfs4ksu).
KernelSU-Next by the [KernelSU-Next team](https://github.com/KernelSU-Next/KernelSU-Next).
