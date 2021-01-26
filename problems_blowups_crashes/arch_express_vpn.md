# Arch express VPN issues

Symptom: couldn’t connect to vpn on arch. Would get about 60% of the way there and fail.

After googling, finally found command:

    expressvpn diagnostics

Which showed

```
Fri Oct 25 07:00:58 2019 WARNING: Failed running command (--up/--down):
external program exited with error status: 1
Fri Oct 25 07:00:58 2019 Exiting due to fatal error
ln: failed to create symbolic link '/etc/resolv.conf': Operation not
permitted
Disconnected with error: vpn process terminated unexpectedly
```

googling led to https://unix.stackexchange.com/questions/381707/cannot-rename-resolv-conf-file-as-root

running

    sudo chattr -i /etc/resolv.conf

fixed it
