
0. Run as root

 ```
 sudo su
 ```
---
1. Create swap file

 Note: set count to your RAM capacity + 2, eg 16GB RAM so count=18
 ```
 dd if=/dev/zero of=/swapfile bs=1G count=18 status=progress
 chmod 600 /swapfile
 mkswap /swapfile 
 echo "/swapfile none swap sw 0 0" | tee -a /etc/fstab
 mount -a
 swapon -a
 ```
---
2. Find the swap device

 This will be referred to as `SWAP_DEVICE` later on
 ```
 findmnt -no UUID -T /swapfile
 ```
---
3. Find the swap file offset

 This will be referred to as `SWAP_FILE_OFFSET` later on
 ```
 filefrag -v /swapfile | awk '{ if($1=="0:"){print substr($4, 1, length($4)-2)} }'
 ```
---
4. Configure GRUB

 ```
 nano /etc/default/grub
 ```
 On line `GRUB_CMDLINE_LINUX_DEFAULT`
 Append in the speech marks:
 ```
 resume=UUID=SWAP_DEVICE resume_offset=SWAP_FILE_OFFSET
 ```
 Exit and save
 
 Update GRUB
 ```
 grub-mkconfig -o /boot/grub/grub.cfg
 ```
---
5. Configure the initial ramdisk

 ```
 nano /etc/mkinitcpio.conf
 ```
 On `HOOKS=(base udev ... filesystems fsck)`
 After `filesystems` add `resume`
 Example:
 ```
 HOOKS=(base udev ... filesystems resume fsck)
 ```
 Exit and save
 
 Generate initial ramdisk
 ```
 mkinitcpio -P
 ```
----
6. Reboot
 ```
 shutdown now -r
 ```
