# HP EliteDesk 800 G2 Tower PC (Skylake) for macOS Mojave

Hackintosh your HP EliteDesk 800 G2 Tower PC Skylake. This is intented to create a fully functional hackintosh for the HP EliteDesk 800 G2 Tower PC i7-6700 (Skylake).

## Important Notes
[up up up](#)

* This guide is for the **HP EliteDesk 800 G2 TWR PC i7-6700 (Skylake)**. 
* All files used and detailed readmes are located in github [sakoula/HP-EliteDesk-800-G2-6700](https://github.com/sakoula/HP-EliteDesk-800-G2-6700/blob/master/Changelog.md)
* The guide was fully tested with **BIOS N01 Ver. 2.36 07/18/2018**
* Following this guide you can run any version of **Mojave 10.14.x**
* macOS has been installed on an internal SSD. I have no experience of having both Windows and macOS on a single disk.
* In order to be able to help you please provide full debug information useing the excellent [black-dragon74/OSX-Debug
](https://github.com/black-dragon74/OSX-Debug) tool.
* Support and Discussion on this guide can be found at [tonymacx86.com](https://www.tonymacx86.com/threads/hp-elitedesk-800-g2-hp-prodesk-600-g2-success.261452/)

---

## Table of Contents
[up up up](#)

<!-- MarkdownTOC -->

- [Known Issues / Work in Progress](#known-issues--work-in-progress)
- [Dual Monitor](#dual-monitor)
- [Hardware Specifications](#hardware-specifications)
- [Installation Guide](#installation-guide)
	- [Installation SSD](#installation-ssd)
	- [Keys Shortcuts to Boot](#keys-shortcuts-to-boot)
	- [BIOS setup](#bios-setup)
	- [Preparing USB Flash Drive](#preparing-usb-flash-drive)
	- [Install mojave installer to the USB Flash Drive](#install-mojave-installer-to-the-usb-flash-drive)
	- [Install `Clover` to the USB Flash Drive](#install-clover-to-the-usb-flash-drive)
	- [Customize Clover on the USB Flash Drive](#customize-clover-on-the-usb-flash-drive)
	- [Install Mojave](#install-mojave)
	- [Install `Clover` on the macOS disk](#install-clover-on-the-macos-disk)
	- [Customize Clover on the macOS disk](#customize-clover-on-the-macos-disk)
	- [Move kexts to `/Library/Extentions`](#move-kexts-to-libraryextentions)
	- [Create a USB Flash Drive just with `Clover` for emergencies](#create-a-usb-flash-drive-just-with-clover-for-emergencies)
- [Postinstallation Steps](#postinstallation-steps)
	- [Create a valid SMBIOS](#create-a-valid-smbios)
	- [Disable Hibernation](#disable-hibernation)
- [Benchmarking](#benchmarking)
	- [Benchmarking Windows 10](#benchmarking-windows-10)
	- [Benchmarking macOS 10.14.2](#benchmarking-macos-10142)
- [Patching Information](#patching-information)
	- [CPU](#cpu)
	- [Audio](#audio)
	- [Ethernet](#ethernet)
	- [Graphics](#graphics)
	- [USB](#usb)
- [Changelog](#changelog)
- [Buy me a coffee or a beer](#buy-me-a-coffee-or-a-beer)
- [Credits](#credits)

<!-- /MarkdownTOC -->

<!--
* This line is a placeholder to generate the table of contents in jekyll
{:toc}

[TOC]
-->

## Known Issues / Work in Progress
[up up up](#)

* sleep [hibernation](https://www.tonymacx86.com/threads/guide-native-power-management-for-laptops.175801/) *work in progress (not focus of the guide)*
* check whether this computer is affected by [goodwin/ALCPlugFix](https://github.com/goodwin/ALCPlugFix) *work in progress*
* Audio through DisplayPorts *has not checked and is not the focus of the guide*
* Enable HiDPI resolutions *work in progress*

If you face another problem please open a issue.

## Dual Monitor
[up up up](#)

> Dual monitors works. Patching has been done using hackingtool. HotPlugging a monitor is not required. The following patch has been applied

```
WhateverGreen: weg @ (DBG) agdpmod using config vit9696
WhateverGreen: igfx @ (DBG) patching framebufferId 0x19120000 connector [1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187
WhateverGreen: igfx @ (DBG) patching framebufferId 0x19120000 connector [2] busId: 0x06, pipe: 10, type: 0x00000400, flags: 0x00000187
WhateverGreen: igfx @ (DBG) patching framebufferId 0x19120000 connector [3] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187
WhateverGreen: igfx @ (DBG) patching framebufferId 0x19120000 connector [-1] busId: 0x00, pipe: 0, type: 0x00000001, flags: 0x00000020

Patch

General > Devices/Properties
General > Connectors
General > Auto Detect Changes
Advanced > FB Port Limit 3
Device Id: 0x1912: Intel HD Graphics 530
```

This configuration works with an `HP EliteDisplay E242` on the top DP port and an `HP EliteDisplay E243i` on the bottom DP port. Very rarely when I pass the boot process and once it turns into 'graphics mode' (Apple Logo) the machine sometimes hangs there for some seconds and reboots (Framebuffer is crashing).

I noticed that if I have only one monitor on the top DP port (any) the coputer boots fine and then I can hot plug the other monitor once on the logon screen I do not have any problem.

## Hardware Specifications
[up up up](#)

* Product Number: L1G77AV
* HP EliteDesk 800 G2 Tower PC

hardware configuration with the following specs:

* Skylake
* [Skylake (100 series)](https://en.wikipedia.org/wiki/LGA_1151#Skylake_chipsets_(100_series))
* HP EliteDesk 800 G2 TWR
* MOBO HP 8053 (U3E1)
* Intel® 8 Q170
* BIOS 02.36
* Intel Core i7 6700 @ 3.40GHz Skylake
* GPU: Integrated: Intel® HD Graphics 530 with (2) DisplayPorts with multi - stream and VGA
* 32GB DDR4 2133 DIMM (Dual-Channel Unknown @ 1064MHz (15-15-15-36))
* Audio: Realtek ALC 221 Audio (revision 0x100103)
* Network: Intel® I219LM Gigabit Network Connection LOM
* 250GB SanDisk SD7SB3Q-256G-1006 (SSD)
* 1000GB Seagate Barracuda 7200.14 ST1000DM003-1SB102 (SATA)
* 1000GB Western Digital Caviar Green WDC WD10EAVS-14M4B0 (SATA)
* display1: HP EliteDisplay E242
* display2: HP EliteDisplay E243i

full specs from the [HP site](http://store.hp.com/us/en/pdp/hp-elitedesk-800-g2-tower-pc-p-w5x93ut-aba--1)

## Installation Guide
[up up up](#)

These are the steps in order to install or upgrade your EliteDesk-800. There is a very detailed document on the steps followed and the customizations which can be found in [DETAILS.md](DETAILS.md).

There is a another document on how I setup my environment including all the tools and utilities I have used [ENVIRONMENT.wks.md](ENVIRONMENT.wks.md).

You will need a working macOS installation (no matter the version) to create a USB Flash Drive with macOS.

Start by downloading the latest version the customization files from the [releases](https://github.com/sakoula/HP-EliteDesk-800-G2-6700/releases) page. It includes:

* `ELITEDESK800_EFI/`: efi directory including all kexts and customization needed
* `addons/LiluFriendLite.kext`: `LiluFriendLite.kext` used in the installation
* `addons/HFSPlus.efi`: `HFSPlus.efi` used in the installation
* `addons/VirtualSmc.efi`: `VirtualSmc.efi` used in the installation

### Installation SSD
[up up up](#)

I have installed macOS on the internal `238GB SanDisk SD7SB3Q-256G-1006 (SSD)`.

### Keys Shortcuts to Boot
[up up up](#)

* `F9` Boot Menu
* `F10` BIOS
* `<ESC>` Startup Menu

### BIOS setup
[up up up](#)

Make sure that you check with this [link](http://support.hp.com/us-en/product/hp-elitedesk-800-g2-tower-pc/7633297/drivers) to update BIOS:

* `sp90164`: HP EliteDesk 800 G2 TWR/SFF SystemBIOS (N01) / 02.36 Rev.A / Aug 13, 2018

> Note: Update the BIOS from the BIOS itself. On the CLOVER USB stick place all bios files in *EFI/HP/BIOS/new*. On Finder create an extra EFI directory under the mounted EFI exactly as Clover do.

Get into the `BIOS` and make the following changes:

BIOS Settings:

* ~~Broke Raid without any problem on booting from one disk~~ (computer came with no raid)
* ~~Setup disk as AHCI~~ (there is no option on the BIOS)
* `Advanced > Security > TPM Embedded Security > TPM Device` **Hidden**
* `Advanced > Security > BIOS SureStart >` **Unchecked ALL**
* `Advanced > Security > System Management Command` **Unchecked**
* `Advanced > Security > SIntel Software Guard Extensions (SGX)` **Disable**
* `Advanced > Boot Options > UEFI boot order` **Checked** (do this or else it will not boot from the stick)
* `Advanced > Boot Options > Audio Alerts During Boot` **Unchecked**
* `Advanced > Secure Boot Configuration >` **Unchecked ALL**
* `Advanced > Secure Boot Configuration > Configure Legacy Support and Secure Boot` **Legacy Support Enable and Secure Boot Disable**
* `Advanced > System Options > Configure Storage Controller for RAID` **Unchecked**
* `Advanced > System Options > VTx` **Checked**
* `Advanced > System Options > VTd` **Unchecked**
* `Advanced > System Options > PCI*` **Unchecked**
* `Advanced > System Options > Allow PCIe/PCI SERR# Interrupt` **Checked**
* `Advanced > Built-In Device Options > Video memory size` **512MB** [here](https://www.tonymacx86.com/threads/skylake-intel-hd-530-integrated-graphics-working-as-of-10-11-4.188891/page-40) 
* `Advanced > Built-In Device Options > Audio Device` **Checked**
* `Advanced > Built-In Device Options > Internal Speakers` **Checked**
* `Advanced > Port Options > Serial Port A` **Unchecked**
* `Advanced > Port Options > SATA*` **Checked**
* `Advanced > Port Options > *USB*` **Checked**
* `Advanced > Port Options > USB Charging Port Function` **Checked**
* `Advanced > Port Options > Media Card Reader/SD_RDR USB` **Checked**
* `Advanced > Port Options > Restrict USB Devices` **Allow all USB Devices**
* `Advanced > Power Management Options > Runtime Power Management` **Checked**
* `Advanced > Power Management Options > Extended Idle Power States` **Checked**
* `Advanced > Power Management Options > S5 Maximum Power Savings` **Checked**
* `Advanced > Power Management Options > SATA Power Management` **Checked**
* `Advanced > Power Management Options > PCI Express Power Management` **Checked**
* `Advanced > Power Management Options > Power On from Keyboard Ports` **Unchecked**
* `Advanced > Power Management Options > Unique Sleep State Blink Rates` **Unchecked**
* `Advanced > Remote Management Options > Active Management (AMT)` **Unchecked**
* `Advanced > Option ROM Launch Policy > Configure Option ROM Launch Policy > All UEI` for the multimonitor support on boot

### Preparing USB Flash Drive
[up up up](#)

[Get a at least 16GB](https://support.apple.com/en-us/HT201372) USB Flash Drive and:

`Disk Utility > Select USB Device > Erase`:

* GUID Partition Table
* Name: USB
* Format: MacOS Extended (Journaled)

### Install mojave installer to the USB Flash Drive
[up up up](#)

Download mojave from Apple AppStore and run the following command to install it on the USB disk you just Erased.

`$ sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/USB`

### Install `Clover` to the USB Flash Drive
[up up up](#)

Rehabman's fork of clover tends to be more stable so we are goinf to use this. Download it from [here](https://bitbucket.org/RehabMan/clover/downloads/).

Run `Clover_v2.4k_r4701.RM-4963.ca6cca7c` installer:

*Clover for UEFI booting only*, *Install Clover in the ESP*

*UEFI Drivers* / `drivers64UEFI`:

* `ApfsDriverLoader-64.efi`
* `AppleImageLoader-64.efi`
* `AptioMemoryFix-64.efi`
* `DataHubDxe-64.efi`
* `FSInject-64.efi`

*FileVault 2 UEFI Drivers* / `drivers64UEFI`:

* `AppleKeyFeeder-64.efi`
* `AppleUISupport-64.efi`
* `AptioInputFix-64.efi`

### Customize Clover on the USB Flash Drive
[up up up](#)

Download the latest [release](https://github.com/sakoula/HP-EliteDesk-800-G2-6700/releases) from github and unzip the archive. You will find an `ELITEDESK800_EFI` directory and a `addons` directory. Mount the USB Flash Drive's `EFI` partition on `/Volumes/EFI`:

1. copy `ELITEDESK800_EFI/CLOVER/kexts/Other` from the downloaded file to USB's EFI to `EFI/CLOVER/kexts/Other`

2. make sure that on the USB's EFI `EFI/CLOVER/kexts/Other` you just have `Lilu.kext`, `WhateverGreen.kext`, `USBInjectAll.kext`, `IntelMausiEthernet.kext`, `VirtualSMC.kext`, `SATA-unsupported.kext`. **Erase the rest**.

3. copy `addons/HFSPlus.efi` from the downloaded file to USB's EFI to `EFI/CLOVER/drivers64UEFI/HFSPlus.efi`

4. copy `addons/VirtualSmc.efi` from the downloaded file to USB's EFI to `EFI/CLOVER/drivers64UEFI/VirtualSmc.efi`

5. copy `ELITEDESK800_EFI/CLOVER/ACPI/PATCHED/*` from the downloaded file to USB's EFI to `EFI/CLOVER/ACPI/PATCHED/*`

6. copy `ELITEDESK800_EFI/CLOVER/config.plist` from the downloaded file to USB's EFI to `EFI/CLOVER/ACPI/config.plist`

### Install Mojave
[up up up](#)

To boot from the USB Flash Drive you can just hit `F9` and you will get the UEFI boot loader

Boot from the USB and install Mojave on the hard disk. 

> **Important**: During installation you will ask to reboot the machine. While on clover make sure to boot from `Boot macOS install from *** disk` disk. If you do not see this disk hit `F3` to show all the hidden disks. You may need to reboot multiple times.

### Install `Clover` on the macOS disk
[up up up](#)

Once the installation is over you will need to install `Clover` bootloader on the hard disk that you have installed macOS in order to be able to boot without the USB Flash Drive.

Run again the `Clover_v2.4k_r4701.RM-4963.ca6cca7c` installer:

*Clover for UEFI booting only*, *Install Clover in the ESP*

*UEFI Drivers* / `drivers64UEFI`:

* `ApfsDriverLoader-64.efi`
* `AppleImageLoader-64.efi`
* `AptioMemoryFix-64.efi`
* `DataHubDxe-64.efi`
* `FSInject-64.efi`

*FileVault 2 UEFI Drivers* / `drivers64UEFI`:

* `AppleKeyFeeder-64.efi`
* `AppleUISupport-64.efi`
* `AptioInputFix-64.efi`

*Install Clover Preference Pane*

### Customize Clover on the macOS disk
[up up up](#)

Download the latest [release](https://github.com/sakoula/HP-EliteDesk-800-G2-6700/releases) from github and unzip the archive. You will find an `ELITEDESK800_EFI` directory and a `addons` directory. 

Mount the EFI partition of the macOS boot parition on `/Volumes/EFI`:

1. copy `ELITEDESK800_EFI/CLOVER/kexts/Other` from the downloaded file to USB's EFI to `EFI/CLOVER/kexts/Other`

2. copy `addons/HFSPlus.efi` from the downloaded file to USB's EFI to `EFI/CLOVER/drivers64UEFI/HFSPlus.efi`

3. copy `addons/VirtualSmc.efi` from the downloaded file to USB's EFI to `EFI/CLOVER/drivers64UEFI/VirtualSmc.efi`

4. copy `ELITEDESK800_EFI/CLOVER/ACPI/PATCHED/*` from the downloaded file to USB's EFI to `EFI/CLOVER/ACPI/PATCHED/*`

5. copy `ELITEDESK800_EFI/CLOVER/config.plist` from the downloaded file to USB's EFI to `EFI/CLOVER/ACPI/config.plist`

### Move kexts to `/Library/Extentions`
[up up up](#)

The right way to load kexts is **not** by injecting them through clover but installing them in `/Library/Extentions` and building them into the kernel cache. 

Download the latest [release](https://github.com/sakoula/HP-EliteDesk-800-G2-6700/releases) from github and unzip the archive. You will find an `ELITEDESK800_EFI` directory and a `addons` directory. 

Mount the EFI partition of the macOS boot parition on `/Volumes/EFI`:

1. **move** `EFI/CLOVER/kexts/Other/*` from macOS boot parition to `/Library/Extentions/*`
2. run from the console `$ sudo chown -R root:wheel /Library/Extentions/*`
3. run from the console `$ sudo chmod -r 755 /Library/Extentions/*`
4. run from the console `$ sudo kextcache -i /` to rebuild the caches
5. **move** `addons/LiluFriendLite.kext` from the downloaded file to `/Library/Extentions/LiluFriendLite.kext`
6. run from the console `$ sudo chown -R root:wheel /Library/Extentions/*`
7. run from the console `$ sudo chmod -r 755 /Library/Extentions/*`
8. run from the console `$ sudo kextcache -i /` to rebuild the caches

**remember** that `kextcache` needs to be run twice

### Create a USB Flash Drive just with `Clover` for emergencies
[up up up](#)

Get a small (2GB will work just fine) USB Flash Drive and:

`Disk Utility > Select USB Device > Erase`:

* GUID Partition Table
* Name: CLOVER
* Format: MS-DOS-FAT

Run the `Clover_v2.4k_r4701.RM-4963.ca6cca7c` installer:

*Clover for UEFI booting only*, *Install Clover in the ESP*

*UEFI Drivers* / `drivers64UEFI`:

* `ApfsDriverLoader-64.efi`
* `AppleImageLoader-64.efi`
* `AptioMemoryFix-64.efi`
* `DataHubDxe-64.efi`
* `FSInject-64.efi`

*FileVault 2 UEFI Drivers* / `drivers64UEFI`:

* `AppleKeyFeeder-64.efi`
* `AppleUISupport-64.efi`
* `AptioInputFix-64.efi`

Download the latest [release](https://github.com/sakoula/HP-EliteDesk-800-G2-6700/releases) from github and unzip the archive. You will find an `ELITEDESK800_EFI` directory and a `addons` directory. Mount the USB Flash Drive's `EFI` partition on `/Volumes/EFI`:

1. copy `ELITEDESK800_EFI/CLOVER/kexts/Other` from the downloaded file to USB's EFI to `EFI/CLOVER/kexts/Other`

2. make sure that on `EFI/CLOVER/kexts/Other` you just have ``Lilu.kext`, `WhateverGreen.kext`, `USBInjectAll.kext`, `IntelMausiEthernet.kext`, `VirtualSMC.kext`, `SATA-unsupported.kext`. **Erase the rest**.

3. copy `addons/HFSPlus.efi` from the downloaded file to USB's EFI to `EFI/CLOVER/drivers64UEFI/HFSPlus.efi`

4. copy `addons/VirtualSmc.efi` from the downloaded file to USB's EFI to `EFI/CLOVER/drivers64UEFI/VirtualSmc.efi`

5. copy `ELITEDESK800_EFI/CLOVER/ACPI/PATCHED/*` from the downloaded file to USB's EFI to `EFI/CLOVER/ACPI/PATCHED/*`

6. copy `ELITEDESK800_EFI/CLOVER/config.plist` from the downloaded file to USB's EFI to `EFI/CLOVER/ACPI/config.plist`

7. edit `config.plist` change the `SystemParameters`:

```xml
<dict>
  <key>InjectKexts</key>
  <string>Detect</string>
  <key>InjectSystemID</key>
  <true/>
</dict>
```

## Postinstallation Steps
[up up up](#)

### Create a valid SMBIOS
[up up up](#)

* create a valid SMBIOS. This is really important and do not forget it. In order to setup your hackintosh machine to use Apple Services, iMessage & FaceTime follow the guide [An iDiot's Guide To iMessage ](https://www.tonymacx86.com/threads/an-idiots-guide-to-imessage.196827/)

### Disable Hibernation
[up up up](#)

check your current state:

```shell_session
$ sudo pmset -g
System-wide power settings:
Currently in use:
 standby              1
 Sleep On Power Button 1
 womp                 1
 autorestart          0
 hibernatefile        /var/vm/sleepimage
 proximitywake        1
 powernap             1
 networkoversleep     0
 disksleep            10
 standbydelayhigh     86400
 sleep                0 (sleep prevented by screensharingd)
 autopoweroffdelay    28800
 hibernatemode        3
 autopoweroff         1
 ttyskeepawake        1
 displaysleep         0
 highstandbythreshold 50
 standbydelaylow      0
```

Be aware that hibernation (suspend to disk or S4 sleep) is not supported on hackintosh.

You should disable it:

```shell
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
```

Always check your hibernatemode after updates and disable it. System updates tend to re-enable it, although the trick above (making sleepimage a directory) tends to help.

And it may be a good idea to disable the other hibernation related options:

```shell
sudo pmset -a standby 0
sudo pmset -a autopoweroff 0
```

## Benchmarking
[up up up](#)

### Benchmarking Windows 10
[up up up](#)

* `GeekBench x64 4.0.3 CPU` 4796/15263
* `GeekBench x64 4.0.3 CPU` 4787/15225
* `GeekBench x64 4.0.3 GPU/OpenCl` 21496
* `GeekBench x64 4.0.3 GPU/OpenCl` 21479
* `CINEBENCH R15.038_RC184115 OpenGL` 55.71fps
* `CINEBENCH R15.038_RC184115 OpenGL` 55.53fps
* `CINEBENCH R15.038_RC184115 CPU` 824cb
* `CINEBENCH R15.038_RC184115 CPU` 821cb
* `LuxMark-v3.1 OpenCL GPU` 2544
* `LuxMark-v3.1 OpenCL GPU` 2541
* `LuxMark-v3.1 OpenCL CPU` 2253
* `LuxMark-v3.1 OpenCL CPU` 2274
* `LuxMark-v3.1 Native C++` 2569
* `LuxMark-v3.1 Native C++` 2532

### Benchmarking macOS 10.14.2
[up up up](#)

* `GeekBench x64 4.3.2 CPU` 44903/16686
* `GeekBench x64 4.3.2 GPU/OpenCl` 22007
* `GeekBench x64 4.3.2 GPU/Metal` 23400
* `CINEBENCH R15.038_RC184115 OpenGL` 29.91fps
* `CINEBENCH R15.038_RC184115 CPU` 810cb
* `LuxMark-v3.1 OpenCL GPU` 2141
* `LuxMark-v3.1 OpenCL CPU` 1773
* `Heaven FPS` 13.2 `Score`332 `Min FPS` 9.6 `Max FPS` 26.1
* `AJA System Test Lite SanDisk (with trim) SanDisk SD7SB3Q-256G-1006:` 217MB/sec write, 400MB/sec read
* `AJA System Test Lite Seagate Barracuda 7200.14 ST1000DM003-1SB102:` 140MB/sec write, 161MB/sec read
* `AJA System Test Lite Western Digital Caviar Green WDC WD10EAVS-14M4B0:` 84MB/sec write, 91MB/sec read

## Patching Information
[up up up](#)

Patching has been done using clover and [hotpatching ACPI](https://www.tonymacx86.com/threads/guide-using-clover-to-hotpatch-acpi.200137/). All the required files exist in the `ELITEDESK800_EFI` directory:

* `CLOVER/config.plist` clover configuration file
* `CLOVER/ACPI/origin` BIOS A06 ACPI aml files (from CLOVER with F4)
* `CLOVER/ACPI/patched` ACPI hotpaches 
* `CLOVER/kexts/Other` kexts required

### CPU
[up up up](#)

* The model is `i7-6700`, and XCPM power management is native supported.
* HWP is supported *work in progress*

kext patches in `/CLOVER/kexts/Other` applied:

* `Lilu.kext` Arbitrary kext and process patching on macOS
* `HibernationFixup.kext` Lilu plugin intended to fix hibernation compatibility issues
* `VirtualSMC.kext` SMC emulator layer
* `SATA-unsupported.kext` SATA unsupported

ACPI patches in `/CLOVER/ACPI/patched` applied:

* `SSDT-DMAC.aml` Add missing DMAC Device to enhace performance like a real Mac
* `SSDT-HPET.aml` Disable HPET device by passing value 0 to HPTE to to behave more like a real Mac
* `SSDT-MEM2.aml` Add missing MEM2 Device to enhace performance like a real Mac
* `SSDT-PMCR.aml` Add missing PMCR Device to enhace performance like a real Mac
* `SSDT-GPRW.aml` For solving instant wake by hooking GPRW
* `SSDT-RMCF.aml` Configuration data for other SSDTs(SSDT-GPRW and SSDT-PTSWAK)
* `SSDT-LPC.aml` To fix unsupported 8-series LPC devices (0x9d48).
* `SSDT-PTSWAK.aml` fixing sleep _PTS and _WAK
* `SSDT-SMBUS.aml` add BUS0 device
* `SSDT-XOSI.aml` Override for host defined _OSI to handle "Darwin"

### Audio
[up up up](#)

* Sound card is `Realtek ALC221` which is drived by `AppleALC` on layout-id 22.

kext patches in `/CLOVER/kexts/Other` applied:

* `AppleALC.kext` Native macOS HD audio for not officially supported codecs
* `CodecCommander.kext` For various fixes

`config.plist` patch applied:

* Patch `Devices > PciRoot(0x0)/Pci(0x1f,0x3)`

### Ethernet
[up up up](#)

* EliteDesk-800 has an `Intel® I219LM Gigabit Network Connection LOM` which works with `Fork of Mieze's Intel Mausi Network Driver by RehabMan`.

kext patches in `/CLOVER/kexts/Other` applied:

* `IntelMausiEthernet.kext`

### Graphics
[up up up](#)

* Supported card is `Intel HD Graphics 530` supported with edits in `config.plist`

kext patches in `/CLOVER/kexts/Other` applied:

* `WhateverGreen.kext` Various patches necessary for certain ATI/AMD/Intel/Nvidia GPUs

`config.plist` patch applied:

* Patch `Devices > PciRoot(0x0)/Pci(0x2,0x0)`
* Patch `KernelAndKextPatches > KextsToPatch > HD530 dual monitor patch for HP EliteDesk800 G2 TWR` # for a working two monitor patch

### USB
[up up up](#)

* USB Port Patching uses [[Guide] Creating a Custom SSDT for USBInjectAll.kext](https://www.tonymacx86.com/threads/guide-creating-a-custom-ssdt-for-usbinjectall-kext.211311/), related file is located in `/CLOVER/kexts/Other/USBPorts.kext`.

It it important to note that due to the large number of usb ports, some of them have been disabled:

* USB back, bottom row, 1rst from left
* USB back, bottom row, 2nd from left
* USB back, bottom row, 3rd from left
* USB back, bottom row, 4rth from left

ACPI patches in `/CLOVER/ACPI/patched` applied:

* `SSDT-EC.aml` add EC0 device for usb power injection
* `SSDT-USBX.aml` USB power properties via USBX device
* `SSDT-UIAC.aml` Port Patching

## Changelog
[up up up](#)

You can view [Changelog](Changelog.md) for detailed information.

## Buy me a coffee or a beer
[up up up](#)

If you feel so you can [buy me](http://google.com) a coffee or a beer!

## Credits
[up up up](#)

- Thanks to [Piker-Alpha](https://pikeralpha.wordpress.com/)

- Thanks to [vit9696/Acidanthera](https://github.com/acidanthera) for providing [AppleALC](https://github.com/acidanthera/AppleALC), [CPUFriend](https://github.com/acidanthera/CPUFriend), [HibernationFixup](https://github.com/acidanthera/HibernationFixup), [Lilu](https://github.com/acidanthera/Lilu), `USBPorts`, [VirtualSMC](https://github.com/acidanthera/VirtualSMC), and [WhateverGreen](https://github.com/acidanthera/WhateverGreen).

- Thanks to [alexandred](https://github.com/alexandred) and [hieplpvip](https://github.com/hieplpvip) for providing [VoodooI2C](https://github.com/alexandred/VoodooI2C).

- Thanks to [apianti](https://sourceforge.net/u/apianti), [blackosx](https://sourceforge.net/u/blackosx), [blusseau](https://sourceforge.net/u/blusseau), [dmazar](https://sourceforge.net/u/dmazar), and [slice2009](https://sourceforge.net/u/slice2009) for providing [Clover](https://sourceforge.net/projects/cloverefiboot).

- Thanks to [RehabMan](https://github.com/RehabMan) for providing [EAPD-Codec-Commander](https://github.com/RehabMan/EAPD-Codec-Commander), [OS-X-Clover-Laptop-Config](https://github.com/RehabMan/OS-X-Clover-Laptop-Config), [OS-X-Null-Ethernet](https://github.com/RehabMan/OS-X-Null-Ethernet), [OS-X-ACPI-Battery-Driver](https://github.com/RehabMan/OS-X-ACPI-Battery-Driver), [OS-X-Voodoo-PS2-Controller](https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller), and [SATA-unsupported](https://github.com/RehabMan/hack-tools/tree/master/kexts/SATA-unsupported.kext) and all the amazing help in the forums.

- Thanks to [sniki](https://www.tonymacx86.com/members/sniki.1501160/) and his guide on [[Guide] HP Elite 8300 / HP 6300 Pro using Clover UEFI hotpatch](https://www.tonymacx86.com/threads/guide-hp-elite-8300-hp-6300-pro-using-clover-uefi-hotpatch.265384/)

- Thanks to [edelton](https://www.tonymacx86.com/members/edelton.1923477/) and his guide [HP EliteDesk 800 G2 / HP ProDesk 600 G2 - SUCCESS](https://www.tonymacx86.com/threads/hp-elitedesk-800-g2-hp-prodesk-600-g2-success.261452/)
