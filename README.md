# Libvirt Backup Tool (backup-vms)

Tool to backup your qemu-kvm virtual machines.
`backup-vms` is built to manage backup as bare-metal mode, in local machine or
remotes hosts.

This tool is based on **Rsync**.

## Features

- backup VM ! (disks + libvirt domain)
- speed up VM downtimes by the use local cache (on FileSystem)
- Let admins choose to cache the VM disks files or not
- manage multiple remotes destinations
- control network speed on remotes syncing
- keep VM file sparsed
- just manage VM backups you need (declared in conffile)
- log and notify admins on errors (by mail)
- thinked KISS (no LVM snapshot or other complicated solution), just rsync.

## How to deal with

1. Git clone and install backup-vms in `/usr/local/sbin`,
2. Copy global conf in `/etc/backup-vms/backup-vms.conf`
3. Declare your hosts in conffile in `/etc/backup-vms/conf.d/*.conf`,
4. Make a cron definition in `/etc/cron.d/backup-vms`

  **That's it!**

Backup is triggered by cron. It takes at least 1 configure file to define
default behavior.
Some overrides is possible by declaration in `conf.d/*.conf`.
All variables located in `/etc/backup-vms/backup-vms.conf` are overridable.

In examples directory you could find some exemples

## Examples

Logs are dropped to journald (and via logfile if it's specified)

To see: `journalctl -t backup-vms`

[You could see some output logs in:](examples/logs/backup-vms.log)

## Solved issues

1. suspend VM cause: ntp failed after resume Vms.
    > solved by use shutdown via acpi

2. make sure vm is stopped before syncing
    > solved by do_waiting_shutdown function

3. vm shutdown twice or more when more than one disk is declared on domain
    > solved by find disk from libvirt domain declaration

4. reduce VM downtimes
   > Solved with VM disk caching

5. shape network trafic to minimize network transfert inplacts
   > Solved optimize I/O disk and network bandwidth

## Roadmap

1. Improve VM dependancies

## Ideas

1. Secure backuped files on remotes 


## License

This project is licensed under the terms of the GNU General Public License v3.0
(GPLv3) license.
