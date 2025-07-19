# OpenCore EFI for Dell Latitude 5410

This is my personal OpenCore EFI configuration for running macOS Sequoia 15.5 on the **Dell Latitude 5410**.

I may update the repo from time to time, as this is my daily driver. If you have any concerns or suggestions, please feel free to submit a commit or open an issue.

---

## Notes

- **PLEASE** remember to **generate a new serial** before running. The one bundled with this EFI is a placeholder - ***iServices won't work*** until you generate new serials.
- **IF** you're running macOS 14+, **PLEASE** set up [OpenCore Legacy Patcher](https://dortania.github.io/OpenCore-Legacy-Patcher/) before starting using the system. **NETWORKING WON'T WORK WITHOUT IT.** Read more: [Broadcom Wireless](./docs/broadcom.md)
- **IF** you're running macOS 12 and older _and_ have a Broadcom card, please read: [Broadcom Wireless](./docs/broadcom.md)
- **IF** you're using an Intel wireless card, please read: [Intel Wireless](./docs/itlwm.md).
- **PLEASE** remember to configure `CfgLock` using [Mod GRUB Shell](https://github.com/datasone/grub-mod-setup_var/releases).
  I don't remember the exact flags I had to use, but it's better if you check by decompiling your specific BIOS using [this procedure](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html) from Dortania.
- **PLEASE** read the rest of the README carefully. If you have any questions, either mail me or open an issue.
- For best stability, ensure NVRAM is reset after EFI update.
- Keep a backup EFI on a USB drive in case of failure.
- If you dual-boot with Windows, remember to set the system to use UTC rather than the default `localtime`. To do this, run the following command in a Command Prompt (`cmd.exe`) with Administrator privileges:

  ```cmd
  reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_DWORD /f

---

## Hardware Specifications

| Component        | Specification                                      |
|------------------|----------------------------------------------------|
| **CPU**         | Intel Core i7-10610U                                |
| **iGPU**        | Intel UHD Graphics 620                              |
| **RAM**         | 16GB DDR4-2666 SODIMM                               |
| **Storage**     | TeamGroup MP33 NVMe 512GB SSD                       |
| **WiFi / BT**   | Dell DW1560 (BCM94352Z) WiFi + BT M.2 Card          |
| **Ethernet**    | Intel I219LM                                        |
| **Display**     | 1920x1080 IPS Screen                                |
| **Audio**       | Realtek ALC3204 (ALC236)                            |
| **Touchpad**    | Cirque/Alps HID (`DELL09A0` - `0488:121F`).         |

---

## Software Specifications

- **OpenCore**: 1.0.4 RELEASE  
- **macOS Version**: Sequoia 15.5  
- **SMBIOS**: `MacBookPro16,3` (Intel Core i5-8257U @ 1.40GHz)  
- **CPU Power Management**: Configured using `CPUFriend` and `CPUFriendFriend` with MacBook Air power profile  

---

## What's Working

- ✅ Wi-Fi (with `AirportBrcmFixup` and OpenCore Legacy Patcher)
- ✅ Ethernet (using `IntelMausi`)
- ✅ Bluetooth (using `BrcmPatchRAM` 2.7.0, read more: [Broadcom Wireless](./docs/broadcom.md))
- ✅ FaceTime and iMessage
- ✅ DisplayPort / USB-C
- ✅ HDMI
- ✅ USB Ports
- ✅ Camera
- ✅ Microphone
- ✅ SD Card Reader
- ✅ Battery Status
- ✅ Brightness Keys
- ✅ Sleep

---

## Partially Working

- ⚠️ **Touchpad**  


  Works using GPIO interrupt (`0x6C`) after significant effort. However, may behave erratically in a long session. Getting the touchpad working was the biggest challenge in this build, and I will keep trying to get it to work better in future updates of this EFI.

  For best results, set click to "Firm" in Settings > Touchpad. Otherwise you may have false long presses.  Tap to Click should work, but I prefer to leave it disabled to avoid false presses whenever the touchpad freaks out.

  - ***Temporary workaround***: closing and opening the lid.

- ⚠️ **AirPods Pro 2** *(as tested so far, might include more Apple headsets)*  

  Audio may disconnect sometimes. It happened annoyingly often when using `BrcmPatchRAM` 2.6.9, but seems to work better on 2.7.0. Also, call quality sucks terribly due to HW limitations - I advise you to use the laptop's included microphone, or any other audio input. Otherwise, it works just as any AirPods would on a Mac, with all features working.

  - **Temporary workaround**: If audio doesn't come back, disconnect and connect again.
 
- ⚠️ **FileVault**

  Works proper, but you have to setup OCLP root patches before you enable it. If you enable it, you need to disable it to update the system. Please read [FileVault with OCLP](./docs/filevault.md) for more details.
    
- ⚠️ **AirPort ecosystem features** *(Handoff, Continuity, AirDrop, Apple Watch unlock)*

  
  Some features work partially with the Dell DW1560 card.
  - AirDrop is able to receive files, but can't send them.
  - Handoff works so far.
  - Apple Watch unlock sets up, but doesn't work in practice.
 

---

## Not Working

- ❌ **Hibernation**  

  I've heard of people getting it working with a kext, haven't tried it myself though.

- ❌ **Fingerprint Reader**  

  Touch ID relies on proprietary Apple hardware, can't be adapted to work with non-Apple fingerprint readers.
  
---

> Tested and maintained for educational purposes. Use at your own risk.
