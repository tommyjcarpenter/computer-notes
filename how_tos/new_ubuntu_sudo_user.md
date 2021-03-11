# Creating a new user on a Ubuntu server with Sudo

1. `sudo adduser tommy` (enter a new password)
2. `sudo usermod -aG sudo tommy`
3. `cd /home/tommy`
4. `sudo mkdir .ssh`
5. `sudo vim .ssh/authorized_keys` : add public key
6. `sudo chmod 700 ~/.ssh`
7. `sudo chmod 600 ~/.ssh/authorized_keys`

Then, I change my shell to fish:

1. `sudo apt-get install software-properties-common`
2. `sudo apt-add-repository ppa:fish-shell/release-3`
3. `sudo apt-get update`
4. `sudo apt-get install fish`
5. `chsh -s /usr/bin/fish`
