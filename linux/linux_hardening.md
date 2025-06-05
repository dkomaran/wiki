**edit /etc/ssh/sshd_config**

- to make sure direct login as root is not permitted

`Protocol 2`

`SyslogFacility AUTHPRIV`

`PermitRootLogin no`

`PasswordAuthentication yes`

`ChallengeResponseAuthentication no`

`GSSAPIAuthentication yes`

`GSSAPICleanupCredentials yes`

`UsePAM yes`

`AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES`

`AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT`

`AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE`

`AcceptEnv XMODIFIERS`

`X11Forwarding yes`

`Subsystem       sftp    /usr/libexec/openssh/sftp-server`


**edit "/etc/yum.repos.d/media.repo" and add:**

`[InstallMedia]`

`name=RHEL6DVD`

`baseurl=file:///mnt/dvd`

`enabled=1`

`gpgcheck=0`

-----------------
  
'chkconfig anacron off'

'chkconfig haldaemon off`

`chkconfig messagebus off`

`chkconfig apmd off`

`chkconfig hidd off`

`chkconfig microcode_ctl off`

`chkconfig autofs off`

`chkconfig hplip* off`

`chkconfig pcscd off`

`chkconfig avahi-daemon* off`

`chkconfig isdn off`

`chkconfig readahead_early off`

`chkconfig bluetooth off`

`chkconfig kdump off`

`chkconfig readahead_later off`

`chkconfig cups* off`

`chkconfig kudzu off`

`chkconfig rhnsd* off`

`chkconfig firstboot off`

`chkconfig mcstrans off`

`chkconfig setroubleshoot off`

`chkconfig gpm off`

`chkconfig mdmonitor off`

`chkconfig xfs off`

`chkconfig postfix off`

`chkconfig rpcbind off`

------------------
  
**create /etc/cron.daily/yum.cron (chmod a+x yum.cron)**

`#!/bin/sh`

`/usr/bin/yum -R 120 -e 0 -d 0 -y update yum`

`/usr/bin/yum -R 10 -e 0 -d 0 -y update`

-----------------
  
edit "/etc/sysctrl.conf" and add the following lines:

`net.ipv4.conf.all.rp_filter=1`

`net.ipv4.conf.all.accept_source_route=0`

`net.ipv4.icmp_echo_ignore_broadcasts=1`

`net.ipv4.icmp_ignore_bogus_error_messages=1`

`kernel.exec-shield=1`

`kernel.randomize_va_space=1`

-----------------
  
`yum install net-snmp net-snmp-utils -y`

**edit /etc/snmp/snmpd.conf:**

`com2sec U_ReadOnly default nagios`

`com2sec U_ReadWrite default nagios`
  
`group G_ReadOnly any U_ReadOnly`

`group G_ReadWrite any U_ReadWrite`

`view all included .1`

`access G_ReadOnly "" any noauth exact all none none`

`access G_ReadWrite "" any noauth exact all none none`

`trapsink 10.11.2.97 nagios`

`rocommunity MY_READ 127.0.0.1`

`rwcommunity MY_WRITE 127.0.0.1`  

`rocommunity MY_READ nawug1.na.ipsos`

`rwcommunity MY_WRITE nawug1.na.ipsos`

-----------------

  
`netstat -tanp|grep LISTEN`

- used to check for open network ports; should usually be just ssh unless other services installed like httpd and mysql
