# Vajra-Neo

Custom kernel for Xiaomi `redwood`, the POCO X5 Pro 5G.
Linux 6.12.92 on Google's android16-6.12 common kernel, Android clang 19.0.1.

| | |
|---|---|
| Codenames | `redwood`, `redwoodin` |
| Defconfig | `redwood_defconfig` |
| Root | any manager with GKI support |

---

## 1.1 — 30 October 2026

- Lines and flicker on screen in some games are gone.
- "Hey Google" voice wake works.
- Digital USB-C earphones and DACs work.

## 1.0 — 27 October 2026

First release: redwood on Google's 6.12 common kernel, a new tree rather than the 5.4 kernel with
backports.

- Core from Google's `android16-6.12` (`android16-6.12-2026-09_r7`, Linux 6.12.92) with one change:
  without it the phone froze when waking from sleep with Wi-Fi connected.
- Every Qualcomm and Xiaomi driver runs as a module loaded from vendor_boot.
- MGLRU on by default, per-VMA locks, the EEVDF scheduler, sched_ext, zram with lz4 and zstd,
  io_uring, EROFS and f2fs compression.
- Clang CFI, shadow call stack and pointer authentication.
- BTF type information for the BPF programs Android 17 runs.
- Qualcomm's fixes for camera, video and mobile data, and upstream fixes for RPMh, clocks, power
  domains, pin control, firmware loading, remote processors, UFS and QRTR.
- Root with any manager you like.
