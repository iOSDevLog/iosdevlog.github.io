---
layout: post
title: "CentOS install laravel"
description: ""
category: php
tags: [laravel, CentOS]
---

# Composer

<https://getcomposer.org/download/>

```bash
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
$ php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
$ php composer-setup.php
$ php -r "unlink('composer-setup.php');"
```

# Laravel

<https://laravel.com/docs/5.5>

```
$ composer global require "laravel/installer"
...
Installation failed, deleting ./composer.json.
The following exception is caused by a lack of memory or swap, or not having swap configured
Check https://getcomposer.org/doc/articles/troubleshooting.md#proc-open-fork-failed-errors for details

The following exception is caused by a lack of memory or swap, or not having swap configured
Check https://getcomposer.org/doc/articles/troubleshooting.md#proc-open-fork-failed-errors for details

PHP Warning:  proc_open(): fork failed - Cannot allocate memory in phar:///usr/local/bin/composer/vendor/symfony/console/Application.php on line 979

Warning: proc_open(): fork failed - Cannot allocate memory in phar:///usr/local/bin/composer/vendor/symfony/console/Application.php on line 979
...
```

<https://getcomposer.org/doc/articles/troubleshooting.md#proc-open-fork-failed-errors>

```
$ free -m
              total        used        free      shared  buff/cache   available
Mem:            993         466         368          12         157         378
Swap:             0           0           0

$ /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
/bin/dd: failed to open ‘/var/swap.1’: Permission denied

$ sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
[sudo] password for iosdevlog:
1024+0 records in
1024+0 records out
1073741824 bytes (1.1 GB) copied, 10.3394 s, 104 MB/s

$ sudo /sbin/mkswap /var/swap.1
Setting up swapspace version 1, size = 1048572 KiB
no label, UUID=607af6e2-b5cc-4fe9-880d-7924e188e303

$ sudo /sbin/swapon /var/swap.1
swapon: /var/swap.1: insecure permissions 0644, 0600 suggested.

$ free -m
              total        used        free      shared  buff/cache   available
Mem:            993         467          72          12         453         367
Swap:          1023           0        1023

$ composer global require "laravel/installer"
```

# add to PATH

```
$ vim ~/.zshrc
export PATH="$HOME/.config/composer/vendor/bin:$PATH"
```

# PHP7.1

<http://www.marksei.com/install-php-7-centos-7/>

# test

```
$ laravel new blog # or composer create-project --prefer-dist laravel/laravel blog
$ cd laravel
$ php artisan serve &
Laravel development server started: <http://127.0.0.1:8000>
$ w3m http://localhost:8000
```

# Homestead

## virtualbox

云服务器不能开启虚拟化，因为云服务器本来就是虚拟的。

cloud serve cant not open virtual.

<https://wiki.centos.org/HowTos/Virtualization/VirtualBox>

<https://wiki.centos.org/zh/HowTos/Virtualization/VirtualBox>

```
$ cd /etc/yum.repos.d/
$ sudo wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo
$ sudo yum search virtualbox
=========================== N/S matched: virtualbox ============================
VirtualBox-4.3.x86_64 : Oracle VM VirtualBox
VirtualBox-5.0.x86_64 : Oracle VM VirtualBox
VirtualBox-5.1.x86_64 : Oracle VM VirtualBox
$ sudo yum install VirtualBox-5.1
$ virtualbox
WARNING: The vboxdrv kernel module is not loaded. Either there is no module
available for the current kernel (3.10.0-327.36.3.el7.x86_64) or it failed to
load. Please recompile the kernel module and install it by

sudo /sbin/vboxconfig

You will not be able to start VMs until this problem is fixed.
Qt FATAL: QXcbConnection: Could not connect to display
[1]    10716 abort      virtualbox
```

### kernel module

```
$ uname -r
3.10.0-327.36.3.el7.x86_64
$ sudo yum install kernel kernel-headers kernel-devel gcc make
Installed:
  kernel.x86_64 0:3.10.0-693.2.2.el7  kernel-devel.x86_64 0:3.10.0-693.2.2.el7

Updated: 
  kernel-headers.x86_64 0:3.10.0-693.2.2.el7 kexec-tools.x86_64 0:2.0.14-17.el7

$ ls /usr/src/kernels/
3.10.0-693.2.2.el7.x86_64

$ uname -r
3.10.0-327.36.3.el7.x86_64
```

```
$ sudo usermod -a -G vboxusers iosdevlog
$ sudo virtualbox & # use root install extension, if you want GUI, see below
```

How can I fix “cannot find a valid baseurl for repo” errors on CentOS?

<https://unix.stackexchange.com/questions/22924/how-can-i-fix-cannot-find-a-valid-baseurl-for-repo-errors-on-centos>

> First you need to get connected, AFAIK CentOS 6 minimal set your network device to ONBOOT=No, just do a dhclient to your network interface and you should be up and running.
>
> Changing to *ONBOOT=yes* in `/etc/sysconfig/network-scripts/ifcfg-enp0s3`, reboot


# Vagrant

<https://www.vagrantup.com/downloads.html>

```
$ sudo yum install vagrant_2.0.0_x86_64.rpm
$ vagrant box add laravel/homestead # slow
$ cat homestead.json
{
    "name": "laravel/homestead",
        "versions":
            [
            {
                "version": "3.1.0",
                "providers":
                    [
                    {
                        "name": "virtualbox",
                        "url": "/home/iosdevlog/php/virtualbox.box"
                    }
                    ]
            }
            ]
}
$ vagrant box add homestead.json
```

## Homestead

```
$ git clone https://github.com/laravel/homestead.git Homestead
$ cd Homestead
$ bash init.sh
$ vim Homestead.yaml
```

edit `Homestead.yaml`

```
---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
- ~/.ssh/id_rsa

folders:
- map: ~/code/laravel
to: /home/vagrant/code

sites:
- map: homestead.localhost
to: /home/vagrant/code/Lavavel/public

databases:
- homestead
```

```
$ ssh-keygen -t rsa -C "iosdevlog@iosdevlog.com"
$ sudo vim /etc/hosts
192.168.10.10  homestead.localhost
```

# lavarel

```
$ cd ~/Homestead
$ vagrant up
$ vagrant ssh
$ cd ~/code
$ composer create-project laravel/laravel Laravel
```

![lavarel](/assets/images/php/lavarel/lavarel.png)

Mac OS safari visit: <http://homestead.localhost>

# vnc server 

<http://linoxide.com/linux-how-to/install-configure-vnc-server-centos-7-0/> author： Arun Pyasi

<https://linux.cn/article-5335-1.html> 译者： boredivan

* Installing X-Windows

```
$ sudo -s
# yum check-update
# yum groupinstall "X Window System"
# yum install gnome-classic-session gnome-terminal nautilus-open-terminal control-center liberation-mono-fonts
```

### if you don't need GUI, skip this 设置默认启动图形界面，如果不想默认启动图形界面，可以不设置 

```
# unlink /etc/systemd/system/default.target
# ln -sf /lib/systemd/system/graphical.target /etc/systemd/system/default.target
# reboot
```

* Installing VNC Server Package

```
# yum install tigervnc-server -y
# cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
# vim /etc/systemd/system/vncserver@:1.service
```

edit `/etc/systemd/system/vncserver@:1.service`

```
[Service]
Type=forking
User=iosdevlog # you username

# Clean any existing files in /tmp/.X11-unix environment
ExecStartPre=-/usr/bin/vncserver -kill %i
ExecStart=/usr/bin/vncserver %i
PIDFile=/home/iosdevlog/.vnc/%H%i.pid # you username
ExecStop=-/usr/bin/vncserver -kill %i
```

```
# systemctl daemon-reload
# su iosdevlog
$ sudo vncpasswd
```

#### Enabling and Starting the service

To enable service at startup ( Permanent ) execute the commands shown below.

```
$ sudo systemctl enable vncserver@:1.service
```

Then, start the service.

```
$ sudo systemctl start vncserver@:1.service
```

### Allowing Firewalls

To enable service at startup ( Permanent ) execute the commands shown below.

```
$ sudo systemctl enable vncserver@:1.service
```

Then, start the service.

```
$ sudo systemctl start vncserver@:1.service
```

![VNCViewer](/assets/images/php/lavarel/VNCViewer.png)
