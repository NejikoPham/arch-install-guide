# Arch Linux Installation Guide
This is an guide to intsall arch (minimal)
## I-Preparing installation medium
 #### 1.Download arch linux iso file from https://archlinux.org/download/ _(Find HTTP Direct Downloads section and choose mirror that is geographically closer to your location)_
 #### 2.Insert a usb stick into your pc _(at least 5GB)_
 #### 3.Use a image flasher to flash the arch iso file to your usb stick _(I reccomend you should use ventoy)_
 ### **_# last steps are for ventoy user_**
 #### 4.Download ventoy from https://www.ventoy.net/en/download.html and install ventoy
 #### 5.Move the arch iso to the drive named ventoy
## II-Starting the installation
 #### 1.Reboot your computer and boot into ventoy
 #### 2.Select the arch iso
 #### 3.Wait until the boot process is finished
 #### 4.Connect to wifi using iwctl   _(if you use ethernet, you can skip this steps)_
    $ iwctl
    $ [iwd]# station wlan0 get-networks
    $ [iwd]# station wlan0 connect <Name of WiFi access point>
    $ [iwd]# exit
 #### 5.Check connection is established
    $ ping 1.1.1.1
 #### 6.Disk partitioning 
 ##### - Partion main storage device using 'fdisk' untility or you can just use 'cfdisk' (easier to partion). You can find storage device name using 'lsblk'
    $ fdisk /dev/nvme0n1             
  *# you can change it to /dev/sda if you don't use m2 ssd*
  
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
**_EFI System partition_**
 
    $ mkfs.fat -F 32 /dev/nvme0n1p1
**_Linux filesystem partition_**
    
    $ mkfs -t ext4 /dev/nvme0n1p2  
**_Linux swap partition_**

    $ mkswap /dev/nvme0n1p3          
 ##### - Correctly mount all filesystems to the `/mnt`:
    $ mount /dev/nvme0n1p2 /mnt
    $ mkdir -p /mnt/boot/efi
    $ mount /dev/nvme0n1p1 /mnt/boot/efi
    $ swapon /dev/nvme0n1p3
 ##### - Install essential packages into new filesystem and generate fstab:
    $ pacstrap -i /mnt base linux linux-firmware sudo vim 
    $ genfstab -U -p /mnt > /mnt/etc/fstab
 #### 7.Basic configuration of new system
 ##### - Create filesystems on created disk partitions:
    $ arch-chroot /mnt
 ##### - Setup system locale and timezone, sync hardware clock with system clock:
    $ ln -sf /usr/share/zoneinfo/Region/City /etc/localtime # choose your timezone
    $ hwclock --systohc
    $ vim /etc/locale.gen                                   # uncomment your locales
    $ locale-gen
    $ echo "LANG=en_US.UTF-8" > /etc/locale.conf            # choose your locale
 ##### - Setup system hostname:
    $ echo yourhostname > /etc/hostname
    $ vim /etc/hosts
        127.0.0.1 localhost
        ::1       localhost
        127.0.1.1 yourhostname
 ##### - Add new users and setup password:
     $ useradd -m -G wheel,storage,power,audio,video -s /bin/bash yourusername 
     $ passwd yourusername
 ##### - Add wheel group to sudoers file to allow users to run sudo:
     $ visudo
        [uncomment following line in file]
        %wheel ALL=(ALL) ALL
 ##### - Install and configure GRUB:
     $ pacman -S grub efibootmgr
     $ grub-install /dev/nvme0n1
     $ grub-mkconfig -o /boot/grub/grub.cfg
 ##### - Setup network:
     $ pacman -S dhcpcd networkmanager resolvconf
     $ systemctl enable dhcpcd
     $ systemctl enable NetworkManager
     $ systemctl enable systemd-resolved
 ##### - Exit chroot, unmount all disks and reboot:
     $ exit
     $ umount /mnt/boot/efi
     $ umount /mnt
     $ reboot
## III-Configuring userspace after initial system setup
 #### 1.Basic configuration of userspace:
     $ timedatectl set-ntp true
 #### 2.[Wifi users only] Connect to WiFi using nmcli:
     $ nmcli device wifi connect <Name of WiFi access point> password <password>
    
 
