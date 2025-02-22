# Arch Linux Installation Guide
This is an guide to intsall arch (minimal)
## I-Preparing installation medium
 #### 1.Download arch linux iso file from https://archlinux.org/download/ (Find HTTP Direct Downloads section and choose mirror that is geographically closer to your location)
 #### 2.Insert a usb stick into your pc (at least 5GB)
 #### 3.Use a image flasher to flash the arch iso file to your usb stick (I reccomend you should use ventoy)
  ### last steps are for ventoy user
 #### 4.Download ventoy from https://www.ventoy.net/en/download.html and install ventoy
 #### 5.Move the arch iso to the drive named ventoy
## II-Starting the installation
 #### 1.Reboot your computer and boot into ventoy
 #### 2.Select the arch iso
 #### 3.Wait until the boot process is finished
 #### 4.Connect to wifi using iwctl   (if you use ethernet, you can skip this steps)
    $ iwctl
    $ [iwd]# station wlan0 get-networks
    $ [iwd]# station wlan0 connect <Name of WiFi access point>
    $ [iwd]# exit
 #### 5.Check connection is established
    $ ping 1.1.1.1
 #### 6.Disk partitioning 
  ##### - Partion main storage device using 'fdisk' untility or you can just use 'cfdisk' (easier to partion). You can find storage device name using 'lsblk'
    $ fdisk /dev/nvme0n1             
  *# you can change it to /dev/sdaX if you don't use m2 ssd*
  
                 [repeat this command until existing partitions are deleted]
    Command (m for help): d
    Command (m for help): d
    Command (m for help): d

                [create partition 1: efi]
    Command (m for help): n
    Partition number (1-128, default 1): Enter ↵
    First sector (..., default 2048): Enter ↵
    Last sector ...: +100M

                 [create partition 3: swap]
    Command (m for help): n
    Partition number (3-128, default 3): Enter ↵
    First sector (..., default ...): Enter ↵
    Last sector ...:+1G Enter ↵
    
                    [create partition 2: main]
    Command (m for help): n
    Partition number (2-128, default 2): Enter ↵
    First sector (..., default ...): Enter ↵
    Last sector ...: Enter ↵
                [change partition types]
    Command (m for help): t
    Partition number (1-3, default 1): 1
    Partion type or alias (type L to list all): uefi
    Command (m for help): t
    Partition number (1-3, default 2): 2
    Partion type or alias (type L to list all): linux
    Command (m for help): t
    Partition number (1-3, default 3): 3
    Partion type or alias (type L to list all): swap

                [write partitioning to disk]
    Command (m for help): 
 ##### - Create filesystems on created disk partitions:
_EFI System partition_
 
    $ mkfs.fat -F 32 /dev/nvme0n1p1
_Linux filesystem partition_
    
    $ mkfs -t ext4 /dev/nvme0n1p2  
_Linux swap partition_

    $ mkswap /dev/nvme0n1p3          
 
