# arch-hyprplasma
Arch Linux Install Guide w/ Hyprland and KDE Plasma - 'BTRFS w/ bootable snapshots'
---
Written and implimented by Charles Cravens
---
Sources: Personal experience, and the fabled "ArchWiki" (https://www.archlinux.org/)
---

<div style="text-align:center"></a><img src=".jpg" /></div>

<div style="page-break-after: always;"></a></div>

# <a name="Title"></a> **Arch Linux Install Guide, 2024**

---

Covers Installation and configuration through DE install of KDE Plasma desktop prior to configuring Hyprland.

This particular procedure is geared towards OS installation on my laptop workstation featuring hybrid Intel/Nvidia graphics (ie; Optimus), and all of the challenges and benefits of successfully configuring Linux to run on a system originally designed around a Windows environment. The specific machine referenced is my Lenovo Legion 5-15IMH05H (2020 model), equipped with the Intel Core i7-10750H, Nvidia RTX 2060 GPU, 32GB RAM, and 15.6" FHD 1920x1080 240Hz LCD w/HDR @ 500nits. Storage consists of a 1TB NvMe system drive and an additional 512GB SSD (SATA) mounted at sda1 as a backup data drive formally utilized as a dual-boot environment with Windows 11 Pro. 

---

## <a name="TOC"></a> **Table of Contents**

**[Introduction](#0)**  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[About Arch Linux](#0a)*  

---

**[Initial Setup](#1)**  

---

[Optional Install Script](#1-a)
[Boot the Live Environment](#1a)  
[Set the Keyboard Layout](#1b)  
[Verify the Boot Mode](#1c)  
[Connect to the Internet](#1d)  
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Setup Wi-FI](#1da)*  
[Update the System clock](#1e)  

---

**[Disk Setup](#2)**  

---

[Example Layouts](#2a)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[BIOS with MBR](#2aa)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[UEFI BIOS With GPT](#2ab)*  
[Creating Partitions](#2b)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[Create New Table](#2ba)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[Create Partitions](#2bb)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[Set Partition Type](#2bc)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[!!! Write Changes to Disk !!!](#2bd)*  
[Format the Partitions](#2c)  
[Mount the File Systems](#2d)  

---

**[Installation](#3)**  

---

[Select the Mirrors](#3a)  
[Install Essential Packages](#3b)  

---

**[Configure the System](#4)**  

---

[Fstab](#4a)  
[Chroot](#4b)  
[Timezone](#4c)  
[Set Localization](#4d)  
[Network Configuration](#4e)  
[Initramfs](#4f)  
[Root Password](#4g)  
[Create User & Enable Sudo](#4h)  
[Additional Changes](#4i)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[Enable Multilib](#4ia)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[Optimise Mirrors](#4ib)*  

---

**[Software Installation](#5)**  

---

[Base Packages](#5)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Kernel](#5a0)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Essential](#5a1)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[SCSI](#5a1a)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[File System](#5a2)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[File System Optional](#5a2a)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[CPU](#5a3)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Bootloader](#5a4)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[CLI Text Editors](#5a5)  
[Additional Packages](#5b)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[System Utilities](#5b1)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Networking](#5b2)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[General](#5b2a)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Wi-Fi](#5b2b)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Bluetooth](#5b2c)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Video](#5b3)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[General](#5b3a)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Nvidia](#5b3b)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[AMD](#5b3c)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Sound](#5b4)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Printing](#5b5)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Xorg](#5b6)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Xorg Apps](#5b6a)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Fonts](#5b6b)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Display Managers](#5b7)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[GDM](#5b7a)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[LightDM](#5b7b)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[LXDM](#5b7c)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[SSDM](#5b7d)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[XDM](#5b7e)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Desktop Environments](#5b8)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Budgie](#5b8a)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Cinnamon](#5b8b)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Deepin](#5b8c)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Gnome](#5b8d)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[KDE Plasma](#5b8e)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[MATE](#5b8f)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[XFCE](#5b8g)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Applications](#5b9)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[System](#5b9a)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Office](#5b9b)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Internet](#5b9c)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Multimedia](#5b9d)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Codecs](#5b9da)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Photo / Image Editing](#5b9e)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*[Misc](#5b9f)*  
[Boot Loader](#5c)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[MBR](#5ca)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[UEFI](#5cb)*  
[Microcode](#5d)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[GRUB](#5da)*  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *[rEFInd](#5db)*  
[Reboot](#5e)  

---
