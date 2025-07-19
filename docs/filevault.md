# FileVault with OpenCore Legacy Patcher

FileVault works out of the box with this repo's bundled EFI - you just have to enable it **AFTER** applying OCLP root patches, and disabling it whenever you're updating OS and need to reapply those patches again. However, I wanted to explain the steps I had to take to fix FileVault on OCLP, as nobody else on the internet had a guide.

---

## When to enable FileVault

Enable FileVault after you patch the system with OCLP. Otherwise, you'll be met with an error when trying to apply OCLP patches.

---

## FileVault Failed: Incorrect Password

When trying to activate FileVault after applying OCLP patches, you may encounter the following error, where FileVault fails due to an "Incorrect Password". Upon checking on the Console, you may see an error saying that FileVault failed to enable due to a broken ARV seal. 

Lucky for us, OCLP developers had figured this out, and added a kernel patch to OCLP EFIs that deals with this.

---

## How to fix FileVault on OCLP patched hackintoshes

Please note that you may be vulnerable to some password interception attacks if you decide to enable FileVault in a seal-less macOS. Do not run OCLP patches if you're on FBI's Most Wanted.

### 1. Add boot arguments

Add `-arv_allow_fv` to your boot arguments under NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 in your `config.plist` file.

### 2. Add kernel patches

Using OCAT, add a new entry on Kernel -> Patch on your `config.plist` file:

Identifier: `com.apple.filesystems.apfs`
Base: `_apfs_filevault_allowed`
Comment: `Force FileVault on Broken Seal`
Count: `0`
Enabled: `true`
Limit: `0`
Replace: `B801000000C3`
Skip: `0`
MinKernel: `20.4.0`
Arch: `x86_64`

Alternatively, add it manually using a text editor:

`
<dict> <key>Arch</key> <string>x86_64</string> <key>Base</key> <string>_apfs_filevault_allowed</string> <key>Comment</key> <string>Force FileVault on Broken Seal</string> <key>Count</key> <integer>0</integer> <key>Enabled</key> <true/> <key>Find</key> <data></data> <key>Identifier</key> <string>com.apple.filesystems.apfs</string> <key>Limit</key> <integer>0</integer> <key>Mask</key> <data></data> <key>MaxKernel</key> <string></string> <key>MinKernel</key> <string>20.4.0</string> <key>Replace</key> <data>uAEAAADD</data> <key>ReplaceMask</key> <data></data> <key>Skip</key> <integer>0</integer> </dict> 
`
