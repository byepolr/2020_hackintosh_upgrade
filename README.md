# 2020_hackintosh_upgrade
Installing macOS Catalina 10.15.5 on Gigabyte Z370N WIFI + i5 9600K + UHD 630

# How create create the install flash drive

## Prepair flash drive
Format the flash drive with GUID partition mapping and name it "Install".

1. Open Disk Utility.
2. Click in View, then "Show All Devices".
3. Select the destination flash drive, then Erase.
4. Name: Install, Format: Mac OS Extended (Journaled) and Scheme: GUID Partition Map

## Create install flash drive
Download 'UniBeast 10.3.0 - Catalina'
- https://www.tonymacx86.com/resources/unibeast-10-3-0-catalina.490/n

Run applications to create bootable flash drive

## Install Bootloader
1. Download the Clover Configurator app (See link in sources). Also copy this to the flash drive for later
2. Open app and mount the EFI drive on the install USB
3. Replace the EFI folder with the one in this repository
4. Unmount the EFI drive

## Make it unique for your machine
1. Open the Clover Configuration
3. Mount the EFI drive on the install USB
4. Goto SMBIOUS and generate new *Serial Number* and *SmUUID*
5. Unmount EFI drive


# UEFI Setup Configuration and Installation

##UEFI configuration
My BIOS is F10 (I use an 8th generation processor, so I don't need to upgrade)
- Shutdown and insert the flash.
- Power on with DEL pressed to enter BIOS setup.
- Change value: BIOS - CSM Support to Disabled
- Change value: BIOS - Secure Boot to Disabled
- Change value: Peripherals - Initial Display to IGP
- Change value: Chipset - VT-d to DISABLE
- Change value: Chipset - Internal Graphics to ENABLE
- Save & Exit Setup

## Installation
### Boot
- Now, hold the F12
- In the list, select your flash drive.
- Choose the flash drive (white icon).

The installer will start.

### Format destination disk
Format the disk with GUID partition mapping and name it "Macintosh HD".

- Select Disk Utility in the list, then press Continue.
- Click in View, then "Show All Devices".
- Select the destination Disk, then Erase.
- Name: HD, Format: APFS and Scheme: GUID Partition Map

Close Disk Utility.
Select "Install macOS", then Continue.
Follow instructions in screen.

After first part install the system will reboot. Maybe you have to press ESC again and select the flash drive.
Choose the internal disk (gray hdd icon) if not already selected.
The second part of the installation will begin. This will take 16 minutes


## Post installation
Open the Clover Configurator from flash drive that you copied before.

Mount and open the flash drive EFI partition. (Inside Clover Configurator, Mount EFI -> Select flash drive in Efi Partitions list -> Mount Partition -> Open Partition)
Copy the EFI folder to desktop.

Go back to Clover Configurator, unmount the EFI partition of the flash drive and mount and open the EFI partition of internal disk.

Move the EFI folder from Desktop to EFI partition of the internal disk.

Eject the flash drive.


Move all Kexts from EFI partition of internal disk to /Library/Extensions and make kext cache, with those commands in Terminal:
```
sudo mv -R /Volumes/EFI/EFI/CLOVER/kexts/10.15/* /Library/Extensions/
sudo chmod -R 755 /Library/Extensions
sudo chown -R root:wheel /Library/Extensions
sudo kextcache -i /
```

Restart the system.

Run this command in Terminal to disable hibernation (doesn't work in macOS):
```
sudo pmset -a hibernatemode 0
```

# Sources
https://mackie100projects.altervista.org/download-clover-configurator/
https://www.insanelymac.com/forum/topic/341346-guide-gigabyte-ga-z370n-wifi-i7-8700k-uhd-630-catalina-10155/
https://www.youtube.com/watch?v=paEke_oobZI&t=132s
https://github.com/acidanthera/WhateverGreen
