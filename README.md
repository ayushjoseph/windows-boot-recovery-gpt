🚨 Recovering a Windows PC After Full GPT Corruption

A real-world system failure → investigation → recovery → optimization case study

📌 Overview

This project documents a real system breakdown where a partition-level disk operation corrupted the GPT partition table, removed all partitions, deleted the EFI boot manager, and forced the system into PXE network boot.

The laptop became completely unbootable.

Instead of sending the machine for service, I diagnosed the issue at the storage and firmware level, manually rebuilt the disk layout, reinstalled Windows, resolved driver failures, and optimized the system for development and daily use.

This repository serves as:

A real-world troubleshooting case study

A learning resource for OS internals

A practical Windows recovery reference

A demonstration of recovery-level system thinking

💥 What Went Wrong

A disk utility rebuild operation overwrote a corrupted GPT header.

Immediate impact:

All partitions disappeared

EFI boot manager deleted

Recovery environment destroyed

Windows unreachable

System forced into PXE network boot

🖥️ System Information

Laptop: Lenovo IdeaPad Gaming 3

CPU: AMD Ryzen (with Radeon Graphics)

GPU: NVIDIA GeForce RTX 3050 + AMD Radeon iGPU

Storage: NVMe SSD (GPT)

OS: Windows 11 Home Single Language

Boot Mode: UEFI

❌ Initial Problem

System failed to boot

BIOS showed No Boot Device Found

SSD detected but partitions missing

Windows installer could not detect an OS

GPT partition table corrupted

🔍 Symptoms Observed

Empty disk view in Windows Installer

Invalid partition layout in disk utilities

No EFI boot entry present

Recovery environment partially broken

🛠️ Recovery Approach (High-Level)

Booted into Windows Recovery Environment (WinRE)

Inspected disk state using DiskPart

Removed corrupted partition metadata

Rebuilt GPT layout manually

Created EFI, MSR, and Primary partitions

Reinstalled Windows 11

Restored UEFI boot functionality

Performed post-install cleanup and optimization

🧰 Tools Used

Windows Recovery Environment (WinRE)

DiskPart

Windows 11 USB Installer

BIOS Boot Menu

Disk Management

Command Prompt (Administrator)

⌨️ Key Commands Used
Disk Inspection
diskpart
list disk
select disk 0
list partition

Cleaning Corrupted GPT
select disk 0
clean
convert gpt

Creating Required Partitions
create partition efi size=100
format fs=fat32 quick
assign letter=S

create partition msr size=16

create partition primary
format fs=ntfs quick
assign letter=C

Exit DiskPart
exit

🪟 Windows Installation

Installed Windows 11 on the newly created primary partition

Windows automatically created required system and recovery structures

Skipped mandatory network requirement using OOBE bypass

Completed local account setup

⚙️ Post-Installation Steps

Verified disk layout using Disk Management

Created a separate Data (D:) partition

Moved user folders:

Desktop

Downloads

Documents

Pictures

Videos

Installed GPU drivers:

NVIDIA GeForce Experience (RTX 3050)

AMD Radeon Software

Verified Windows activation status

⚠️ Limitations

OEM recovery partition (Lenovo OneKey Recovery) is missing

Factory reset via Lenovo tools is unavailable

Windows reset via Settings still functions normally

📚 Learning Outcomes

Deep understanding of GPT & UEFI boot flow

Manual disk recovery techniques

Windows boot architecture (EFI → Boot Manager → OS)

Safe partitioning strategies

Real-world troubleshooting under failure conditions

✅ Final Status

✔ System fully functional

✔ Windows activated

✔ Dual GPU drivers installed

✔ Optimized disk layout for development & daily use

👤 Author

Ayush Joseph
