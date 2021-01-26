# mdadm  raid1 drive pair

## TLDR

1) clear out both disks (if you cannot do this, follow the next section)
2) use Webmin or MDADM to create the array with the two disk or the two partitions
3) this will install `linux-raid` fs onto both disks, AND create a new drive called `/dev/mdwhatever` that is the logical RAID drive
4) use MDADM mkfs or GParted to install a filesystem onto the logical RAID drive `/dev/mdwhatever` (not the original disks!!!) 
5) use `sudo blkid` and adjust `/etc/fstab` accordingly

## Step by step

For new disks, I set the bios to AHCI because I was intending to use madam, see: http://askubuntu.com/questions/500470/should-i-enable-my-mobo-raid-controller-or-leave-it-at-ahci

This guide follows  https://wiki.archlinux.org/index.php/RAID but with my own notes

1. GPT partition table both disks

TOMMY NOTE: I changed the filesystem type to unformatted since `LINUX RAID` will replace them when you do the mdadm command below anyway

2. Create
```
sudo mdadm --create --verbose --level=1 --metadata=1.2 --raid-devices=2 /dev/md/sys /dev/sdd1 /dev/sde1
sudo mdadm --create --verbose --level=1 --metadata=1.2 --raid-devices=2 /dev/md/media /dev/sdb1 /dev/sdc1
```
check: 

    cat /proc/mdstat

This should show the status (syncing) of new arrays.

3. Update configuration file
```
echo 'DEVICE partitions' > /etc/mdadm.conf
sudo mdadm --detail --scan >> /etc/mdadm.conf
```

4. Assemble the Array
Once the configuration file has been updated the array can be assembled using mdadm:

    sudo mdadm --assemble --scan

At this point I was able to see it in Webmin.

5. Format the RAID Filesystem

### If using Vera to encrypt the partition
Even if using Veracrypt, create a gpt partition table, and create one partition, then use vera on that partion. Otherwise, whenever you open the disks in gparted, errors will be shown, and it will show the disk as unformatted.
So:
1. create gpt table on `/dev/md/name` 
2. create a single big partition, `ext4` (will get replaced)
3. unmount it if it got mounted
4. open veracrypt and encrypt THE PARTITON, not the entire device! Again encrypting the entire device leads to the device looking unformatting and gparted willl throw other errors like "invalid label" 

### If not using Vera:

The array can now be formatted like any other disk, just keep in mind that:

- Due to the large volume size not all filesystems are suited (see: File system limits).
- The filesystem should support growing and shrinking while online (see: File system features).
- One should calculate the correct stride and stripe-width for optimal performance.

NOTE: Used gparted here to format `/dev/md0`. however I couldn't format it to ExFAT. So I made a GPT partition table
and then I formatted on command line:

    sudo mkfs.exfat /dev/md0

6. Adding it to fstab (if not vera since automount defeats that purpose)
Helpful link: http://askubuntu.com/questions/540202/mount-an-mdadm-raid-1-drive-at-boot

Run this command that shows all UUIDs, which fetches us the magic UUID of the logical RAID partition:

    sudo blkid

    /dev/sda2: UUID="32967f90-b415-456f-8575-bd310e53e1e1" TYPE="swap" PARTUUID="feede1a8-3a27-4738-8003-dff5f62a8d4e"
    ...
    /dev/md0: UUID="EF58-3925" TYPE="exfat" PTTYPE="dos"

Making a directory:

    sudo mkdir /media
    sudo mkdir /media/SYSRAID

then add to fstab using the UUID for the raid drive

    UUID="EF58-3925" /media/SYSRAID exfat defaults 0 0

Add mdadm_udev to the HOOKS section of the mkinitcpio.conf to add support for mdadm directly into the init image:

    HOOKS="base udev autodetect block mdadm_udev filesystems usbinput fsck"

Then 

    mkinitcpio -p linux


7. Giving new labels
This is to give the RAID partition a label.
Helpful link: http://askubuntu.com/questions/276911/how-to-rename-partitions

Using `/dev/mdwhatever`, replace `/dev/sdxN` with your partition (e.g. `/dev/sdc1`):

     sudo fatlabel /dev/sdxN my_label

for NTFS:

    sudo ntfslabel /dev/sdxN my_label

for exFAT:

    sudo exfatlabel /dev/sdxN my_label

for ext2/3/4:

    sudo e2label /dev/sdxN my_label

# Misc gotchas/notes

## MOUNTING RAID 1 VERACRYPT VOLUMES
Tommy: if you use veracrypt to encrypt an entire device (e.g., md0), then use the NAME that was given during the create (see /dev/md/sys above!)
It's number WILL CHANGE FROM BOOT TO BOOT. That is, /dev/md/sys might symlink to md126 on one boot, then symlink to md127 on another.
However, the symlink is always valid. 

## MAKING A NEW ARRAY WITH ONE DISK NOW AND ADD ONE LATER

You CANNOT just take a disk with existing data on it and make it a raid array. MDADM will overwrite the original fs with `linux-raid` fs type.
Then, you install a FS over it and then add the data.

So lets say you have two disks, one with data on it, and you want that data mirrored to the second disk.
You need to:
1. first delete one disk
2. make an MDADM array with one disk
3. add the new FS you want onto it with MDADM
4. copy on the data from disk 2
5. then delete the other disk and add it to the mirror. 

To create a single disk MDADM array follow: https://wiki.archlinux.org/index.php/Convert_a_single_drive_system_to_RAID

    mdadm --create /dev/md0 --level=1 --raid-devices=2 missing /dev/sdb1
