# Hackintosh Installation Guide

This comprehensive guide will walk you through installing macOS on your Intel system using this OpenCore EFI configuration.

## 📋 Prerequisites

### **Hardware Requirements**
- Intel CPU (6th generation or newer recommended)
- Compatible Intel/AMD chipset
- 8GB+ RAM (16GB+ recommended)
- 120GB+ storage device (SSD recommended)
- UEFI motherboard with CSM disabled
- Supported network adapter (see [`HARDWARE.md`](HARDWARE.md))

### **Software Requirements**
- 8GB+ USB drive
- Access to a Mac or existing Hackintosh
- [macOS Installer](https://apps.apple.com/us/app/macos-big-sur/id1526878132) (Big Sur or newer)
- [OpenCore](https://github.com/acidanthera/OpenCorePkg/releases) (if updating)
- [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
- [ProperTree](https://github.com/corpnewt/ProperTree) (for [`config.plist`](efi/oc/config.plist:1) editing)

### **BIOS/UEFI Settings**

#### **Disable the Following:**
- **Secure Boot** (mandatory)
- **Fast Boot** 
- **CSM/Legacy Boot**
- **Intel VT-d** (or enable `DisableIoMapper` in config)
- **Intel SGX**
- **Intel Platform Trust Technology**
- **CFG Lock** (if available, or use `AppleXcpmCfgLock` quirk)

#### **Enable the Following:**
- **VT-x** (mandatory)
- **Above 4G Decoding**
- **Hyper-Threading**
- **EHCI/XHCI Hand-off**
- **OS Type: Windows 10 WHQL** (if available)
- **SATA Mode: AHCI**

## 🛠️ Step 1: Create macOS Installer

### **Method 1: Using macOS (Recommended)**

1. **Download macOS Installer**
   ```bash
   # Download from App Store or use the following commands:
   # For macOS Big Sur:
   curl -s https://raw.githubusercontent.com/corpnewt/gibMacOS/master/gibMacOS.command | bash
   ```

2. **Format USB Drive**
   ```bash
   # Replace 'MyVolume' with your USB drive name
   sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
   ```

3. **Mount EFI Partition**
   ```bash
   # Find your USB drive identifier
   diskutil list
   
   # Mount EFI partition (replace diskX with your USB identifier)
   sudo diskutil mount diskXs1
   ```

### **Method 2: Using Windows**

1. Use [gibMacOS](https://github.com/corpnewt/gibMacOS) to download macOS
2. Use [MakeInstall](https://github.com/corpnewt/MakeInstall) to create installer
3. Mount EFI partition using [MountEFI](https://github.com/corpnewt/MountEFI)

## 🔧 Step 2: Configure EFI

### **Install EFI Files**

1. **Copy EFI Folder**
   ```bash
   # Copy this repository's EFI folder to your USB
   cp -R /path/to/hackintosh-efi/efi /Volumes/EFI/
   ```

2. **Verify Structure**
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

### **Generate SMBIOS Data**

1. **Download GenSMBIOS**
   ```bash
   git clone https://github.com/corpnewt/GenSMBIOS
   cd GenSMBIOS
   python GenSMBIOS.command
   ```

2. **Generate MacPro7,1 SMBIOS**
   - Select option `1` to install/update MacSerial
   - Select option `3` to generate SMBIOS
   - Enter `MacPro7,1` when prompted
   - Copy the generated values

3. **Update config.plist**
   
   Open [`config.plist`](efi/oc/config.plist:764) with ProperTree and replace:
   
   ```xml
   <key>PlatformInfo</key>
   <dict>
       <key>Generic</key>
       <dict>
           <key>MLB</key>
           <string>REPLACEME_MLB</string>                    <!-- Replace with generated MLB -->
           <key>ROM</key>
           <data>UkVQTEFDRU1F</data>                        <!-- Replace with MAC address -->
           <key>SystemProductName</key>
           <string>MacPro7,1</string>                       <!-- Keep as MacPro7,1 -->
           <key>SystemSerialNumber</key>
           <string>REPLACEME_SERIAL</string>                <!-- Replace with generated Serial -->
           <key>SystemUUID</key>
           <string>REPLACEME-UUID-REPLACE-WITH-GENERIC</string> <!-- Replace with generated UUID -->
       </dict>
   </dict>
   ```

4. **Set ROM Value**
   - Find your network adapter's MAC address
   - Convert to Base64 or use your MAC address directly
   - Replace `UkVQTEFDRU1F` with your encoded MAC address

### **Configure Network Kexts**

Based on your network hardware, disable unused kexts in [`config.plist`](efi/oc/config.plist:198):

1. **Intel Ethernet (I219-V, I225-V, I226-V)**
   - Keep [`IntelMausi.kext`](efi/oc/Kexts/IntelMausi.kext) enabled
   - Keep [`AppleIGC.kext`](efi/oc/Kexts/AppleIGC.kext) enabled (for I225-V/I226-V)
   - Disable [`AtherosE2200Ethernet.kext`](efi/oc/Kexts/AtherosE2200Ethernet.kext) and [`RealtekRTL8111.kext`](efi/oc/Kexts/RealtekRTL8111.kext)

2. **Atheros Ethernet**
   - Keep [`AtherosE2200Ethernet.kext`](efi/oc/Kexts/AtherosE2200Ethernet.kext) enabled
   - Disable [`IntelMausi.kext`](efi/oc/Kexts/IntelMausi.kext), [`AppleIGC.kext`](efi/oc/Kexts/AppleIGC.kext), and [`RealtekRTL8111.kext`](efi/oc/Kexts/RealtekRTL8111.kext)

3. **Realtek RTL8111**
   - Keep [`RealtekRTL8111.kext`](efi/oc/Kexts/RealtekRTL8111.kext) enabled
   - Disable other network kexts

## 🚀 Step 3: Installation Process

### **Boot from USB**

1. **Insert USB Installer** into target system
2. **Boot to BIOS/UEFI** (usually F2, F12, or DEL during startup)
3. **Select USB as boot device** or change boot priority
4. **Boot from UEFI: [USB Name]** (not legacy)

### **OpenCore Boot Menu**

1. **Select macOS Installer** from OpenCore boot picker
2. **Wait for verbose boot** (if enabled) or Apple logo
3. **If boot fails**, restart and try different options

### **macOS Installation**

1. **Select Language** and continue
2. **Open Disk Utility** from macOS Utilities
3. **Format target drive**:
   - Select your installation drive
   - Click "Erase"
   - Format: **APFS**
   - Scheme: **GUID Partition Map**
   - Name: **Macintosh HD** (or preferred name)

4. **Run macOS Installer**:
   - Exit Disk Utility
   - Select "Install macOS"
   - Choose your formatted drive
   - Wait for installation (30-60 minutes)

5. **System will restart multiple times** - always boot from USB until setup is complete

## ⚙️ Step 4: Post-Installation

### **Install EFI to Hard Drive**

1. **Mount EFI partition** of your macOS drive:
   ```bash
   sudo diskutil mount disk0s1
   ```

2. **Copy EFI folder** from USB to hard drive:
   ```bash
   cp -R /Volumes/EFI/EFI /Volumes/EFI/
   ```

3. **Test booting** without USB drive

### **Complete macOS Setup**

1. **Boot from hard drive** (remove USB)
2. **Complete macOS Setup Assistant**:
   - Create user account
   - Skip Apple ID (recommended for Hackintosh)
   - Disable analytics and Siri
   - Configure privacy settings

### **Verify Functionality**

1. **Check System Information**:
   - About This Mac should show MacPro7,1
   - Memory, graphics, and storage should be detected

2. **Test Hardware**:
   - Ethernet connectivity
   - Audio output/input
   - USB ports (2.0 and 3.0)
   - Graphics acceleration
   - Sleep/wake functionality

## 🔧 Post-Installation Tweaks

### **Audio Configuration**

If audio doesn't work with `alcid=1`:

1. **Find your codec** using Terminal:
   ```bash
   ioreg -l | grep -i "codec"
   ```

2. **Try different layout IDs** in boot arguments:
   ```
   alcid=1, alcid=2, alcid=3, alcid=7, alcid=11, etc.
   ```

3. **Check [AppleALC wiki](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs)** for your specific codec

### **USB Port Mapping**

For optimal USB functionality:

1. **Download [USBMap](https://github.com/corpnewt/USBMap)**
2. **Generate custom USB map** for your motherboard
3. **Replace [`USBInjectAll.kext`](efi/oc/Kexts/USBInjectAll.kext)** with custom USBMap.kext
4. **Disable XhciPortLimit** quirk in [`config.plist`](efi/oc/config.plist:554)

### **CPU Power Management**

For better power management:

1. **Generate CPUFriendDataProvider** using [CPUFriend](https://github.com/acidanthera/CPUFriend)
2. **Replace existing [`CPUFriendDataProvider.kext`](efi/oc/Kexts/CPUFriendDataProvider.kext)**
3. **Verify power management** with Intel Power Gadget

## 🛠️ Troubleshooting

### **Common Boot Issues**

| Issue | Solution |
|-------|----------|
| **Stuck at Apple Logo** | Add `-v` to boot arguments for verbose mode |
| **Kernel Panic** | Check hardware compatibility, disable problematic kexts |
| **No boot picker** | Set `ShowPicker` to `true` in [`config.plist`](efi/oc/config.plist:599) |
| **Graphics issues** | Verify WhateverGreen, check boot arguments |
| **USB not working** | Enable XhciPortLimit, check XHCI-unsupported |

### **Installation Issues**

| Issue | Solution |
|-------|----------|
| **"This copy of the Install macOS application is damaged"** | Re-download installer, check USB drive |
| **Installer won't start** | Verify BIOS settings, check EFI configuration |
| **Drive not visible** | Enable AHCI mode in BIOS |
| **Installation fails** | Check storage compatibility, try different drive |

### **Post-Installation Issues**

| Issue | Solution |
|-------|----------|
| **No audio** | Try different alcid values, check codec compatibility |
| **No Ethernet** | Enable correct network kext, disable others |
| **Sleep issues** | Check power management, USB configuration |
| **Graphics acceleration issues** | Verify WhateverGreen configuration |

## 🔍 Boot Arguments Reference

Current boot arguments in this EFI:
```
alcid=1 watchdog=0 agdpmod=pikera dk.e1000=0 e1000=0
```

### **Additional Boot Arguments (if needed)**
- `-v` - Verbose mode (for troubleshooting)
- `-s` - Single user mode
- `-x` - Safe mode
- `debug=0x100` - Disable kernel panic reboot
- `keepsyms=1` - Keep kernel symbols for panic logs
- `npci=0x2000` - Fix PCI configuration
- `nv_disable=1` - Disable NVIDIA graphics
- `igfxonln=1` - Force online status for Intel graphics

## 📞 Getting Help

### **Useful Resources**
- [Dortania OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [r/hackintosh subreddit](https://www.reddit.com/r/hackintosh/)
- [tonymacx86 forums](https://www.tonymacx86.com/)
- [OpenCore documentation](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs)

### **Before Asking for Help**
1. Check this troubleshooting section
2. Review hardware compatibility in [`HARDWARE.md`](HARDWARE.md)
3. Provide complete system specifications
4. Include boot arguments and kext list
5. Share panic logs or error messages

---

**⚠️ Important**: Always backup your working EFI configuration before making changes. Keep a bootable USB with known-good EFI as a recovery option.