Hit this: https://www.reddit.com/r/archlinux/comments/ifdc4s/nvidia_driver_45066_prevents_x_from_sending_any/

Also VMware broken on 5.8. 

Could have done this: https://wiki.archlinux.org/index.php/Arch_Linux_Archive#How_to_restore_all_packages_to_a_specific_date

Linked from that reddit thread, but saw it late

My eventual fix:
* Downgrading to cinnamon
* Figured out the gcc version of my kernel by doing cat /proc/version
* If the GCC version also wasn’t downgraded properly, VMware failed to build, complaining that GCC version didn’t match 

All this:

    sudo pacman -U gcc-10.1.0-2-x86_64.pkg.tar.zst gcc-libs-10.1.0-2-x86_64.pkg.tar.zst
    sudo pacman -U /var/cache/pacman/pkg/nvidia-utils-450.57-2-x86_64.pkg.tar.zst /var/cache/pacman/pkg/nvidia-450.57-6-x86_64.pkg.tar.zst /var/cache/pacman/pkg/nvidia-settings-450.57-1-x86_64.pkg.tar.zst
    sudo pacman -U /var/cache/pacman/pkg/linux-5.7.12.arch1-1-x86_64.pkg.tar.zst /var/cache/pacman/pkg/linux-headers-5.7.12.arch1-1-x86_64.pkg.tar.zst


Similar help from https://bbs.archlinux.org/viewtopic.php?id=252993

    $ cd /var/cache/packman/pkg
    wink@wink-desktop:/var/cache/pacman/pkg
    $ sudo pacman -U gcc-9.2.0-4-x86_64.pkg.tar.xz gcc-libs-9.2.0-4-x86_64.pkg.tar.xz gcc-fortran-9.2.0-4-x86_64.pkg.tar.xz
    $ sudo pacman -U linux-5.4.15.arch1-1-x86_64.pkg.tar.zst linux-headers-5.4.15.arch1-1-x86_64.pkg.tar.zst nvidia-dkms-440.44-15-x86_64.pkg.tar.zst nvidia-utils-440.44-3-x86_64.pkg.tar.zst

I also TEMPORARILY (until everything up to date was working agian) add IgnorePkg to /etc/pacman.conf:

    IgnorePkg   = gcc gcc-libs gcc-fortran linux linux-headers nvidia-dkms nvidia-utils

Also helped: https://github.com/mkubecek/vmware-host-modules/issues/40

Also helped: https://ubuntuforums.org/showthread.php?t=1565491
