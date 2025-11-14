# **LineageOS GSI Adaptation for Samsung Galaxy A03 Core**

## 📱 Maintained unofficially by **MarsXz**
## Base GSI by **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)**

---

## 🌍 Choose your language
**English** | [Русский](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-RU.md) | [Қазақша](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-KZ.md) | [Український](https://github.com/MarsXz8656/LineageOs-Samsung-Galaxy-A03Core-gsi-adaptation/blob/main/README-UA.md)

---

This project provides an adapted **AOSP/LineageOS GSI** specifically optimized for the **Samsung Galaxy A03 Core**.

The base system uses the **LineageOS GSI by Andy Yan**, with device-specific adjustments for better compatibility, stability and overall user experience.

---

## ⚠️ Disclaimer

You install this at your **own risk**.  
I am **not responsible** for any bootloops, data loss, soft-brick, or hardware damage.

If you are not comfortable modifying your device — **stop here.**

---

## 📃 Documentation

I am **not** the original developer of LineageOS or the GSI itself.  
I only maintain the **unofficial adaptation** specifically for the Samsung Galaxy A03 Core.

Updates may include:
- device-specific optimizations  
- kernel/vendor compatibility fixes  
- my optional AIO optimization module  
- stability improvements  

Special thanks to the developers credited below.

---

## 💾 Flashing Steps
*(To be added soon — if you want, I can write the full flashing guide for you.)*

---

## ⭐ Features

- Device-specific optimizations for the **Samsung Galaxy A03 Core**
- Removal of unnecessary components not used on this device
- Hidden Treble Settings for a clean **full ROM-like** experience  
  To enable Treble settings:
  ```bash
  su -c "pm enable me.phh.treble.app/.TopLevelSettingsActivity"
- **Offline charging automatically reboots the device**  
  *(prevents being stuck at the Samsung logo on some kernels)*

---

## ⛔ Known Issues

- **VoLTE is not supported**
- **Incorrect mobile signal bar display** (always shows 2 bars regardless of actual signal strength)

---

## 🔧 Future Fixes

The author may release a **Magisk module** in the future that could fix some of these issues.

---

## 📌 Sources

- **[LiteGApps](https://litegapps.github.io/)** — for lightweight GApps packages
- **[LineageOS](https://lineageos.org/)** — for their amazing open-source ROM
- **[Andy Yan](https://sourceforge.net/projects/andyyan-gsi/files/)** — for maintaining the LineageOS GSI
