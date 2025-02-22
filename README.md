# Arch Linux Installation Guide
This is an guide to intsall arch (minimal)
I-Preparing installation medium
  1.Download arch linux iso file from https://archlinux.org/download/ (Find HTTP Direct Downloads section and choose mirror that is geographically closer to your location)
  2.Insert a usb stick into your pc (at least 5GB)
  3.Use a image flasher to flash the arch iso file to your usb stick (I reccomend you should use ventoy)
  #last steps are for ventoy user
  4.Download ventoy from https://www.ventoy.net/en/download.html and install ventoy
  5.Move the arch iso to the drive named ventoy
II-Starting the installation
  1.Reboot your computer and boot into ventoy
  2.Select the arch iso
  3.Wait until the boot process is finished
  4.Connect to wifi using iwctl #if you use ethernet, you can skip this steps
    $ iwctl
 [iwd]# station wlan0 get-networks
 [iwd]# station wlan0 connect <Name of WiFi access point>
 [iwd]# exit
  5.Check connection is established
    $ ping 1.1.1.1
  6.
