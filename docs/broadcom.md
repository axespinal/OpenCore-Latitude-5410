# Broadcom Wireless

This repo is configured to work out of the box with a BCM94352Z card, which is not a native Apple card.

---

## BrcmPatchRAM 2.7.0

Acidanthera removed some patches in `BrcmPatchRAM` 2.7.0 to "[improve] performance on macOS 15+". Turns out that, for my Dell DW1560 card, you now need to add `-btlfxboardid` to the boot arguments for the BT to work. I added it to this repo, but you may not need it for your own configuration.

---

## macOS Ventura 13 and older

You don't need OCLP root patching. Please follow these steps: [I don't need root patching](./docs/amfi.md)

---

## macOS Sonoma 14 and newer

### Non-native cards

If you're running a non-native Broadcom card on macOS Sonoma 14 and above, this repo works out of the box for you. All you need to do is download [OCLP](https://dortania.github.io/OpenCore-Legacy-Patcher/), set the root patches before you do anything else on the system, and you're good to go.

### Native cards

You don't need `AirportBrcmFixup` if you have an Apple AirPort and Fenvi Card with a Device ID (14E4:43ba, 14e4:43a3, or 14e4:43a0). Please remove `AirportBrcmFixup.kext` from your EFI.

