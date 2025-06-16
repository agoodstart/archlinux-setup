# Laptop setup with Arch Linux
- - - -
Sources: 
* https://www.youtube.com/watch?v=kXqk91R4RwU&t
* https://github.com/IvnLum/Arch-Linux-Crypt-Install/blob/main/cryptinst.sh
* https://gist.github.com/dante-robinson/fdc55726991d3f17e0dbef1701d343ef
* https://bbs.archlinux.org/viewtopic.php?id=261327
* https://wiki.archlinux.org/title/Persistent_block_device_naming
* https://www.youtube.com/watch?v=H_g1sf1PWt0
* https://www.youtube.com/watch?v=fFxWuYui2LI

I am in the process of building a new machine with arch linux.
The target machine is a dell xps 15 9500 laptop with the following specs:
• 15.6" UHD+ (3840 x 2400) InfinityEdge Touch AntiReflective 500-Nit Display 
• 32GB DDR4-2933MHz, 2x16G
• 1TB M.2 PCIe NVMe Solid State Drive
• NVIDIA(R) GeForce(R) GTX 1650 Ti 4GB GDDR6
• Killer Wi-fi 6 AX500-DBS (2x2) and Bluetooth 5.0 

The goal of this laptop is for personal use, as well as local development, as I want to practise heavily with docker and kubernetes. Therefore, I want to reserve appropriate space for docker and kubernetes usage.
Aside from docker and kubernetes, I want to practise with golang and rust (building gRPC schema's with clean code), either on the linux root, or in their dedicated docker containers to ensure isolation and separation of concerns.
Also, a couple of vms are required for testing purposes. One windows 11 vm is necessary to ensure crossfunctional capabilities when writing applications.
After careful consideration and research, I want to partition the SSD into the following:

/dev/nvme0n1p1 - 512MB - EFI System - not encrypted - FAT32 formatted - for UEFI Boot
/dev/nvme0n1p2 - 1GB - Linux filesystem - not encrypted - ext4 formatted - for the bootloader files (/boot)
/dev/nvme0n1p3 - 220GB - linux root x86-64 - LUKS encrypted - BTRFS formatted - for default mount + wayland/hyprland / userspace
/dev/nvme0n1p4 - 120GB - Microsoft basic data - LUKS encrypted - NTFS formatted -  for one dedicated windows 11 virtual machine, starting at 80GB with possibility to extend to 120GB max
/dev/nvme0n1p5 - 150GB - linux LVM - LUKS encrypted - ext4 formatted - for multiple virtual linux machines
/dev/nvme0n1p6 - 450GB - Linux filesystem - LUKS encrypted - xfs formatted (mkfs.xfs -n ftype=1) - for docker local and kubernetes
/dev/nvme0n1p7 - 10GB - linux swap - LUKS encrypted - initialize with makeswap - for zram backup - used during hibernation or zram overload


/dev/nvme0n1p3 should be the partition that will use applications like vscode, vim, virtlib+qemu, maybe mail applications, browsers. I think 220GB would be enough to support this. After configuring zram and installing the arch linux system, it would probably have 150GB left
/dev/nvme0n1p5 should be big enough to support multiple logical volumes
/dev/nvme0n1p6 for heavy use of docker / kubernetes, can ask a lot of resources, therefore 450GB should be reasonable.
