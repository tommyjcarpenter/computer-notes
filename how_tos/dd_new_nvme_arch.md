# New NVME Drive

Bought a new NVME drive. Wanted to copy the old Drive to it. 

helpful commands:
1. `blkid`
1. `lsblk`

Steps:

1. BACKUP EVERYTHING
1. BACKUP EVERYTHING AGAIN
1. boot into live USB
1. `dd if=/dev/sdOLD of=/dev/sdNEW bs=64K conv=noerror,sync status=progress`
1. reboot
1. boot into live USB again
1. Mount `/dev/nvmexxx` /mnt
1. `arch-chroot /mnt`
1. Regenerate a new uuid: `tune2fs -U $(uuidgen) /dev/nvmexxx`
1. change `/etc/fstab` with the new UUID from the previous step
1. `grub-mkconfig -o /boot/grub/grub.cfg`
1. `mkinitcpio -p linux`
