# fix "corrupt crypto system" errors

Was hitting errors about corrupt crypto systems and packages being invalid.
The following post solved problem: http://www.cupoflinux.com/SBB/index.php?topic=2959.0:

Updates Mirrors

    sudo pacman-mirrors -g

Forces update of lists

    sudo pacman -Syy

synchs and/or reinstalls gnupg

    sudo pacman -S gnupg

Updates pacman keys

    sudo pacman-key --populate archlinux

Updates pacman keys

    sudo pacman-key --populate manjaro

refreshes pacman keys

    sudo pacman-key --refresh-keys

Force list update and synchronize packages on system. Ii the above does not work... REMOVE ALL KEYS as shown below and start over at the top of this list.

    sudo pacman -Syyu

If that does not work.... SHUT OFF pgp signature checking as a last resort: HERE

    sudo rm /etc/pacman.d/gnupg
