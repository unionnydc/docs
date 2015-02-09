# Ubuntu 14.04 web server

### CONTENTS:
- [add non-root-user](#add-non-root-user)
- [install git](#install-git)
- [clone unrealgrp-docs](#clone-unrealgrp-docs)
- [rename server](#rename-server)
- [authorize personal ssh key](#authorize-personal-ssh-key)
- [harden ssh](#harden-ssh)
- [setup firewall](#setup-firewall)
- [add local ssh config](#add-local-ssh-config)
- [configure .bashrc](#configure-bashrc)
- [update ubuntu](#update-ubuntu)
- [install postgres](#install-postgres)
- [install rvm](#install-rvm)
- [install nginx](#install-nginx)
- [install redis](#install-redis)

### TODO:
- install unicorn
- nginx.conf and default site conf
- logrotate
- remote backups with backup gem

## add non-root user
```bash
# if no non-root user, add one
$ adduser webdev
$ usermod -a -G sudo webdev
```

## install git
```bash
$ sudo apt-get install git
$ git config --global user.name "WebDev User"
$ git config --global user.email webdev@my-server-nickname
```

## clone unrealgrp-docs
```bash
$ git clone git@github.com:Unreal-Group/unrealgrp-docs.git
```

## rename server:
```bash
$ sudo vim /etc/hostname
# my-server-nickname

$ sudo vim /etc/hosts
# 127.0.1.1 my-server-nickname

$ sudo hostname my-server-nickname
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
# NOTE: aws ec2 defaults are probably fine
$ sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.pristine
$ sudo vim /etc/ssh/sshd_config
# Protocol 2
# PermitRootLogin without-password
# # Port 30000 # consider change to custom port
# # AllowUsers sean # CAREFUL
```

## setup firewall
```bash
# activate firewall rules
$ cd ~/unrealgrp-docs/support
# review iptables.firewall.rules, then:
$ sudo cp iptables.firewall.rules /etc/iptables.firewall.rules
$ sudo /sbin/iptables-restore < /etc/iptables.firewall.rules

# apply firewall rules on reboot
# review firewall, then:
$ sudo cp firewall /etc/network/if-pre-up.d/firewall
$ sudo chmod +x /etc/network/if-pre-up.d/firewall
```

## add local ssh config
```bash
# local:
$ vim ~/.ssh/config
# Host nickname
# Hostname ip-address
# User sean
## Port 30000 # custom port from remote sshd_config

# remote:
$ sudo service ssh restart

# IMPORTANT: make sure you can login before existing remote shell!
# local:
$ ssh nickname # from ~/.ssh/config host
```

## configure .bashrc
```bash
$ cd && cp .bashrc .bashrc.pristine
$ cd ~/unrealgrp-docs/support
$ cp .git-prompt.sh ~/.
# double check .bashrc-append, then:
$ cat .bashrc-append >> ~/.bashrc
$ cd && source .bashrc
```

## update ubuntu
```bash
$ sudo apt-get update
$ sudo aptitude safe-upgrade
```

## install postgres
```bash
# postgresql-contrib includes additional modules
# e.g., hstore, fuzzystrmatch, pg_trgm
$ sudo apt-get install postgresql postgresql-contrib

# [optional] create database:
$ sudo -u postgres createdb my-database-name
# load my-database-name into psql:
$ sudo -u postgres psql my-database-name
# to add a password type: \password
# then quit with: \q
```

## install rvm
```bash
$ cd && gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3
$ \curl -sSL https://get.rvm.io | bash
$ source ~/.rvm/scripts/rvm
$ echo "source ~/.rvm/scripts/rvm" >> ~/.bashrc
$ echo "gem: --no-doc --no-ri" >> ~/.gemrc
$ rvm list known
# consider latest stable ruby
$ rvm install 2.2.0 # give password to allow sudo installs
$ rvm use 2.2.0 --default
```

## install nginx
```bash
$ sudo apt-get install nginx
# TODO: tweak nginx.conf and default site conf
```

## install redis
```bash
$ sudo apt-get install redis-server
# check it out:
$ redis-cli
# 127.0.0.1:6379> ping
# PONG
# quit by typing: quit
```
