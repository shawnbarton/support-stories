# Case 0001
# Supermicro X10DRU-i+ and AOC-SLG3-2M2 Compatibility
*AKA: Cannot boot from NVMe drives installed in AOC-SLG3-2M2 on an X10DRU-i+ mainbaord*

## Issue description:

I purchased an AOC-SLG3-2M2 so that I could have NVMe boot and OS drives in this server. The motherboard (X10DRU-i+) supports PCI-e bifurcation and is listed on the add-in card's "Validated Platforms" list. 

After installation of the card I applied the "correct" BIOS settings and I was able to see the drives in the OS (or OS installer) but I was not able to boot from the drives.

## Server Hardware:

- Chasis & Motherboard: Supermicro CSE-829U X10DRU-i+ 2U 12x 3.5" (LFF)
    - Motherboard specs [here](https://www.supermicro.com/en/products/motherboard/X10DRU-i+)
- Processor(s): 2 x Intel Xeon E5-2660 V4 - 14-Core 28-Threads 2.00GHz (3.20GHz Boost, 35MB Cache, 105W TDP)
- Heatsink(s):  2 x SuperMicro X10DRi LGA-2011 2U Heatsink
- Memory (RAM): 4 x 32GB - DDR4 2400MHz (PC4-19200, 2Rx4)
- RAID: Adaptec 71605 FH 1GB
- Backplane: BPN-SAS3-826EL1
- Network: Broadcom BCM57810S Dual Port - 10GbE SFP+ Low Profile PCIe-x8 CNA 
- GPU: PNY nVidia RTX A2000 6GB
- Power Supply(s): 2 x SuperMicro (PWS-1K02A-1R) 1U 1000W Titanium Hot-Swap PSU

## Problematic Hardware:

- Model: Supermicro AOC-SLG3-2M2
- Product overview: [specifications at supermicro.com](https://www.supermicro.com/en/products/accessories/addon/AOC-SLG3-2M2.php) or [waybackmachine at archive.org](https://web.archive.org/web/20230128074158/https://www.supermicro.com/en/products/accessories/addon/AOC-SLG3-2M2.php)
- NVMe Drives: 2 x Samsung SSD 970 EVO Plus 1TB

## Resolution:

### Summary:

Supermicro support states that the add-in card being "validated" means nothing more than "the card works" - NOT that all features are available. In this case, the missing feature is one of being able to boot from the device. This is the official "business decision" and will likely not change in the future.

I was able to add NVMe drivers to the latest BIOS. Afterwards, I was able to boot from the NVMe drives on the add-in card. They now work as expected.

You can either roll your own BIOS modification, as described below, or use my modified file.

**This modified BIOS file is provided solely as a convinience for you. I will provide no warranty for it or any resulting issues. You are choosing to use this file entirely AT YOUR OWN RISK.**

### Steps:

1. I ensured all firmware in the system was as up-to-date including BMC IPMI. I was already running the latest BIOS (v3.5)
    - As of writing, the latest BMC version was `File Name:BMC_X10AST2400-C001MS_20211001_03.94_STD.zip, Revision:03.94`
2. Noted down current BIOS settings
2. I found [this article](https://winraid.level1techs.com/t/howto-get-full-nvme-support-for-all-systems-with-an-ami-uefi-bios/30901) describing in detail how to add the NVMe driver to AMI UEFI BIOS
    - As of writing, the current NVMe driver was `NvmExpressDxe_5.ffs, dated 09/20/2021`
    - I read the flashing experience for this specific board model from the user *Wishbringer* as described [here](https://winraid.level1techs.com/t/guide-how-to-flash-a-modded-ami-uefi-bios/30627/246)
3. I downloaded the [latest available BIOS](https://www.supermicro.com/en/support/resources/downloadcenter/firmware/MBD-X10DRU-i+/BIOS) from Supermicro
    - `File Name: X10DRU2.427.zip Revision:3.5`
4. I opened the UEFI version of the BIOS file from the above archive in UEFITool 0.28 and added the NVMe driver to the BIOS DXE Volume
    - I chose not to use the newer version of UEFITool, also called NE (for New Engine), as it was still in an immature development phase
5. I flashed the modified BIOS file using the server's BMC web interface
    - Maintenance > BIOS Update
    - Chose File (navigated to the modified `X10DRU2.427`)
    - Clicked "Upload BIOS"
    - Once the upload completed, I was presented with some check boxes:
        - Preserve ME Region (left unchecked)
        - Preverve NVRAM (left unchecked)
        - Preserve SMBIOS (checked and greyed out)
    - Clicked "Start Upgrade"
6. The upgrade process compeleted without issue
7. I booted into BIOS setup and put my old settings back
    - I did not retain an exhaustive list of my BIOS settings so I cannot rule out whether they are needed to have a successful boot process. Important to check is:
        - Make sure the PCI-e port is correctly bifurcated to a x4x4(x4x4/x8) mode
        - Make sure the OPROM is set to EFI (instead of Legacy)
        - Make sure the Boot Mode is EFI (or at least DUAL)
        - Save and Reset
8. Reinstalled OS on NVMe drive by booting from a USB installer in UEFI mode
    - Ubuntu 22.04
9. The OS on the NVMe drive is visible in the BIOS and is bootable