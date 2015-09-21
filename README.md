---
title: "systemd and CentOS7"
author: "Martin Kopta"
date: "September 20, 2015"
output: html_document
---


Learn how to work with systemd,
how to create unit files
and what is new in CentOS 7.

Agenda
========================================================

* systemd 101
    basic commands to operate the system
* Comparsion with init.
* Configuring systemd.
* Unit files and how to create one.
* What is new in CentOS 7

Bonus: tips and tricks.


systemd 101
========================================================

```
systemctl disable sshd
systemctl enable sshd
systemctl start sshd
systemctl stop sshd
systemctl restart sshd
systemctl reload sshd
systemctl status sshd

journalctl -e      # end
journalctl -f      # follow
journalctl -b      # this boot
journalctl -u sshd # unit
```


Comparsion with init
========================================================

* C versus shell scripts versus
* Unit files versus init scripts
* Targets versus runlevels
* Binary versus plain text logs
* systemd never loses initial log messages
* systemd knows when a service crashes
* systemd can respawn daemons as needed
* systemd records runtime data
    (captures stdout/stderr of processes)
* systemd doesn't lose daemon context during runtime
* systemd can kill all components of a service cleanly
* systemctl command replaces service and chkconfig

Comparsion with init (cont.)
========================================================

```
Runlevel    Target              Symlink to
Runlevel 0  runlevel0.target -> poweroff.target
Runlevel 1  runlevel1.target -> rescue.target
Runlevel 2  runlevel2.target -> multi-user.target
Runlevel 3  runlevel3.target -> multi-user.target
Runlevel 4  runlevel4.target -> multi-user.target
Runlevel 5  runlevel5.target -> graphical.target
Runlevel 6  runlevel6.target -> reboot.target

systemctl get-default
systemctl set-default multi-user.target
    (/etc/systemd/system/default.target,
        previously /etc/inittab)
systemctl poweroff
systemctl reboot
```

Configuring systemd
========================================================

```
/etc/systemd/journald.conf
  mkdir /var/log/journal
  SystemMaxUse=500M
  systemctl restart systemd-journald 

/etc/systemd/system.conf
/etc/systemd/user.conf
```


Unit files and how to create one
========================================================

```
systemctl list-units
systemctl list-units -t services
systemctl list-unit-files -a
systemctl list-unit-files -a -t services

ls /{lib,etc}/systemd/system

systemctl cat sshd

systemctl daemon-reload

man 5 systemd.service

cp /lib/systemd/system/sshd.service \
   /etc/systemd/system/xxx.service
vim $_
systemctl daemon-reload
systemctl enable xxx
systemctl start xxx
journalctl -u xxx -f
```


What is new in CentOS 7
========================================================

Changed

* Init: s/init/systemd/
* System and service manger: s/upstart/systemd/
* Default FS: s/ext4/xfs/
* Kernel: s/2.6/3.10/
* Default first user id: s/500/1000/
* Maximum file size: s/16TB/500TB/
* Moved /bin, /sbin, /lib, and /lib64 to /usr
* Default boot loader: s/GRUB 0.97/GRUB 2/
  supports GPT, EFI, xfs, ext4, ntfs, hfs+, raid, ..
* Default firewall: s/iptables/firewalld/
* Default desktop: s/GNOME 2/GNOME 3, KDE 4.10/
* Default database: s/MySQL/MariaDB/
* Default java: s/OpenJDK 1.6/OpenJDK 1.7/

New

* Docker support (epel->extras)
* No 32bit
* Python 3 available in epel7
* Support for 40 Gigabit NICs
* Minium 1GB (5GB) to install
* Support for new processors (Intel Broadwell)
  and graphics (AMD Hawaii) 
* Full support for OpenJDK 1.8
* Atomic host


Tips and tricks
========================================================

```
systemd-analyze
systemd-analyze blame
systemd-analyze critical-chain
systemd-analyze critical-chain expenseit-web
systemctl list-dependencies expenseit-web
systemctl kill expenseit-web
journalctl /usr/sbin/sshd
journalctl -b -1
journalctl --since=today
journalctl -p err
journalctl -u expenseit-web -e -b
journalctl -u expenseit-web -f
systemctl show mysqld | grep CPUShares
systemctl suspend
systemctl hibernate


hostnamectl
localectl
timedatectl
loginctl
```

Even more
========================================================

* firewalld, firewall-cmd
* selinux


Random links
========================================================

https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files
https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7
https://www.digitalocean.com/community/tutorials/systemd-essentials-working-with-services-units-and-the-journal

https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/index.html
https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/
https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/chap-Managing_Services_with_systemd.html

https://systemd.events/

https://www.youtube.com/watch?v=MxjenQ31b70

https://access.redhat.com/articles/rhel-atomic-getting-started

http://www.freedesktop.org/software/systemd/man/systemd-journal-upload.html
http://www.freedesktop.org/software/systemd/man/systemd-journal-remote.html
