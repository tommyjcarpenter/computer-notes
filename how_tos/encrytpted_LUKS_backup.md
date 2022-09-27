# creating encrypted LUKS backup

make partitions:
```
(use gparted)
```

encrypting:
```
sudo cryptsetup luksFormat /dev/sdc1
```

open it
```
sudo cryptsetup open --type luks /dev/sdc1 archbackup
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
sudo mkdir -p /media/ARCHBACKUP
sudo mount -t ext4 /dev/mapper/archbackup /media/ARCHBACKUP
````

unmount/close
```
sudo umount /dev/mapper/archbackup
sudo cryptsetup close archbackup
```
