# Hackintosh EFI - Intel System with OpenCore

A comprehensive EFI configuration for running macOS on Intel-based systems using OpenCore bootloader. This configuration targets **MacPro7,1** SMBIOS and supports various Intel chipsets with multiple network adapter options.

## 🖥️ Hardware Compatibility

This EFI is designed for Intel systems with the following hardware support:

### **CPU Support**
- Intel processors with compatible chipsets
- CPU power management via [`CPUFriend.kext`](efi/oc/Kexts/CPUFriend.kext) v1.2.9
- Custom CPU frequency data via [`CPUFriendDataProvider.kext`](efi/oc/Kexts/CPUFriendDataProvider.kext) v1.0.0

### **Network Adapters**
- **Intel Ethernet**: [`IntelMausi.kext`](efi/oc/Kexts/IntelMausi.kext) v1.0.8
- **Atheros Ethernet**: [`AtherosE2200Ethernet.kext`](efi/oc/Kexts/AtherosE2200Ethernet.kext) v2.4.0
- **Realtek RTL8111**: [`RealtekRTL8111.kext`](efi/oc/Kexts/RealtekRTL8111.kext) v2.5.0
- **Intel I225-V/I226-V**: [`AppleIGC.kext`](efi/oc/Kexts/AppleIGC.kext) v1.5d1

### **Graphics Support**
- Intel integrated graphics via [`WhateverGreen.kext`](efi/oc/Kexts/WhateverGreen.kext) v1.6.9
- AMD graphics with `agdpmod=pikera` boot argument

### **Audio Support**
- Native audio via [`AppleALC.kext`](efi/oc/Kexts/AppleALC.kext) v1.9.4
- Layout ID: **1** (configured via `alcid=1` boot argument)

## 🚀 OpenCore Information

- **OpenCore Version**: Latest stable release
- **Target SMBIOS**: MacPro7,1
- **OpenCore GUI**: Enabled with GoldenGate theme
- **Boot Picker**: External mode with 5-second timeout

### **Key Kexts**
| Kext | Version | Purpose |
|------|---------|---------|
| [`Lilu.kext`](efi/oc/Kexts/Lilu.kext) | v1.7.0 | Arbitrary kext and process patching |
| [`VirtualSMC.kext`](efi/oc/Kexts/VirtualSMC.kext) | v1.3.6 | SMC emulation |
| [`WhateverGreen.kext`](efi/oc/Kexts/WhateverGreen.kext) | v1.6.9 | Graphics patching |
| [`AppleALC.kext`](efi/oc/Kexts/AppleALC.kext) | v1.9.4 | Native audio support |
| [`RestrictEvents.kext`](efi/oc/Kexts/RestrictEvents.kext) | v1.1.5 | Blocks unwanted processes |
| [`BlueToolFixup.kext`](efi/oc/Kexts/BlueToolFixup.kext) | v2.7.0 | Bluetooth functionality |
| [`USBInjectAll.kext`](efi/oc/Kexts/USBInjectAll.kext) | v0.8.1 | USB port injection |
| [`XHCI-unsupported.kext`](efi/oc/Kexts/XHCI-unsupported.kext) | v0.9.2 | USB 3.0 support for unsupported controllers |

## ⚡ Boot Arguments

The following boot arguments are configured in [`config.plist`](efi/oc/config.plist:726):

```
alcid=1 watchdog=0 agdpmod=pikera dk.e1000=0 e1000=0
```

### **Boot Arguments Explanation**
- `alcid=1` - Sets audio layout ID to 1 for [`AppleALC.kext`](efi/oc/Kexts/AppleALC.kext)
- `watchdog=0` - Disables watchdog timer to prevent kernel panics
- `agdpmod=pikera` - Fixes AMD GPU acceleration and connection issues
- `dk.e1000=0` - Disables [`DriverKit`](efi/oc/config.plist:726) variant of Intel I225-V driver
- `e1000=0` - Disables legacy Intel ethernet driver to use [`AppleIGC.kext`](efi/oc/Kexts/AppleIGC.kext)

## 🛠️ Installation Requirements

### **Prerequisites**
- Intel-based system compatible with macOS
- UEFI firmware with Secure Boot disabled
- 8GB+ USB drive for macOS installer
- Access to a Mac or existing Hackintosh for installer creation

### **BIOS Settings**
- **Disable**: Secure Boot, Fast Boot, VT-d (unless using DisableIoMapper)
- **Enable**: VT-x, Above 4G Decoding, Hyper-Threading, EHCI/XHCI Hand-off

## 📋 What's Working

✅ **Fully Functional**
- macOS boot and installation
- Intel/AMD graphics acceleration
- Audio (all outputs/inputs)
- Ethernet (Intel, Atheros, Realtek)
- USB 2.0/3.0 ports
- Sleep/wake functionality
- CPU power management
- Bluetooth connectivity
- NVRAM functionality

## ❌ What's Not Working

❌ **Known Issues**
- WiFi (requires compatible card or USB adapter)
- DRM content in some applications
- Thunderbolt hot-plug (system dependent)
- Some USB-C functionality (varies by hardware)

## ⚠️ Important Notes

### **SMBIOS Generation Required**
Before using this EFI, you **MUST** generate proper SMBIOS data:

1. Download [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
2. Generate MacPro7,1 SMBIOS
3. Replace placeholder values in [`config.plist`](efi/oc/config.plist:769):
   - `SystemSerialNumber`: Replace `REPLACEME_SERIAL`
   - `MLB`: Replace `REPLACEME_MLB`  
   - `SystemUUID`: Replace `REPLACEME-UUID-REPLACE-WITH-GENERIC`
   - `ROM`: Replace with your network MAC address

### **Network Kext Selection**
Multiple network kexts are included. Disable unused ones in [`config.plist`](efi/oc/config.plist:198) based on your hardware:
- Keep only the kext matching your network controller
- Disable others by setting `Enabled` to `false`

## 🔗 Quick Start

1. **Download**: Clone or download this repository
2. **Generate SMBIOS**: Use GenSMBIOS for MacPro7,1
3. **Configure**: Update [`config.plist`](efi/oc/config.plist:1) with your SMBIOS data
4. **Install**: Copy EFI folder to your USB installer's EFI partition
5. **Boot**: Start installation process

For detailed installation instructions, see [`INSTALLATION.md`](docs/INSTALLATION.md).

For hardware compatibility details, see [`HARDWARE.md`](docs/HARDWARE.md).

## 📚 Credits

- **OpenCore Team** - [OpenCore](https://github.com/acidanthera/OpenCorePkg)
- **Acidanthera** - Kext development and maintenance
- **Dortania** - [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- **Olarila Community** - ACPI patches and configurations

## ⚖️ Disclaimer

This EFI configuration is provided as-is for educational purposes. Installing macOS on non-Apple hardware may violate Apple's Software License Agreement. Users assume all responsibility for any consequences of using this configuration. Always backup your data and understand the risks before proceeding.

## 📄 License

This project is released under the MIT License. See individual kext licenses for their respective terms.

---

**Need Help?** Check the installation guide, hardware compatibility list, or consult the Hackintosh community forums for support.