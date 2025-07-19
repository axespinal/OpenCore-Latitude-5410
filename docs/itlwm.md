# Intel Wireless

This repo can be compatible with most, if not all Intel wireless cards. However, this repo is set up to work with Broadcom cards. To adapt this repo to be compatible with Intel cards, please refer to the guide below.

---

## For all versions

Remove the following kexts: `AirportBrcmFixup.kext`, `BrcmFirmwareData.kext`, `BrcmPatchRAM3.kext`.

---

## macOS Sonoma 14 and older

You don't need root patching. You can reenable macOS security measures using this guide: [I don't need root patching](./amfi.md). After you're done, return here and follow the steps below.

---

## macOS Sequoia 15 and newer

You have two options to have working Intel wireless on Sequoia:

### itlwm and HeliPort

You can use `itlwm` and `HeliPort` to have a working Wi-Fi in Sequoia without having to disable macOS's security measures nor having to perform OCLP's root patches. A good option if you're OK with HeliPort's limitations - just make sure to remove all the AMFI/SIP patches. (read more: [I don't need root patching](./amfi.md))

### AirportItlwm

You can also use `AirportItlwm`, but you'd need OCLP root patching for it to work. 

### IntelBluetoothFirmware

For Bluetooth to work, add `IntelBluetoothFirmware`. It should work in tandem with `BlueToolFixup`, which is bundled with this repo. Please update it if it's not the latest version, to avoid any incompatibilities.
