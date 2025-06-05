<h1>Turning on Audit Control monitoring</h1>

This is to monitor changes to the /tmp folder on each CATI server as we had someone with root access somehow change the file permissions from 777 to 755 which broke some functionality with reporting that was dependent on writing to /tmp.

This is the command that was run on each CATI server under the root account to enable logging:

`auditctl -a exit,always -F dir=/tmp -F perm=war`

This can be changed permanently by modifying the /etc/audit/audit.rules file. Just add this line:

`-a exit,always -F dir=/tmp -F perm=war`

to the end of the file and then run:

`service auditd reload`

For example, if you then run:

`chmod 755 /tmp`

Then this entry appears in the log file.

`type=SYSCALL msg=audit(1259087952.185:965): arch=40000003 syscall=15 success=yes exit=0 a0=83c18b0 a1=1ed a2=8051614 a3=0 items=1 ppid=31529 pid=31586 auid=500 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=pts1 ses=159 comm="chmod" exe="/bin/chmod" subj=user_u:system_r:unconfined_t:s0 key=(null)`

`type=CWD msg=audit(1259087952.185:965):  cwd="/root"`

`type=PATH msg=audit(1259087952.185:965): item=0 name="/tmp" inode=553249 dev=fd:00 mode=041777 ouid=0 ogid=0 rdev=00:00 obj=system_u:object_r:tmp_t:s0`

If you then run:

`chmod 777 /tmp` 

It adds this entry to the log file:

`type=SYSCALL msg=audit(1259088048.908:966): arch=40000003 syscall=15 success=yes exit=0 a0=87778b0 a1=1ff a2=8051614 a3=0 items=1 ppid=31529 pid=31591 auid=500 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=pts1 ses=159 comm="chmod" exe="/bin/chmod" subj=user_u:system_r:unconfined_t:s0 key=(null)`

`type=CWD msg=audit(1259088048.908:966):  cwd="/root"`

`type=PATH msg=audit(1259088048.908:966): item=0 name="/tmp" inode=553249 dev=fd:00 mode=040755 ouid=0 ogid=0 rdev=00:00 obj=system_u:object_r:tmp_t:s0`

You can see that it logs what the value of file permissions were before the change was made (as you can then compare it to what the file permissions are current for that folder).

In order to view human readable timestamps in the aduit log, you need to run this perl command which converts the timestamp value while leaving the rest of the log file untouched:

`perl -pe 's/\d+/localtime($&)/e' /var/log/audit/audit.log`
