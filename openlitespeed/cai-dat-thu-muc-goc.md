# md
https://docs.openlitespeed.org/config/
## wsl
```bash

sudo mkdir /usr/local/lsws/Example2
sudo mkdir /usr/local/lsws/Example2/{conf,html,logs}

sudo chown lsadm:lsadm /usr/local/lsws/Example2/conf
```

## Add to WebAdmin Console // Virtual Hosts > Add.
Virtual Host Name = Example2

Virtual Host Root = $SERVER_ROOT/Example2

Config File = $SERVER_ROOT/conf/vhosts/Example2/vhost.conf

Enable Scripts/ExtApps = Yes

Restrained = No

## document root
```bash
pwd
```
/home/cong/git/document/openlitespeed
### General tab

Document Root = /home/cong/git/document/openlitespeed

Index Files = index.html, index.php

### Rewrite tab:
Enable Rewrite = Yes

Auto Load from .htaccess = yes

## Set up Listeners
Click the Add button to create an HTTP listener with following settings:

Document

Listener Name = HTTP

IP Address = ANY IPv4

Port = 8888

Secure = No

### Virtual Host Mappings
Example2

host *

```bash
sudo systemctl restart lsws
```
/home/cong/git/document/openlitespeed

## them vao nhom
```bash
sudo usermod -aG cong nobody
```
