# CONTENTS:
- [add non-root-user](#add-non-root-user)
- [rename server](#rename-server)
- [authorize personal ssh key](#authorize-personal-ssh-key)
- [harden ssh](#harden-ssh)
- [setup firewall](#setup-firewall)
- [add local ssh config](#add-local-ssh-config)
- [configure .bashrc](#configure-bashrc)
- [update ubuntu](#update-ubuntu)
- [install rvm](#install-rvm)
- [install git, nginx, redis](#install-git-nginx-redis)

### TODO:
- git config
- postgresql
- install unicorn
- configure git
- nginx.conf and default site conf
- logrotate
- remote backups with backup gem

## add non-root user
```bash
# if no non-root user, add one
$ adduser sean
$ usermod -a -G sudo sean
```

## rename server:
```bash
$ sudo vim /etc/hostname
# cyclone

$ sudo vim /etc/hosts
# 127.0.1.1 cyclone

$ sudo hostname cyclone
```

## authorize personal ssh key
```bash
# local:
$ cat ~/.ssh/id_rsa.pub | pbcopy

# remote:
$ cd ~/.ssh
$ cat >> authorized_keys
# paste public key then ctrl+d
$ chmod 600 authorized_keys
```

## harden ssh
```bash
$ sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.pristine
$ sudo vim /etc/ssh/sshd_config
# Port 30000 # change to custom port
# Protocol 2
# PermitRootLogin no
# # AllowUsers sean # CAREFUL
```

## setup firewall
```bash
# download and activate firewall rules
$ wget http://j.mp/16sESJI -O iptables.firewall.rules
# double check downloaded file, then:
$ sudo cp iptables.firewall.rules /etc/iptables.firewall.rules
$ sudo /sbin/iptables-restore < /etc/iptables.firewall.rules

# apply firewall rules on reboot
$ wget http://j.mp/1zY4Pi1 -O firewall
# double check downloaded file, then:
$ sudo cp firewall -O /etc/network/if-pre-up.d/firewall
$ sudo chmod +x /etc/network/if-pre-up.d/firewall
```

## add local ssh config
```bash
# local:
$ vim ~/.ssh/config
# Host nickname
# Hostname ip-address
# User sean
# Port 30000 # custom port from remote sshd_config

# remote:
$ sudo service ssh restart

# IMPORTANT: make sure you can login before existing remote shell!
# local:
$ ssh nickname # from ~/.ssh/config host
```

## configure .bashrc
```bash
$ cp .bashrc .bashrc.pristine
$ wget http://j.mp/1zY6y77 -O .git-prompt.sh
$ wget http://j.mp/1C2RwrQ -O bashrc-gist
# double check downloaded file, then:
$ cat bashrc-gist >> .bashrc
$ source .bashrc
$ rm bashrc-gist
```

## update ubuntu
```bash
$ sudo apt-get update
$ sudo aptitude safe-upgrade
```

## install rvm
```bash
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3
$ \curl -sSL https://get.rvm.io | bash
$ source ~/.rvm/scripts/rvm
$ echo "source ~/.rvm/scripts/rvm" >> ~/.bashrc
$ echo "gem: --no-doc --no-ri" >> ~/.gemrc
$ rvm list known
# consider latest stable ruby
$ rvm install 2.2.0 # give password to allow sudo installs
$ rvm use 2.2.0 --default
```

## install git, nginx, redis
```bash
$ sudo apt-get install git nginx redis-server
```
