# Hardware Compatibility Guide

This document provides detailed hardware compatibility information for this Hackintosh EFI configuration. The setup is optimized for Intel-based systems with specific network and graphics configurations.

## 🖥️ System Overview

- **Target SMBIOS**: MacPro7,1
- **OpenCore Version**: Latest stable
- **macOS Compatibility**: Big Sur, Monterey, Ventura, Sonoma
- **Architecture**: Intel x86_64 only

## 🔧 CPU Compatibility

### **Supported Intel CPU Families**

| Generation | Architecture | Codename | Examples | Status |
|------------|-------------|----------|----------|--------|
| **6th Gen** | Skylake | LGA1151 | i5-6600K, i7-6700K | ✅ Supported |
| **7th Gen** | Kaby Lake | LGA1151 | i5-7600K, i7-7700K | ✅ Supported |
| **8th Gen** | Coffee Lake | LGA1151 | i5-8400, i7-8700K | ✅ Supported |
| **9th Gen** | Coffee Lake-R | LGA1151 | i5-9600K, i7-9700K | ✅ Supported |
| **10th Gen** | Comet Lake | LGA1200 | i5-10400, i7-10700K | ✅ Supported |
| **11th Gen** | Rocket Lake | LGA1200 | i5-11400, i7-11700K | ✅ Supported |
| **12th Gen** | Alder Lake | LGA1700 | i5-12400, i7-12700K | ⚠️ Partial |

### **CPU Power Management**
- **Kext**: [`CPUFriend.kext`](efi/oc/Kexts/CPUFriend.kext) v1.2.9
- **Data Provider**: [`CPUFriendDataProvider.kext`](efi/oc/Kexts/CPUFriendDataProvider.kext) v1.0.0
- **Features**: Native frequency scaling, C-states, P-states

### **Notes**
- **12th Gen Intel**: Requires additional configuration for E-cores
- **AMD CPUs**: Not supported by this configuration
- **Custom frequency data**: May need regeneration for optimal performance

## 🔌 Motherboard Chipsets

### **Intel Chipsets (Supported)**

| Chipset | Platform | LGA Socket | Status | Notes |
|---------|----------|------------|--------|-------|
| **Z170/H170** | Skylake | LGA1151 | ✅ Full | Stable support |
| **Z270/H270** | Kaby Lake | LGA1151 | ✅ Full | Stable support |
| **Z370/H370** | Coffee Lake | LGA1151 | ✅ Full | Stable support |
| **Z390/H370** | Coffee Lake-R | LGA1151 | ✅ Full | Recommended |
| **Z490/H470** | Comet Lake | LGA1200 | ✅ Full | Good support |
| **Z590/H570** | Rocket Lake | LGA1200 | ✅ Full | Good support |
| **Z690/H670** | Alder Lake | LGA1700 | ⚠️ Partial | Requires tuning |

### **Required ACPI**
- **SSDT**: [`MaLd0n.aml`](efi/oc/ACPI/MaLd0n.aml) - Universal ACPI patch
- **Patches**: IRQ fixes for IPIC, RTC, and TIMR

## 🌐 Network Adapters

### **Intel Ethernet**

| Controller | Kext | Version | Status | Notes |
|------------|------|---------|--------|-------|
| **I219-V/LM** | [`IntelMausi.kext`](efi/oc/Kexts/IntelMausi.kext) | v1.0.8 | ✅ Native | Most common |
| **I225-V** | [`AppleIGC.kext`](efi/oc/Kexts/AppleIGC.kext) | v1.5d1 | ✅ Native | 2.5Gb Ethernet |
| **I226-V** | [`AppleIGC.kext`](efi/oc/Kexts/AppleIGC.kext) | v1.5d1 | ✅ Native | 2.5Gb Ethernet |
| **I210/I211** | [`IntelMausi.kext`](efi/oc/Kexts/IntelMausi.kext) | v1.0.8 | ✅ Native | Server grade |

**Boot Arguments**: `dk.e1000=0 e1000=0` (disables conflicting drivers)

### **Realtek Ethernet**

| Controller | Kext | Version | Status | Notes |
|------------|------|---------|--------|-------|
| **RTL8111** | [`RealtekRTL8111.kext`](efi/oc/Kexts/RealtekRTL8111.kext) | v2.5.0 | ✅ Full | Very common |
| **RTL8125** | [`RealtekRTL8111.kext`](efi/oc/Kexts/RealtekRTL8111.kext) | v2.5.0 | ✅ Full | 2.5Gb variant |

### **Atheros Ethernet**

| Controller | Kext | Version | Status | Notes |
|------------|------|---------|--------|-------|
| **AR8161** | [`AtherosE2200Ethernet.kext`](efi/oc/Kexts/AtherosE2200Ethernet.kext) | v2.4.0 | ✅ Full | Common on older boards |
| **AR8162** | [`AtherosE2200Ethernet.kext`](efi/oc/Kexts/AtherosE2200Ethernet.kext) | v2.4.0 | ✅ Full | |
| **AR8171** | [`AtherosE2200Ethernet.kext`](efi/oc/Kexts/AtherosE2200Ethernet.kext) | v2.4.0 | ✅ Full | |

### **WiFi Compatibility**

❌ **Not Included**: This EFI doesn't include WiFi kexts

**Compatible WiFi Cards**:
- **Intel**: AX200, AX201, AX210 (with [itlwm](https://github.com/OpenIntelWireless/itlwm))
- **Broadcom**: BCM94360CD, BCM94352Z (native support)
- **Atheros**: AR9280, AR9380 (with [AirportAtheros40](https://github.com/khronokernel/IO80211-Patches))

## 🎮 Graphics Support

### **Intel Integrated Graphics**

| iGPU | Architecture | Supported | Kext | Notes |
|------|-------------|-----------|------|-------|
| **HD 530** | Skylake | ✅ Full | [`WhateverGreen.kext`](efi/oc/Kexts/WhateverGreen.kext) | Complete support |
| **HD 630** | Kaby Lake | ✅ Full | [`WhateverGreen.kext`](efi/oc/Kexts/WhateverGreen.kext) | Complete support |
| **UHD 630** | Coffee Lake | ✅ Full | [`WhateverGreen.kext`](efi/oc/Kexts/WhateverGreen.kext) | Complete support |
| **UHD 630** | Comet Lake | ✅ Full | [`WhateverGreen.kext`](efi/oc/Kexts/WhateverGreen.kext) | Complete support |
| **UHD 750** | Rocket Lake | ✅ Full | [`WhateverGreen.kext`](efi/oc/Kexts/WhateverGreen.kext) | Complete support |
| **UHD 770** | Alder Lake | ⚠️ Partial | [`WhateverGreen.kext`](efi/oc/Kexts/WhateverGreen.kext) | Requires additional patches |

### **AMD Discrete Graphics**

| GPU Family | Examples | Status | Boot Args | Notes |
|------------|----------|--------|-----------|-------|
| **Polaris** | RX 460/470/480/560/570/580 | ✅ Native | `agdpmod=pikera` | Perfect support |
| **Vega** | RX Vega 56/64 | ✅ Native | `agdpmod=pikera` | Perfect support |
| **Navi (RDNA)** | RX 5500/5600/5700 XT | ✅ Native | `agdpmod=pikera` | Perfect support |
| **Navi (RDNA2)** | RX 6600/6700/6800/6900 XT | ✅ Native | `agdpmod=pikera` | Perfect support |
| **RDNA3** | RX 7600/7700/7800/7900 XT | ❌ No | N/A | No macOS support |

### **NVIDIA Graphics**

| GPU Family | Examples | Status | Notes |
|------------|----------|--------|-------|
| **Maxwell** | GTX 900 series | ❌ Limited | Web drivers for older macOS only |
| **Pascal** | GTX 10 series | ❌ No | No macOS support |
| **Turing** | RTX 20 series | ❌ No | No macOS support |
| **Ampere** | RTX 30 series | ❌ No | No macOS support |

**Boot Arguments**: `agdpmod=pikera` - Fixes AMD GPU board-id and connection issues

## 🔊 Audio Support

### **Audio Configuration**
- **Kext**: [`AppleALC.kext`](efi/oc/Kexts/AppleALC.kext) v1.9.4
- **Layout ID**: 1 (configured via `alcid=1` boot argument)
- **Codec Support**: Most Realtek, Creative, and VIA codecs

### **Common Audio Codecs**

| Codec | Layout IDs | Status | Notes |
|-------|------------|--------|-------|
| **ALC887** | 1, 2, 3, 5, 7, 11, 13, 17, 18, 20, 33, 87, 99 | ✅ Full | Very common |
| **ALC892** | 1, 2, 3, 4, 5, 7, 12, 15, 16, 17, 20, 22, 28, 31, 32, 90, 92, 97 | ✅ Full | Common on gaming boards |
| **ALC1200** | 1, 2, 3, 5, 7, 11, 12, 15, 17, 20, 21, 27, 28, 29, 34 | ✅ Full | High-end codec |
| **ALC1220** | 1, 2, 3, 5, 7, 11, 13, 15, 16, 17, 19, 20, 21, 27, 28, 29, 34 | ✅ Full | Gaming/enthusiast |

### **Audio Troubleshooting**
1. **No audio**: Try different layout IDs in boot arguments (`alcid=X`)
2. **Partial audio**: Check [AppleALC documentation](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs)
3. **Find your codec**: `ioreg -l | grep -i "codec"`

## 🔌 USB Configuration

### **USB Kexts**
- **Injection**: [`USBInjectAll.kext`](efi/oc/Kexts/USBInjectAll.kext) v0.8.1
- **XHCI Support**: [`XHCI-unsupported.kext`](efi/oc/Kexts/XHCI-unsupported.kext) v0.9.2
- **Port Limit**: XhciPortLimit quirk enabled (temporary)

### **USB Standards Support**
- **USB 2.0**: ✅ Full support
- **USB 3.0/3.1**: ✅ Full support
- **USB 3.2**: ✅ Depends on controller
- **USB-C**: ⚠️ Varies by implementation

### **USB Port Mapping**
For optimal USB functionality:
1. **Use USBMap tool** to create custom port map
2. **Replace USBInjectAll** with custom USBMap.kext
3. **Disable XhciPortLimit** after mapping

## 🔷 Additional Hardware

### **Bluetooth Support**
- **Kext**: [`BlueToolFixup.kext`](efi/oc/Kexts/BlueToolFixup.kext) v2.7.0
- **Compatibility**: Intel and Broadcom controllers
- **Status**: ✅ Working with compatible cards

### **Storage Support**
- **SATA**: ✅ Native support (AHCI mode required)
- **NVMe**: ✅ Native support
- **RAID**: ❌ Not recommended for macOS

### **Memory Support**
- **DDR4**: ✅ Full support
- **DDR5**: ✅ With compatible motherboards
- **XMP/DOCP**: ✅ Works in most cases

## ⚠️ Hardware Limitations

### **Unsupported Hardware**
- **AMD CPUs**: Requires different configuration
- **NVIDIA GPUs**: No current macOS support
- **Intel WiFi**: Requires additional kexts not included
- **Thunderbolt**: Limited support, varies by implementation
- **RAID configurations**: Not recommended

### **Problematic Hardware**
- **B360/H310 Chipsets**: May require additional patches
- **Some Z690/Z790**: Newer chipsets may need updates
- **Integrated WiFi**: Often requires replacement
- **Some USB controllers**: May need custom mapping

## 🔧 Hardware Configuration Tips

### **Optimal Configuration**
- **CPU**: Intel 8th-11th gen for best compatibility
- **Motherboard**: Z390/Z490/Z590 chipsets recommended
- **GPU**: AMD RX 5000/6000 series for best performance
- **Network**: Intel I219-V or Realtek RTL8111
- **WiFi**: Replace with compatible Broadcom card

### **BIOS Recommendations**
- **Disable**: Secure Boot, Fast Boot, CSM, VT-d
- **Enable**: VT-x, Above 4G Decoding, XHCI Hand-off
- **Set SATA to AHCI mode**
- **Disable integrated graphics if using discrete GPU**

## 📊 Compatibility Matrix

| Component | Support Level | Notes |
|-----------|---------------|-------|
| **Intel CPU (8th-11th gen)** | 🟢 Excellent | Recommended platform |
| **AMD RX 5000/6000 GPU** | 🟢 Excellent | Native acceleration |
| **Intel Ethernet** | 🟢 Excellent | Multiple variants supported |
| **Realtek Audio** | 🟢 Excellent | Most codecs supported |
| **USB 2.0/3.0** | 🟢 Excellent | Full support |
| **12th gen Intel** | 🟡 Good | Requires additional tuning |
| **Intel Graphics** | 🟡 Good | Full acceleration |
| **Bluetooth** | 🟡 Good | Depends on controller |
| **WiFi** | 🟡 Limited | Requires compatible card |
| **Thunderbolt** | 🟡 Limited | System dependent |
| **NVIDIA GPU** | 🔴 None | No macOS support |
| **AMD CPU** | 🔴 None | Wrong configuration |

## 🔍 Hardware Detection

### **System Information Commands**
```bash
# CPU Information
sysctl -n machdep.cpu.brand_string

# Graphics Information
system_profiler SPDisplaysDataType

# Audio Information
system_profiler SPAudioDataType

# Network Information
system_profiler SPNetworkDataType

# USB Information
system_profiler SPUSBDataType
```

### **Hardware Verification**
1. **Check System Information** app for proper detection
2. **Verify PCI devices**: `lspci` (if installed)
3. **Check loaded kexts**: `kextstat`
4. **Test functionality**: Network, audio, graphics, sleep

---

**💡 Pro Tip**: Always verify your specific hardware model numbers against this compatibility list before installation. Hardware manufacturers often use different revisions with the same model name that may require different configurations.