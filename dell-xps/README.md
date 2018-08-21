# Dell XPS Linux Journey

When I got this computer, I wanted to have the same OS, files, programs, etc on it. So, I used GParted to create an image of the Linux OS partition (I have it dual-booted) and saved it to my backup drive. I restored the PC401 NVMe SK hynix 1TB SSD with the image through the use of an Ubuntu Live USB. I used the Ubuntu USB because the disk had no OS on it (other than Windows, but that doesn’t count) and I needed to unmount the disk to read/write from a .iso file. After the disk was restored, I expanded the partition (via GParted) and booted into the OS. It worked fine.

## Problems

Later, my OS wouldn't boot, so I again booted into an Ubuntu Live USB and saw that the disk was corrupted. So, I ram `fsck` to recover my files and got my home folder back, intact. I then decided to do a fresh install of Kali Linux with LVM/LUKS. I downloaded the Kali Linux ISO from the internet and created a bootable USB. I rebooted, and booted from the Kali USB. I was greeted by a GRUB boot screen, with the option to install. It didn't work: It hung on starting the User Manager. I was confused, for the Ubuntu USB worked fine. With a bit of Googling, I found out that my GPU was the culprit, and added `nouveau.modeset=0` to my GRUB option. It worked.

## Reinstalling

I installed Kali Linux according to the installer, and selected `LVM+LUKS` for the encryption option. The OS installed, but didn't boot. The GRUB screen showed up, I selected to boot, and it dropped to an `initramfs` shell. To fix this, I again booted from the live USB, chrooted into my internal SSD, and purged the kernel, `initramfs-tools`, and `cryptsetup-initramfs` files. After rebuilding and reinstalling, the booting worked.

## WiFi Driver Error

On boot, the WiFi driver was not working. I was using `ath10k` for both Bluetooth and WiFi, but Ethernet via USB-C was working. Through Ethernet, I upgraded the system, hoping that it would solve the problem and it did not. I removed the `ath*` and did a full system upgrade again and that fixed the problem. However, on the reboot, I was again dropped to the `initramfs` shell, so I used my procedure to fix it.

## Files

I have, in this folder, my GRUB configuration files. Use if you wish