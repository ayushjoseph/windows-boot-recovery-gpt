🚨 Recovering a Windows PC After Full GPT Corruption

A real-world system failure → investigation → recovery → optimization case study

📌 Overview

This project documents a real system breakdown where a partition-level operation corrupted the GPT partition table, wiped all partitions, removed the EFI boot manager, and pushed the system into PXE network boot.

The laptop was completely unbootable.

Instead of sending the machine for service, I diagnosed the problem at the storage and firmware level, rebuilt the disk manually, reinstalled Windows, fixed driver failures, and optimized the system for development work.

This repo serves as:

A real-world troubleshooting example

A learning resource for OS internals

A practical system recovery guide

A demonstration of recovery-level thinking

💥 What Went Wrong

A disk utility rebuild operation overwrote a corrupted GPT header.

Instant result:

All partitions disappeared

EFI boot manager deleted

Recovery environment destroyed

Windows unreachable



🔍 Investigation
BIOS Result

SSD was detected but had no boot entry.

DiskPart Output
diskpart
list disk
select disk 0
list part


Output:

No partitions found
Disk shows full unallocated space


The disk still existed.
The partition table did not.

🔧 Recovery Steps Performed
1. Wipe corrupted GPT metadata
diskpart
select disk 0
clean
convert gpt

2. Recreate Windows boot structure manually

EFI (Boot Partition):

create partition efi size=100
format fs=fat32 quick label="System"
assign letter=S


Microsoft Reserved:

create partition msr size=16


Windows partition:

create partition primary
format fs=ntfs quick label="Windows"
assign letter=W

3. Install Windows

Custom installation → target rebuilt primary partition.

4. Fix Windows “No Internet” Setup Block

During OOBE stage:

OOBE\BYPASSNRO


Allowed offline setup and completed install.

⚙️ Post-install Setup
Driver Stack (critical order)

AMD Radeon

NVIDIA RTX 3050

System tuning

Lenovo Vantage updates

Windows Update

Power mode optimization

💾 Storage Architecture Optimization

Repartitioned SSD for development workflow:

Drive	Purpose
C: (250GB)	OS + tools + WSL
D: (226GB)	Projects & data

Moved downloads, documents, desktop, and media to D:.

🧠 What This Taught Me

✔ GPT corruption causes total OS loss
✔ PXE boot often means bootloader missing, not hardware
✔ DiskPart is the safest recovery tool
✔ Partitioning improves long-term maintainability
✔ Driver install order matters on hybrid GPU systems
✔ Troubleshooting is logic, not guessing

🛠️ Tools Used

Windows Recovery Environment

DiskPart

BIOS / UEFI

Lenovo Vantage

AMD & NVIDIA Drivers

Windows Installer

Native CLI utilities

🎯 Final Result

✅ Fully restored system
✅ Clean OS installation
✅ Optimized for development
✅ Dual GPU enabled
✅ Correct boot architecture
✅ Professional system layout

Machine rebuilt from raw disk to working system.



BIOS falling back to PXE network boot

Windows installer unable to detect SSD
