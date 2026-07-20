# NetHunter Kernel — LG K20 Plus (LV517)

**Android 7.1 · arm64 · LineageOS 14.1 base**

A custom Kali NetHunter kernel for the LG K20 Plus, built on LineageOS 14.1 [LV517] sources, tuned for wireless pentesting and the full NetHunter toolset.

**July 2026 update: Bluetooth USB dongle support added** — see [What's New](#whats-new-july-2026) below.

---

## What's New (July 2026) 
This update adds USB Bluetooth dongle support for use with the Nethunter Bluetooth Arsenal.

- Enabled `CONFIG_BT_ATH3K`, `CONFIG_BT_HCIUART`, `CONFIG_BT_HCIUART_H4`
- Fixed a linker error (`get_rome_version`/`rome_download`/`btusb_pm_sem` undefined references) caused by `CONFIG_BT_ATH3K` being off while `btusb.c` still referenced its symbols
- Rebuilt clean on a modern host toolchain (GCC 13.3.0) — required a `-fcommon` host-tools fix and a `scripts/gcc-wrapper.py` allowlist update, since this tree predates GCC 10's default behavior change
- Tested end-to-end on hardware: CSR USB BT dongle enumerates, `btusb` binds, `hci0` comes up clean (`UP RUNNING`, no errors)

Everything else in this README — WiFi injection, HID attacks, I/O scheduler, etc. — is the **original 2019 base kernel**, untouched.

---
<h2 align="center">What's In The Box</h2>
<h5 align="center">This kernel is for Android 7.1 arm64 only (works partly in 8.x)</h5>


| Category | Support |
| --- | --- |
| **SELinux** | Enforcing by default, flips to permissive when the NetHunter app asks nicely |
| **WiFi (OTG)** | Atheros, Ralink, Realtek USB adapters |
| **Packet Injection** | `mac80211` injection — the whole reason this kernel exists |
| **Bluetooth** | USB BT dongle support (ATH3K/HCIUART/HCIVHCI) — new as of this release, see below |
| **HID Attacks** | USB HID Gadget keyboard & mouse — HID Attacks, DuckHunter |
| **I/O Scheduler** | FIOPS by default, SIO available if you're feeling nostalgic |
| **DriveDroid** | Compatible |
| **Kernel Modules** | Full module support — `modprobe` works from the Kali chroot |
| **Network Filesystems** | CIFS and NFS baked in as modules |
| **Encryption** | Data partition encryption optional — custom ROM support varies, you're on your own |

---

## Bluetooth Support

**Added July 2026** — Changes to base kernel

- `CONFIG_BT_ATH3K`, `CONFIG_BT_HCIUART`, `CONFIG_BT_HCIUART_H4` (VHCI was already along for the ride)
- Confirmed working end-to-end with a CSR-chipset USB BT dongle — clean `hci0 UP RUNNING`, real TX/RX, no errors
- Unlocks Bluetooth Arsenal scanning and pairing from the NetHunter app

Bring the interface up from the chroot each session:

```bash
hciconfig hci0 up
hciconfig hci0
```

Standard BR/EDR scan/pair tooling should just work. BLE and passive-sniffing capability depend on your specific dongle's chipset — a generic CSR adapter gets you classic Bluetooth, not Ubertooth-grade packet capture.

---

## Installation

1. Grab the latest flashable zip from [Releases](../../releases)
2. Boot into TWRP
3. Flash it
4. Reboot.

> Requires an unlocked bootloader and TWRP already on your K20 Plus (LV517). This repo assumes you do.

## Disclaimer
For educational and authorized security testing purposes only. This is pentesting gear for a phone you own, testing networks you're allowed to touch. Flashing custom kernels carries real risk to your device — that's on you, not this README.
00
