 ---
author: "Koushik"
date: 2020-05-28
#linktitle: MacOS on Hackintosh PC
title: Guide to Install MacOS Mojave on a Hackintosh PC
weight: 10
comments: true
readingtime: true
tags: ["macOS", "Hackintosh", "Mojave"]
categories: ["Hackintosh"]
---

This is a quite comprehensive post on how I installed macOS Mojave on my PC. I have listed down the hardware specifications of my Hackintosh followed by the entire process I went through to install macOS on my PC. Good luck!

#### Specifications


| Unit          | Component     |
|:-------------:|:-------------:|
| Motherboard   | Asus Z170 Gaming Pro |
| RAM     | Corsair Vengeance 8GB LPX DDR4 |
| iGPU | Intel HD530   |
| PSU  | Corsair VS550 - 550W |
| Processor | Intel Core-i5 6600k (Skylake) |
| HDD/SSD  | Kingston 120GB SSD + 1TB Seagate HDD |
| Case | Corsair Spec-01 (ATX) Mid Tower Cabinet |

#### Links I followed for vanilla install:

1. [https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/)
2. [https://hackintosher.com/guides/guide-to-fresh-installing-macos-mojave-on-a-hackintosh-10-14/](https://hackintosher.com/guides/guide-to-fresh-installing-macos-mojave-on-a-hackintosh-10-14/)
3. [https://www.youtube.com/watch?v=fA9AotXqkqA](https://www.youtube.com/watch?v=fA9AotXqkqA)


## Making a Bootable USB

### Step 1: Downloading the macOS

This requires a minimum of 16GB USB drive. The entire process of creating a bootable USB becomes simpler with another iMac or MacBook. Otherwise, use this **[link](https://internet-install.gitbook.io/macos-internet-install/preparing-your-installer/preparing-your-installer-media)** to create a bootable USB without an iMac or a Macbook.

- Download the Apple’s official macOS Mojave installer from the App Store.
- After downloading plug the USB drive in and format it using disk utility. Make sure to choose GUID Partition Map in the scheme. Leave the format as Mac OS Extended (Journaled). Click on Erase.
- Open terminal and paste the command shown below and follow the instructions on terminal. This will take up to 30 minutes.

```bash
sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```

### Step 2: **Installing the bootloader**

- Download the Clover EFI bootloader **[here](https://sourceforge.net/projects/cloverefiboot/)** and install it into the USB installer disk's EFI Boot Partition.
- Before proceeding to install, click on customize and complete the following.
    - ***CHECK*** Clover for UEFI booting utility
    - ***CHECK*** Install clover in the ESP
    - Under UEFI Drivers
        - Recommended Drivers
            - ***UNCHECK*** AudioDxe
            - ***CHECK*** DataHubDxe
            - ***CHECK*** FSInject
            - ***CHECK*** SMCHelper
        - File System Drivers
            - ***CHECK*** ApfsDriverLoader
            - ***CHECK*** VBoxHfs
        - FireVault 2 UEFI Drivers
            - ***CHECK*** AppleImageCodec
            - ***CHECK*** AppleKeyAggregator
            - ***CHECK*** AppleKeyFeeder
            - ***CHECK*** AppleUITheme
            - ***CHECK*** FirmwareVolume
        - Additional Drivers
            - ***CHECK*** PartitionDxe

Click on install and wait until the installation is complete. Now the final step is to add *kernel extension files* (kexts) to the EFI partition in the USB installer device.

### Step 3: Adding config.plist for Skylake

- Download the **[Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/)** and mount the EFI of the USB installer.
- Open it and mount the EFI partition of the USB installer if it isn't mounted already. Navigate to EFI → CLOVER and open the config.plist file with the Clover Configurator.
- Go the ***Kexts Installer*** section and click on the ***OS version*** drop-down in the top right corner and select ***Others***.
- Select ***Lilu, WhateverGreen, FakeSMC, IntelMausiEthernet, USBInjectAll*** and download. While downloading the kexts, a drop-down will appear. Check all the sensors and click on install.
- Quit Clover Configurator and delete the config.plist file.
- The config.plist file can be configured manually depending on the hardware but I downloaded a preconfigured config.plist file from **[this](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/)** website. Download the config.plist and paste it in the same folder as it was before.
- Open the config.plist file using Clover Configurator and go to the ***SMBIOS*** section, select the relevant model based on the hardware by clicking on the button in the right edge of the window and let it fill all the details in all the required fields.
- Make sure the *verbose* argument is checked in the ***Boot*** section and close the configurator. Now the USB installer is ready.

## Installing macOS Mojave

### Step 1: BIOS settings

I use ASUS Z170 Pro Gaming motherboard and the BIOS page is a bit different from other motherboards. Make sure the BIOS is up-to date and load optimized defaults.

- ***Disable*** the serial port in the ***Serial Port Configuration***
- ***Disable*** Fastboot and CSM Support
- Set the SATA Mode Selection to ***AHCI***
- Set ***XMP*** profile for memory
- ***Disable*** Intel Virtualization Technology
- ***Enable*** VT-d
- Now for graphics settings, if a dedicated GPU is used, change the Primary Display setting to ***PCIE*** and ***enable*** iGPU Multi-Monitor. If there's no dedicated GPU, then change the Primary Display setting to ***CPU Graphics*** and ***disable*** iGPU Multi-Monitor.
- Set DVMT Pre-Allocated memory to ***64M***
- ***Enable*** IOAPIC 24-119 Entries
- ***Enable*** Legacy USB Support
- ***Enable*** XHCI Handoff
- Set Secure Boot OS Type to ***Other OS***

Save these settings and plug in the USB Installer device. Boot from **Install macOS Mojave** partition. Since we enabled the verbose argument, it'll display all the debug messages. After booting, go to disk utilities and choose the disk for installing the macOS and select GUID Partition Map. Click on Erase, wait until the process is over. Close disk utilities and select Install macOS, click on continue and follow the on-screen instructions.

- The installation will reboot the machine twice and make sure to choose the Mojave installation disk and not the USB installer while rebooting.

### Step 2: Post-installation

- Now the machine can boot only with the USB plugged into the system. The bootloader has to be copied to the Mojave disk in order to boot without the USB.
- Install Clover in the Hackintosh now and configure it exactly same as how it was done for the USB installer device.
- Open the EFI partition of the Mojave disk and delete the EFI folder. Copy the EFI folder from the USB installer to the Mojave EFI partition disk and now the Hackintosh should be able to boot without the USB installer device. Congratulations!

---

## Some issues I ran into

### Issue 1

Pre-installation boot issue

```
Start FinalizeBootStruct
Start RandomSeed
End RandomSeed
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```

This was due to the memory misconfiguration and enabling AptioMemoryFix in the partition, Kernel Support CPU Lapic, Kernel PM and Apple RTC Patch resolved this issue.

### Issue 2

Pre-installation boot issue 

```
apfs_module_start:1689: load: com.apple.filesystems.apfs. ... (dd/mm/yyyy)
```

Resolved this issue by putting **[SSDT-EC.aml](https://www.tonymacx86.com/threads/guide-usb-power-property-injection-for-sierra-and-later.222266/page-55#post-1728645)**  in /EFI/CLOVER/ACPI/patched.

### Issue 3

Pre-installation boot issue 

```
Still waiting for root device
```

This error was due to the USB3 port I was using to install. Plugging the USB drive into the USB2 port resolved the issue.

### Issue 4

Post-installation issue: Audio did not work

- This was solved by using **[this](https://hackintosher.com/guides/get-hackintosh-audio-working/)** guide.
- My audio codec - Asus S1220A.

## Existing issues

PC won't wake after putting it in sleep mode. This is a very minor issue and is due to the integrated graphics HD 530. Can be solved by adding a dedicated GPU. Only cheap work-around is to disable the sleep mode. 

---

Everything else works like a charm!

![First successful installation of Hackintosh (macOS Mojave)](/images/pic3.jpg)
*First successful installation of macOS Mojave on a Hackintosh*

