# SSH no password

1) If you don’t already have SSH keys on your own machine, generate private and public key
```
ssh-keygen -t rsa
```

2) on the server,
```
mkdir ~/.ssh;
```

3) on the server, create a file called `~/.ssh/authorized_keys`
4) add the PUBLIC key `id_ras.pub` created in step 1 into this file
5) on the server, run:
```
chmod 700 ~/.ssh/; chmod 600 ~/.ssh/authorized_keys;
```
6) should now be able to just do ssh server and have it work. 

Helpful link: https://mediatemple.net/community/products/dv/204644740/using-ssh-keys-on-your-server

# Debugging

If you have root access to the server, the easy way to solve such problems is to run sshd in debug mode, e.g.:

    service ssh stop      # will not kill existing ssh connections
    /usr/sbin/sshd -d     # full path to sshd executable needed, 'which sshd' can help
    ...debug output...
    service ssh start

Authentication refused: bad ownership or modes for directory; Might have to do a

    chmod -R 755 /home/tommy/

then a 

    chmod 700 ~/.ssh/; chmod 600 ~/.ssh/authorized_keys;

http://unix.stackexchange.com/questions/37164/ssh-and-home-directory-permissions
