# OpenCore EFI for Dell Latitude 5410

This is my personal OpenCore EFI configuration for running macOS Sonoma 14.7.6 on the **Dell Latitude 5410**.

I may update the repo from time to time, as this is my daily driver. If you have any concerns or suggestions, please feel free to submit a commit or open an issue.

I'm currently running macOS Sonoma 14.7.6, and don't plan on updating it until AirportItlwm gets updated to work on Sequoia with SIP enabled. However, you can easily run Sequoia with this EFI by swapping `AirportItlwm` with `itlwm`, or running OCLP patches to get the Wi-Fi working.

---

## Notes

- **PLEASE** remember to **generate a new serial** before running. The one bundled with this EFI is a placeholder - ***iServices won't work*** until you generate new serials.
- **PLEASE** remember to configure CfgLock using [Mod GRUB Shell](https://github.com/datasone/grub-mod-setup_var/releases). I don't remember the exact flags I had to use, but it's better if you check by decompiling your specific BIOS using [this procedure](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html) from Dortania.
- **PLEASE** read the rest of the README carefully. If you have any questions, either mail me or open an issue.
- For best stability, ensure NVRAM is reset after EFI update.
- Always match the `AirportItlwm.kext` version with your macOS version.
- Keep a backup EFI on a USB drive in case of failure.
- If you dual-boot with Windows, remember to set the system to use UTC rather than the default `localtime`. To do this, run the following command in a Command Prompt (`cmd.exe`) with Administrator privileges:

  ```cmd
  reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_DWORD /f

---

## Hardware Specifications

| Component        | Specification                                      |
|------------------|----------------------------------------------------|
| **CPU**         | Intel Core i7-10610U (4 Cores / 8 Threads @ 1.80 GHz) |
| **iGPU**        | Intel UHD Graphics 620                              |
| **RAM**         | 16GB DDR4-2666 SODIMM                               |
| **Storage**     | TeamGroup MP33 NVMe 512GB SSD                       |
| **WiFi / BT**   | Intel AX210 WiFi 6 + Bluetooth                      |
| **Ethernet**    | Intel I219LM                                        |
| **Display**     | 1920x1080 IPS Screen                                |
| **Audio**       | Realtek ALC3204 (ALC236)                            |
| **Touchpad**    | Cirque/Alps HID (`DELL09A0` - `0488:121F`) GPIO: `0x6C` |

---

## Software Specifications

- **OpenCore**: 1.0.4 RELEASE  
- **macOS Version**: Sonoma 14.7.6  
- **SMBIOS**: `MacBookPro16,3` (Intel Core i5-8257U @ 1.40GHz)  
- **CPU Power Management**: Configured using `CPUFriend` and `CPUFriendFriend` with MacBook Air power profile  

---

## What's Working

- ✅ Wi-Fi (with `AirportItlwm` for macOS 14.4+)
- ✅ Ethernet
- ✅ Bluetooth
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
  Works using GPIO interrupt (`0x6C`) after significant effort. However, may behave erratically at times. Getting the touchpad working was the biggest challenge in this build.  
  - ***Temporary workaround***: closing and opening the lid.

    

- ⚠️ **FaceTime & iMessage**  
  After macOS 14.4, `AirportItlwm` causes activation issues.  
  - ***Temporary workaround***: activate using `Itlwm`, then switch to `AirportItlwm`. Mileage may vary.

- ⚠️ **AirPods Pro 2** *(as tested so far, might include more Apple headsets)*  
  Doesn't work due to `IntelBluetoothFirmware` incompatibilities. Some Apple apps (e.g., Safari) output audio; appears and functions within system settings, but overall experience is unreliable. You can replace the stock Intel card with a supported Broadcom card to fix this issue.
  - ***Temporary workaround***: start playing audio through Safari, immediately play audio in the other desired app. If audio stops or pauses, repeat the process.

---

## Not Working

- ❌ **Hibernation**  

  I've heard of people getting it working with a kext, haven't tried it myself though.

- ❌ **Fingerprint Reader**  

  Touch ID relies on proprietary Apple hardware, can't be adapted to work with non-Apple fingerprint readers.

- ❌ **AirPort ecosystem features** *(Handoff, Continuity, AirDrop, Apple Watch unlock)*

  Do not work due to `AirportItlwm` limitations. You can replace the stock Intel card with a supported Broadcom card to gain those functionalities.

---

## Credits

- [OpCore-Simplify by lzhoang2801](https://github.com/lzhoang2801/OpCore-Simplify) – Clean EFI folder structure and helpful utilities  
- [OpenCore Project](https://github.com/acidanthera/OpenCorePkg) – The heart of this Hackintosh  
- [VoodooI2C Project & Wiki](https://voodooi2c.github.io/#Downloads) – Crucial for Alps/Cirque I2C touchpad support and GPIO debugging  

---

> Tested and maintained for educational purposes. Use at your own risk.
