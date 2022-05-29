On 2022-05-28 evening, I updated my system, which brought in Kernel 5.18.0.

Woke up on 2022-05-29, and all my MDADM arrays were gone.

After some playing around and googling, I found:
1. https://bbs.archlinux.org/viewtopic.php?id=276716
1. https://bugs.archlinux.org/task/74888

At first, I tried to downgrade the Linux Kernel using:

    sudo pacman -U /var/cache/pacman/pkg/linux-5.17.8.arch1-1-x86_64.pkg.tar.zst /var/cache/pacman/pkg/linux-headers-5.17.8.arch1-1-x86_64.pkg.tar.zst

However, that led to a broken system due to NVIDIA and lightdm..

I hit ctrl-alt-F2 to login, and fully upgraded again.

Then, I found this:

    https://wiki.archlinux.org/title/Arch_Linux_Archive#How_to_restore_all_packages_to_a_specific_date

Which is exactly what I did - at the time of writing, my `/etc/pacman.conf` still looks like:

```
[core]
SigLevel = PackageRequired
Server=https://archive.archlinux.org/repos/2022/05/27/$repo/os/$arch
#Include = /etc/pacman.d/mirrorlist

[extra]
SigLevel = PackageRequired
Server=https://archive.archlinux.org/repos/2022/05/27/$repo/os/$arch
#Include = /etc/pacman.d/mirrorlist

#[community-testing]
#Include = /etc/pacman.d/mirrorlist

[community]
SigLevel = PackageRequired
Server=https://archive.archlinux.org/repos/2022/05/27/$repo/os/$arch
#Include = /etc/pacman.d/mirrorlist
```

Waiting for a RAID1 fix to upgrade normally again.
