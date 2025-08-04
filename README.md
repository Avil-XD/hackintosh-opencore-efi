
# Hackintosh EFI - Intel System with OpenCore

A comprehensive EFI configuration for running macOS on Intel-based systems using OpenCore bootloader. This configuration targets **MacPro7,1** SMBIOS and supports various Intel chipsets with multiple network adapter options.

---

## �🖥️ Real-World System Example

This EFI is **currently running** on the following hardware:

- **Motherboard**: Gigabyte B360M Gaming HD
- **CPU**: Intel Core i7-8700K (Coffee Lake, 6 cores/12 threads)
- **RAM**: 16GB DDR4
- **Storage**: 1TB SSD (macOS) + 3TB HDD (data)
- **GPU**: Gigabyte AMD Radeon RX560 OC 4GB
- **WiFi**: TP-Link USB WiFi Adapter (for internet access)  
   - Driver: [Wireless-USB-Big-Sur-Adapter by chris1111](https://github.com/chris1111/Wireless-USB-Big-Sur-Adapter)
- **Ethernet**: Inbuilt Realtek controller (native, via RealtekRTL8111.kext)

Below is a screenshot of the system, showing the system specs as detected by macOS:

![About This Mac Screenshot](Screenshot%202025-06-25%20at%209.39.34%E2%80%AFPM.png)

---

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

## 🚦 What's Not Working

**Current Limitations**
- WiFi: Requires a compatible card or USB adapter. See the [Dortania Wireless Buyers Guide](https://dortania.github.io/Wireless-Buyers-Guide/) for recommended options.
- DRM content may not work in some applications
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

## 🖥️ Hardware Compatibility (Full Matrix)

**Target SMBIOS**: MacPro7,1
**OpenCore Version**: Latest stable
**macOS Compatibility**: Big Sur, Monterey, Ventura, Sonoma
**Architecture**: Intel x86_64 only

### CPU Compatibility

| Generation | Architecture | Codename | Examples | Status |
|------------|-------------|----------|----------|--------|
| 6th Gen | Skylake | LGA1151 | i5-6600K, i7-6700K | ✅ Supported |
| 7th Gen | Kaby Lake | LGA1151 | i5-7600K, i7-7700K | ✅ Supported |
| 8th Gen | Coffee Lake | LGA1151 | i5-8400, i7-8700K | ✅ Supported |
| 9th Gen | Coffee Lake-R | LGA1151 | i5-9600K, i7-9700K | ✅ Supported |
| 10th Gen | Comet Lake | LGA1200 | i5-10400, i7-10700K | ✅ Supported |
| 11th Gen | Rocket Lake | LGA1200 | i5-11400, i7-11700K | ✅ Supported |
| 12th Gen | Alder Lake | LGA1700 | i5-12400, i7-12700K | ⚠️ Partial |

**CPU Power Management**: [`CPUFriend.kext`](efi/oc/Kexts/CPUFriend.kext) v1.2.9, [`CPUFriendDataProvider.kext`](efi/oc/Kexts/CPUFriendDataProvider.kext) v1.0.0

---

### Motherboard Chipsets

| Chipset | Platform | LGA Socket | Status | Notes |
|---------|----------|------------|--------|-------|
| Z170/H170 | Skylake | LGA1151 | ✅ Full | Stable support |
| Z270/H270 | Kaby Lake | LGA1151 | ✅ Full | Stable support |
| Z370/H370 | Coffee Lake | LGA1151 | ✅ Full | Stable support |
| Z390/H370 | Coffee Lake-R | LGA1151 | ✅ Full | Recommended |
| Z490/H470 | Comet Lake | LGA1200 | ✅ Full | Good support |
| Z590/H570 | Rocket Lake | LGA1200 | ✅ Full | Good support |
| Z690/H670 | Alder Lake | LGA1700 | ⚠️ Partial | Requires tuning |

**Note:** B360/H310 chipsets may require extra patches (this EFI is tested on B360M).

---

### Network Adapters

| Controller | Kext | Version | Status | Notes |
|------------|------|---------|--------|-------|
| I219-V/LM | [`IntelMausi.kext`](efi/oc/Kexts/IntelMausi.kext) | v1.0.8 | ✅ Native | Most common |
| I225-V/I226-V | [`AppleIGC.kext`](efi/oc/Kexts/AppleIGC.kext) | v1.5d1 | ✅ Native | 2.5Gb Ethernet |
| I210/I211 | [`IntelMausi.kext`](efi/oc/Kexts/IntelMausi.kext) | v1.0.8 | ✅ Native | Server grade |
| RTL8111 | [`RealtekRTL8111.kext`](efi/oc/Kexts/RealtekRTL8111.kext) | v2.5.0 | ✅ Full | Very common |
| AR8161/62/71 | [`AtherosE2200Ethernet.kext`](efi/oc/Kexts/AtherosE2200Ethernet.kext) | v2.4.0 | ✅ Full | Older boards |

**WiFi:** Not included by default. Use compatible Broadcom or Intel cards (with itlwm) or USB adapters (like TP-Link).

---

### Graphics Support

**Intel iGPU:** HD 530/630, UHD 630/750/770 (see WhateverGreen)

**AMD dGPU:** RX 460/470/480/560/570/580, Vega, Navi (5000/6000 series) — `agdpmod=pikera` required for AMD

**NVIDIA:** Not supported on modern macOS

---

### Audio Support

- [`AppleALC.kext`](efi/oc/Kexts/AppleALC.kext) v1.9.4, layout ID 1 (`alcid=1`)
- Most Realtek codecs supported (ALC887, ALC892, ALC1200, ALC1220, etc.)

---

### USB, Bluetooth, Storage, Memory

- **USB**: [`USBInjectAll.kext`](efi/oc/Kexts/USBInjectAll.kext), [`XHCI-unsupported.kext`](efi/oc/Kexts/XHCI-unsupported.kext), custom mapping recommended
- **Bluetooth**: [`BlueToolFixup.kext`](efi/oc/Kexts/BlueToolFixup.kext), works with Intel/Broadcom
- **Storage**: SATA (AHCI), NVMe supported
- **Memory**: DDR4/DDR5, XMP/DOCP

---

### Hardware Limitations

- **Unsupported**: AMD CPUs, NVIDIA GPUs, Intel WiFi (without extra kexts), Thunderbolt (varies), RAID
- **Problematic**: Some B360/H310, Z690/Z790, integrated WiFi, some USB controllers

---

### Hardware Detection & Verification

Use System Information (About This Mac) to verify CPU, RAM, GPU, storage, and network. For command-line:

```bash
sysctl -n machdep.cpu.brand_string
system_profiler SPDisplaysDataType
system_profiler SPAudioDataType
system_profiler SPNetworkDataType
system_profiler SPUSBDataType
```

---

## 🔗 Quick Start

1. **Download**: Clone or download this repository
2. **Generate SMBIOS**: Use GenSMBIOS for MacPro7,1
3. **Configure**: Update [`config.plist`](efi/oc/config.plist:1) with your SMBIOS data
4. **Install**: Copy EFI folder to your USB installer's EFI partition
5. **Boot**: Start installation process


---

## 🛠️ Full Installation Guide

### Prerequisites

#### **Hardware Requirements**
- Intel CPU (6th generation or newer recommended)
- Compatible Intel/AMD chipset
- 8GB+ RAM (16GB+ recommended)
- 120GB+ storage device (SSD recommended)
- UEFI motherboard with CSM disabled
- Supported network adapter (see hardware compatibility below)

#### **Software Requirements**
- 8GB+ USB drive
- Access to a Mac or existing Hackintosh
- [macOS Installer](https://apps.apple.com/us/app/macos-big-sur/id1526878132) (Big Sur or newer)
- [OpenCore](https://github.com/acidanthera/OpenCorePkg/releases) (if updating)
- [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- [ProperTree](https://github.com/corpnewt/ProperTree) (for [`config.plist`](efi/oc/config.plist) editing)

#### **BIOS/UEFI Settings**

**Disable:** Secure Boot, Fast Boot, CSM/Legacy Boot, Intel VT-d (or enable `DisableIoMapper` in config), Intel SGX, Intel Platform Trust Technology, CFG Lock (if available, or use `AppleXcpmCfgLock` quirk)

**Enable:** VT-x, Above 4G Decoding, Hyper-Threading, EHCI/XHCI Hand-off, OS Type: Windows 10 WHQL (if available), SATA Mode: AHCI

---

### Step 1: Create macOS Installer

**On macOS:**
1. Download macOS installer from App Store or use gibMacOS.
2. Format USB drive using Disk Utility.
3. Create installer with `createinstallmedia`.
4. Mount EFI partition: `diskutil list` and `sudo diskutil mount diskXs1`.

**On Windows:**
1. Use gibMacOS to download macOS.
2. Use MakeInstall to create installer.
3. Mount EFI partition using MountEFI.

---

### Step 2: Configure EFI

1. Copy this repository's EFI folder to your USB's EFI partition.
2. Verify structure:
   ```
   EFI/
   ├── BOOT/
   │   └── BOOTx64.efi
   └── OC/
       ├── OpenCore.efi
       ├── config.plist
       ├── ACPI/
       ├── Drivers/
       ├── Kexts/
       └── Resources/
   ```
3. Generate SMBIOS data with GenSMBIOS (choose MacPro7,1) and update `config.plist`:
   - Replace `REPLACEME_SERIAL`, `REPLACEME_MLB`, `REPLACEME-UUID-REPLACE-WITH-GENERIC`, and `ROM` with your values.
4. Set ROM value to your MAC address (Base64 or plain).
5. Enable only the network kext matching your hardware in `config.plist` (disable others).

---

### Step 3: Installation Process

1. Boot from USB installer (select UEFI: [USB Name]).
2. In OpenCore boot menu, select macOS installer.
3. Use Disk Utility to format your target drive (APFS, GUID Partition Map).
4. Install macOS (system will reboot several times; always boot from USB until setup is complete).

---

### Step 4: Post-Installation

1. Mount EFI partition of your macOS drive and copy EFI from USB.
2. Test booting without USB.
3. Complete macOS setup assistant.
4. Verify hardware: About This Mac, System Information, test Ethernet, audio, USB, graphics, sleep/wake.

---

### Post-Installation Tweaks

- **Audio**: If no sound, try different `alcid` values in boot args. See [AppleALC wiki](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs).
- **USB**: Use USBMap to create a custom map for your board, replace USBInjectAll.kext, and disable XhciPortLimit.
- **CPU Power**: Generate CPUFriendDataProvider for your CPU and replace the kext.

---

### Troubleshooting

| Issue | Solution |
|-------|----------|
| Stuck at Apple Logo | Add `-v` to boot args |
| Kernel Panic | Check hardware/kexts |
| No boot picker | Set `ShowPicker` to `true` in config.plist |
| Graphics issues | Check WhateverGreen, boot args |
| USB not working | Enable XhciPortLimit, check XHCI-unsupported |
| No audio | Try different alcid values |
| No Ethernet | Enable correct kext, disable others |
| Sleep issues | Check power/USB config |
| Graphics accel issues | Check WhateverGreen config |

---

**Target SMBIOS**: MacPro7,1  
**OpenCore Version**: Latest stable  
**macOS Compatibility**: Big Sur, Monterey, Ventura, Sonoma  
**Architecture**: Intel x86_64 only

### CPU Compatibility

| Generation | Architecture | Codename | Examples | Status |
|------------|-------------|----------|----------|--------|
| 6th Gen | Skylake | LGA1151 | i5-6600K, i7-6700K | ✅ Supported |
| 7th Gen | Kaby Lake | LGA1151 | i5-7600K, i7-7700K | ✅ Supported |
| 8th Gen | Coffee Lake | LGA1151 | i5-8400, i7-8700K | ✅ Supported |
| 9th Gen | Coffee Lake-R | LGA1151 | i5-9600K, i7-9700K | ✅ Supported |
| 10th Gen | Comet Lake | LGA1200 | i5-10400, i7-10700K | ✅ Supported |
| 11th Gen | Rocket Lake | LGA1200 | i5-11400, i7-11700K | ✅ Supported |
| 12th Gen | Alder Lake | LGA1700 | i5-12400, i7-12700K | ⚠️ Partial |

**CPU Power Management**: [`CPUFriend.kext`](efi/oc/Kexts/CPUFriend.kext) v1.2.9, [`CPUFriendDataProvider.kext`](efi/oc/Kexts/CPUFriendDataProvider.kext) v1.0.0

---

### Motherboard Chipsets

| Chipset | Platform | LGA Socket | Status | Notes |
|---------|----------|------------|--------|-------|
| Z170/H170 | Skylake | LGA1151 | ✅ Full | Stable support |
| Z270/H270 | Kaby Lake | LGA1151 | ✅ Full | Stable support |
| Z370/H370 | Coffee Lake | LGA1151 | ✅ Full | Stable support |
| Z390/H370 | Coffee Lake-R | LGA1151 | ✅ Full | Recommended |
| Z490/H470 | Comet Lake | LGA1200 | ✅ Full | Good support |
| Z590/H570 | Rocket Lake | LGA1200 | ✅ Full | Good support |
| Z690/H670 | Alder Lake | LGA1700 | ⚠️ Partial | Requires tuning |

**Note:** B360/H310 chipsets may require extra patches (this EFI is tested on B360M).

---

### Network Adapters

| Controller | Kext | Version | Status | Notes |
|------------|------|---------|--------|-------|
| I219-V/LM | [`IntelMausi.kext`](efi/oc/Kexts/IntelMausi.kext) | v1.0.8 | ✅ Native | Most common |
| I225-V/I226-V | [`AppleIGC.kext`](efi/oc/Kexts/AppleIGC.kext) | v1.5d1 | ✅ Native | 2.5Gb Ethernet |
| I210/I211 | [`IntelMausi.kext`](efi/oc/Kexts/IntelMausi.kext) | v1.0.8 | ✅ Native | Server grade |
| RTL8111 | [`RealtekRTL8111.kext`](efi/oc/Kexts/RealtekRTL8111.kext) | v2.5.0 | ✅ Full | Very common |
| AR8161/62/71 | [`AtherosE2200Ethernet.kext`](efi/oc/Kexts/AtherosE2200Ethernet.kext) | v2.4.0 | ✅ Full | Older boards |

**WiFi:** Not included by default. Use compatible Broadcom or Intel cards (with itlwm) or USB adapters (like TP-Link).

---

### Graphics Support

**Intel iGPU:** HD 530/630, UHD 630/750/770 (see WhateverGreen)

**AMD dGPU:** RX 460/470/480/560/570/580, Vega, Navi (5000/6000 series) — `agdpmod=pikera` required for AMD

**NVIDIA:** Not supported on modern macOS

---

### Audio Support

- [`AppleALC.kext`](efi/oc/Kexts/AppleALC.kext) v1.9.4, layout ID 1 (`alcid=1`)
- Most Realtek codecs supported (ALC887, ALC892, ALC1200, ALC1220, etc.)

---

### USB, Bluetooth, Storage, Memory

- **USB**: [`USBInjectAll.kext`](efi/oc/Kexts/USBInjectAll.kext), [`XHCI-unsupported.kext`](efi/oc/Kexts/XHCI-unsupported.kext), custom mapping recommended
- **Bluetooth**: [`BlueToolFixup.kext`](efi/oc/Kexts/BlueToolFixup.kext), works with Intel/Broadcom
- **Storage**: SATA (AHCI), NVMe supported
- **Memory**: DDR4/DDR5, XMP/DOCP

---

### Hardware Limitations

- **Unsupported**: AMD CPUs, NVIDIA GPUs, Intel WiFi (without extra kexts), Thunderbolt (varies), RAID
- **Problematic**: Some B360/H310, Z690/Z790, integrated WiFi, some USB controllers

---

### Hardware Detection & Verification

Use System Information (About This Mac) to verify CPU, RAM, GPU, storage, and network. For command-line:

```bash
sysctl -n machdep.cpu.brand_string
system_profiler SPDisplaysDataType
system_profiler SPAudioDataType
system_profiler SPNetworkDataType
system_profiler SPUSBDataType
```

---

## 📞 Getting Help

- [Dortania OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [r/hackintosh subreddit](https://www.reddit.com/r/hackintosh/)
- [tonymacx86 forums](https://www.tonymacx86.com/)
- [OpenCore documentation](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs)

Before asking for help, check this README, verify your hardware, and provide full specs, boot args, kext list, and error logs.

---

## 📚 Credits

- **OpenCore Team** - [OpenCore](https://github.com/acidanthera/OpenCorePkg)
- **Acidanthera** - Kext development and maintenance
- **Dortania** - [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- **Olarila Community** - ACPI patches and configurations

---

## ⚖️ Disclaimer

This EFI configuration is provided as-is for educational purposes. Installing macOS on non-Apple hardware may violate Apple's Software License Agreement. Users assume all responsibility for any consequences of using this configuration. Always backup your data and understand the risks before proceeding.

## 📄 License

This project is released under the MIT License. See individual kext licenses for their respective terms.

---

**Need Help?** Check the installation guide, hardware compatibility list, or consult the Hackintosh community forums for support.