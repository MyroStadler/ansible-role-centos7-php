Centos7 PHP
===========

Install PHP.

Requirements
------------

### XDebug

You need to open the XDebug port yourself, this role only opens 80 for http and 443 for https. 

#### Debugging a cloud instance

1. Open port 9001 on the instance:

        firewall-cmd --zone=public --add-port=9001/tcp --permanent # firewall
        firewall-cmd --reload
        semanage port -m -t http_port_t -p tcp 9001 # SELinux 
1. Forward port 9001 on my local machine:

        ssh -R 9001:localhost:9001 centos@example.com
1. Set PHPStorm's PHP > Debug to listen on port 9001.

#### Debugging a VirtualBox or Docker instance

The trick with getting these to work is to have your machine's local, 
reachable IP address as `xdebug.remote_host` and having `xdebug.remote_connect_back=0`.

For Virtualbox I usually have to add a second, host-only adapter (which creates vboxnet0 -- see `ifconfig`), and then find this inet IP with `ifconfig`
on the host, using that for `xdebug.remote_host`.

Having vboxnet0 fixes docker issues as well, since all that is needed is for 127.0.0.1 have a well-known alias IP address. 
You could do something like this to create an alias if you don't use VirtualBox: 
https://gist.github.com/ralphschindler/535dc5916ccbd06f53c1b0ee5a868c93

Role Variables
--------------

All variables are defaulted in `defaults/main.yml`.

Dependencies
------------

none

Example Playbook
----------------

```
---
- name: "Install PHP"
  hosts: all
  become: true
  tasks:
    - include_role:
        name: myrostadler.centos7_php
```

License
-------

GPL-3.0-only

Author Information
------------------

Myro Stadler
