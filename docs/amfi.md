# Root patching

You don't need root patching if:

    1. You're running macOS Ventura 13 or older, as both Broadcom and Intel cards work without the need of OCLP.

    2. You're running an Intel card, and:

        a. You're fine with using `itlwm` and HeliPort on any macOS version.

        b. You're running macOS Sonoma 14 or older.

    3. You're only using wired connection, with no wireless support.

You may not need root patching if you're using a third party USB dongle to get wireless support. Please search online for your specific adapter and determine for yourself if you need root patching support or not.

---

## I need root patching

This repo comes with root patch support by default. You don't need to do anything here.

---

## I don't need root patching

Good for you. There's a handful of things you should do in order to tweak in this repo's `config.plist` before you're able to run macOS with all the bundled security measures:

### 1. IOSkywalkFamily.kext

macOS Sonoma 14 and later requires an older `IOSkywalkFamily.kext` in order to work with Broadcom chipsets. You have to remove this kext from the EFI/OC/Kexts folder, and from the EFI/OC/config.plist file under Kernel -> Add. 

You also need to remove the block line from the Kernel -> Block section of the `config.plist` file to allow Apple's native `IOSkywalkFamily.kext` to load.

### 2. AMFIPass.kext

You can restore full AMFI functionality by removing `AMFIPass.kext` from the EFI/OC/Kexts folder and from the EFI/OC/config.plist file under Kernel -> Add.

### 3. Secure Boot Model

You can also set the `SecureBootModel` to `Default` under Misc -> Security in the `config.plist` file.

### 4. Restore System Integrity Protection (SIP)

Under NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 on the `config.plist` file, set `csr-active-config` to `030A0000`.

### 5. Remove Broken Seal FileVault patches

Remove the `Force FileVault on Broken Seal` patch from Kernel -> Patch on the `config.plist` file.

Remove the boot argument `-arv_allow_fv` from NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 on the `config.plist` file.
