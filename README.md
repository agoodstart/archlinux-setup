# Laptop setup with Arch Linux (WIP) #

### TOC ###

## Machine Specs
I am in the process of building a new machine with Arch Linux.
The target machine is a Dell XPS 15 9500 with the following specs:
* 15.6" UHD+ (3840 x 2400) InfinityEdge Touch AntiReflective 500-Nit Display
* 32GB DDR4-2933MHz, 2x16G
* 1TB M.2 PCIe NVMe Solid State Drive
* NVIDIA(R) GeForce(R) GTX 1650 Ti 4GB GDDR6
* Killer Wi-fi 6 AX500-DBS (2x2) and Bluetooth 5.0 

The goal of this setup is for personal use, as well as local development, as I want to practise heavily with Docker and Kubernetes. 

Aside from docker and kubernetes, I want to practise with Golang and Rust, either in root, or in their dedicated Docker containers to ensure isolation.
Also, a couple of VMs are required for testing purposes. One windows 11 vm is necessary to ensure crossfunctional capabilities when writing applications.

## Pre setup

### Connect to internet
`iwctl`
```
station wlan0 scan
station wlan0 get-networks
station wlan0 connect <SSID>
```

### SSH into Arch Linux

Get IPv4 address of the laptop:

`ip -4 -o a show eth0 | awk '{print $4}' | cut -d/ -f1`

Check if SSH daemon is running:

`systemctl status sshd`

if not, turn on:

`systemctl start sshd`

Set password of root (or go for passwordless login, but I would configure that post installation)

`passwd root`

From another machine, SSH into the laptop:

`ssh root@<IPv4 address>`

Because the identity of the laptop will change, it is better to not save the SSH connection in known_hosts,
or add the key to authorized_keys:

`ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null root@<IPv4 address>`

## Partitioning
### Partition Table
I want to partition the SSD into the following and will be added to fstab:

Partition       | Size   | Mount Point  | Partition Type       | Filesystem  | Encryption  | Purpose
:---            | :---   | :---         | :---                 | :---        | :---        | :---
/dev/nvme0n1p1  | 512MB  | /boot/efi    | EFI System           | FAT32       | No          | EFI Partition for UEFI Boot
/dev/nvme0n1p2  | 2GB    | /boot        | Linux Filesystem     | ext4        | No          | Bootloader files
/dev/nvme0n1p3  | 50GB   | -            | Linux Swap           | swap        | Yes         | Hibernation and zram overflow
/dev/nvme0n1p4  | 150GB  | /            | Linux Root (x86-64)  | btrfs       | Yes         | Root with Wayland/Hyprland

I want to add the following settings too, but that would be post-installation and will be :
Partition       | Size   | Mount Point  | Partition Type       | Filesystem  | Encryption  | Purpose
:---            | :---   | :---         | :---                 | :---        | :---        | :---
/dev/nvme0n1p5  | 350GB  | /con         | Linux Filesystem     | XFS         | Yes         | Docker and Kubernetes
/dev/nvme0n1p6  | 200GB  | /virt_lin    | Linux LVM            | LVM         | Yes         | Linux VMs
/dev/nvme0n1p7  | 120GB  | /virt_win    | Linux Filesystem     | ext4        | Yes         | Windows 11 VM
/dev/nvme0n1p8  | 25GB   | -            | -                    | -           | -           | Reserved space

### Formatting

Format with fdisk:

`fdisk /dev/nvme0n1`



## Sources: 

### Benchmarking
* https://www.assyoma.it/single-post/2015/02/02/zfs-btrfs-xfs-ext4-and-lvm-with-kvm-a-storage-performance-comparison
* https://gist.github.com/braindevices/fde49c6a8f6b9aaf563fb977562aafec
* https://forum.endeavouros.com/t/considering-some-btrfs-optimizations/27025

### Others
* https://www.youtube.com/watch?v=YC7NMbl4goo
* https://www.youtube.com/watch?v=c5KkTZ1HbHo
* https://www.youtube.com/watch?v=Qgg5oNDylG8
* https://www.youtube.com/watch?v=83ZZp8wJ-UY

### Installation
* [(Youtube) Installation with LUKS](https://www.youtube.com/watch?v=kXqk91R4RwU&t)
* [(Gist) Crypt installation script](https://github.com/IvnLum/Arch-Linux-Crypt-Install/blob/main/cryptinst.sh)
* [(Gist Arch) Installation Guide](https://gist.github.com/dante-robinson/fdc55726991d3f17e0dbef1701d343ef)
* [(Arch Forum) TMPFS Config](https://bbs.archlinux.org/viewtopic.php?id=261327)
* [(Arch Wiki) Block device naming](https://wiki.archlinux.org/title/Persistent_block_device_naming)
* [(Youtube) BTRFS, ZRAM, TimeShift](https://www.youtube.com/watch?v=fFxWuYui2LI)


### Post Installation
* [(GitHub) ZRAM Hibernate](https://github.com/gissf1/zram-hibernate)
* [(Youtube) HyDE](https://www.youtube.com/watch?v=H_g1sf1PWt0)
* [(Youtube) Post Installation](https://www.youtube.com/watch?v=8Oz4CIB4YjU)
