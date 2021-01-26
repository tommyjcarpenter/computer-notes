I probably killed an update of linux (like shut down when the update process was still running) and woke up to a `no image vmlinuz found`

This got me back:

1. Boot a Arch Linux live CD or USB drive
2. Mount your root partition: `mount /dev/sda# /mnt`
3. Change your root directory: `arch-chroot /mnt`
4. Reinstall the kernel: `pacman -S linux`

This was adapted from the below instructions and link however some of the steps didn’t apply since I don’t have a separate arch boot partition for the imz image (DO NOT CONFUSE THIS WITH BOOT PARTITION FOR THE GRUB!)

1. Boot a Arch Linux live CD or USB drive
2. Get connected to the Internet: wifi-menu
3. Mount your root partition: `mount /dev/sda# /mnt`
4. Mount your boot partition: `mount /dev/sda# /mnt/boot`
5. Change your root directory: `arch-chroot /mnt`
6. Reinstall the kernel: `pacman -S linux`

https://joshtronic.com/2017/09/07/fixing-an-arch-linux-system-that-is-booting-into-emergency-mode/
