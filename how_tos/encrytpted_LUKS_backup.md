# creating encrypted LUKS backup

make partitions:
```
(use gparted)
```

encrypting:
```
sudo cryptsetup luksFormat /dev/sdf1
```

open it
```
sudo cryptsetup open --type luks /dev/sdf1 archbackup
```

make the filesystem:
```
mkfs -t ext4 /dev/mapper/archbackup
```

label it:
```
sudo e2label /dev/mapper/archbackup ARCHBACKUP
```
(for exfat use exfat label)      

mount it 
```
sudo mkdir -p /run/media/tommy/ARCH
sudo mount -t ext4 /dev/mapper/archbackup /run/media/tommy/ARCH
````

unmount/close
```
sudo umount /dev/mapper/archbackup
sudo cryptsetup close archbackup
```
