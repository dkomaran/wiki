| Directory Mappings                               | LINUX(RedHat)                               |
| ------------------------------------------------ | ------------------------------------------- |
| Root filesystem                                  | / {/dev/sda1}                               |
| Home Directory                                   |                                             |
| Sample configuration files                       |                                             |
| User Accounts                                    | LINUX(RedHat)                               |
| Password files                                   | /etc/passwd                                 |
|                                                  | /etc/shadow                                 |
| Groups file                                      | /etc/group                                  |
| Maximum # of user ID                             | 65535                                       |
| Allow/Deny remote login                          | /etc/securetty                              |
|                                                  | {ttyp1}                                     |
| User nobody's id #                               | 99                                          |
| Group nobody's id #                              | 99                                          |
| Recover root password                            | linux S                                     |
|                                                  | vi /etc/shadow                              |
| Create new user                                  | useradd                                     |
| Delete user                                      | userdel                                     |
| List users                                       |                                             |
| Modify user account                              | usermod                                     |
| General Commands                                 | LINUX(RedHat)                               |
| Unique host ID                                   | hostid                                      |
| Administrator                                    | linuxconf                                   |
| Performance monitor                              | top                                         |
| Virtual Memory statistics                        | vmstat                                      |
| Error logs                                       | dmesg                                       |
| Physical RAM                                     | 64 GB {>2.3.24}                             |
| Shared Memory                                    | sysctl kernel.shmmax                        |
| Process Data Space                               | 900 MB                                      |
| Display swap size                                | free                                        |
| Activate Swap                                    | swapon -a                                   |
| Printers                                         | LINUX                                       |
| Printer Queues                                   | /var/spool/lpd/lp/\*                        |
| Stop LP                                          | /etc/init.d/lpd stop                        |
| Start LP                                         | /etc/init.d/lpd start                       |
| Submit print jobs                                | lpr                                         |
| LP statistics                                    | lpq                                         |
| Remove print jobs                                | lprm                                        |
| Add printer queue                                | printtool                                   |
| Remove Printer queue                             |                                             |
| Make default printer                             |                                             |
| TCP/IP                                           | LINUX(RedHat)                               |
| Network IP configuration                         | /etc/sysconfig/network-scripts/             |
| Hosts IP addresses                               | /etc/hosts                                  |
| Name service switch                              | /etc/nsswitch.conf                          |
| Network parameters                               | sysctl -a \| grep net                       |
| Routing daemon                                   | routed                                      |
| NIC Configurations                               | ifconfig -a                                 |
| Secondary IP Address                             | modprobe ip_alias                           |
|                                                  | ifconfig eth0:1 IP                          |
| Login prompt                                     | /etc/issue                                  |
| Increase the # of pseudo-terminals               | cd /dev                                     |
|                                                  | ./MAKEDEV -v pty                            |
| Maximum # of ptys                                | 256                                         |
| YP/NIS service binder                            | /sbin/ypbind                                |
| System Files                                     | LINUX(RedHat)                               |
| NFS exported                                     | /etc/exports                                |
| NFS Client mounted directories                   | /var/lib/nfs/xtab                           |
| Max File System                                  | 2 TB                                        |
| Max # File Descriptors                           | sysctl fs.file-max                          |
| DISK/LVM Commands                                | LINUX(RedHat)                               |
| Filesystem table                                 | /etc/fstab                                  |
| Free disk blocks                                 | df -k                                       |
| Device listing                                   | cat /proc/devices                           |
| Disk information                                 | cat /proc/scsi/scsi0/sda/model              |
| Disk Label                                       | fdisk -l                                    |
| LVM Concepts                                     | logical extents                             |
|                                                  | logical volume                              |
|                                                  | volume group                                |
| Display volume group                             | vgdisplay -v                                |
| Modify physical volume                           | pvchange                                    |
| Prepare physical disk                            | pvcreate                                    |
| List physical volume                             | pvdisplay                                   |
| Remove disk from volume group                    | vgreduce                                    |
| Move logical volumes to another physical volumes | pvmove                                      |
| Create volume group                              | vgcreate                                    |
| Remove volume group                              | vgremove                                    |
| Volume group availability                        | vgchange                                    |
| Restore volume group                             | vgcfgrestore                                |
| Exports volume group                             | vgexport                                    |
| Imports volume group                             | vgimport                                    |
| Volume group listing                             | vgscan                                      |
| Change logical volume characteristics            | lvchange                                    |
| List logical volume                              | lvdisplay                                   |
| Make logical volume                              | lvcreate                                    |
| Extend logical volume                            | lvextend                                    |
| Reduce logical volume                            | lvreduce                                    |
| Remove logical volume                            | lvremove                                    |
| Prepare boot volumes                             | lilo                                        |
| Extend File system                               | resize2fs                                   |
| Reduce/Split mirrors                             | lvsplit                                     |
| Merge mirrors                                    | lvmerge                                     |
| Create striped volumes                           | lvcreate -i 3 -I 64                         |
| Backup                                           | tar cvf /dev/rst0 /                         |
| Restore                                          | tar xvf /dev/rst0                           |
| MISC                                             | LINUX(RedHat)                               |
| Startup script                                   | /etc/rc.d/rc                                |
| Kernel                                           | /boot/vmlinuz                               |
| Kernel Parameters                                | sysctl -a                                   |
|                                                  | make mrproper                               |
|                                                  | mkinitrd /boot/initrd-2.2.16.img 2.2.16     |
|                                                  | vi /etc/lilo.conf                           |
| List modules                                     | lsmod                                       |
| Load module                                      | insmod                                      |
| Unload module                                    | rmmod                                       |
| Initialize system                                | netconf                                     |
| Physical RAM                                     | free                                        |
| Kernel Bits                                      | getconf WORD_BIT                            |
| Crash utility                                    | [lcrash](http://oss.sgi.com/projects/lkcd/) |
| Trace System Calls                               | strace                                      |
| Machine model                                    | uname -m                                    |
| OS Level                                         | uname -r                                    |
| Run Level                                        | runlevel                                    |
| Core dump files                                  |                                             |
| Boot single user                                 | linux S                                     |
| Maintenance mode                                 |                                             |
| Interrupt Key                                    |                                             |
| Return to console                                |                                             |
| Timezone Management                              | /etc/sysconfig/clock                        |
| NTP Daemon                                       | /etc/ntp.conf                               |
|                                                  | /etc/rc.d/init.d/xntpd                      |
| Software                                         | LINUX(RedHat)                               |
| Install Software                                 | rpm -i package                              |
| Uninstall software                               | rpm -e package                              |
| List installed software                          | rpm -qa                                     |
| Verify installed software                        | rpm -V package                              |
| List all files                                   | rpm -ql package                             |
| List installed patches                           |                                             |
| Package owner                                    | rpm -qf file                                |
| SW Directory                                     | /var/lib/rpm                                |
| Devices                                          | LINUX(RedHat)                               |
| Devices                                          | /dev                                        |
| Install devices for attached peripherals         | /dev/MAKEDEV                                |
| Remove device                                    |                                             |
| Device drivers                                   |                                             |
| CPU                                              | cat /proc/cpuinfo                           |
| List Terminal                                    |                                             |
| Whole Disk                                       | /dev/sda                                    |
| CDROM                                            | /dev/cdrom                                  |
| CDROM file type                                  | iso9660                                     |
| Floppy drive                                     | /dev/fd0                                    |