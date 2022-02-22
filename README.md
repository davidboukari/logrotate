# logrotate


## Ex for logstash
```
$ cat  /etc/logrotate.d/logstash 
/var/log/logstash/logstash-*.log
{
      	daily
        rotate 7
	size 100M
        copytruncate
        compress
        delaycompress
        missingok
        notifempty
}


# -d: debug
$ logrotate -d /etc/logrotate.d/logstash 
reading config file /etc/logrotate.d/logstash
Allocating hash table for state file, size 15360 B

Handling 1 logs

rotating pattern: /var/log/logstash/logstash-*.log
 104857600 bytes (7 rotations)
empty log files are not rotated, old logs are removed
considering log /var/log/logstash/logstash-deprecation.log
  log does not need rotating (log size is below the 'size' threshold)
considering log /var/log/logstash/logstash-plain.log
  log does not need rotating (log size is below the 'size' threshold)
considering log /var/log/logstash/logstash-slowlog-plain.log
  log does not need rotating (log size is below the 'size' threshold)

$  ls -lahtr /var/log/logstash/
total 36K
-rw-r--r--. 1 logstash logstash    0 26 mars   2021 logstash-slowlog-plain.log
drwxr-xr-x. 8 root     root     4,0K 21 févr. 16:28 ..
-rw-r--r--. 1 logstash logstash 4,5K 21 févr. 20:28 logstash-deprecation.log
-rw-r--r--. 1 logstash logstash  22K 21 févr. 20:28 logstash-plain.log
drwxr-xr-x. 2 logstash root       98 21 févr. 21:24 .


````

## Logrotate & cron
* https://www.tecmint.com/install-logrotate-to-manage-log-rotation-in-linux/
```
Logrotate and Cron

By default, the installation of logrotate creates a crontab file inside /etc/cron.daily named logrotate. As it is the case with the other crontab files inside this directory, it will be executed daily starting at 6:25 am if anacron is not installed.
Suggested Read: 11 Cron Scheduling Task Examples in Linux

Otherwise, the execution will begin around 7:35 am. To verify, watch for the line containing cron.daily in either /etc/crontab or /etc/anacrontab.

```



## Others
```
[root@elasticsearch-1 ansible-roles]# ls /etc/logrotate.d/messages
/etc/logrotate.d/messages
[root@elasticsearch-1 ansible-roles]# cat /etc/logrotate.d/messages
/var/log/messages
{
        rotate 7
        size 100k
        weekly
        missingok
        notifempty
        compress
        delaycompress
        sharedscripts
        postrotate
                invoke-rc.d rsyslog rotate >/dev/null 2>&1 || true
        endscript
}

[root@elasticsearch-1 ansible-roles]# cat /etc/cron.daily/logrotate
#!/bin/sh

/usr/sbin/logrotate -s /var/lib/logrotate/logrotate.status /etc/logrotate.conf
EXITVALUE=$?
if [ $EXITVALUE != 0 ]; then
    /usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]"
fi
exit 0


[root@elasticsearch-1 ansible-roles]# cp /etc/cron.daily/logrotate /etc/cron.hourly/messages
[root@elasticsearch-1 ansible-roles]# cat /etc/cron.hourly/messages
#!/bin/sh

/usr/sbin/logrotate -s /var/lib/logrotate/logrotate.status /etc/logrotate.conf
EXITVALUE=$?
if [ $EXITVALUE != 0 ]; then
    /usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]"
fi
exit 0
```

## Others
```
cd /etc/logrotate.d/
ls
alternatives  apache2  apport  apt  bootlog  btmp  conntrackd  dpkg  rsyslog  ubuntu-advantage-tools  ufw  unattended-upgrades  wtmp
cp ufw iptables
vim iptables
cat iptables
/var/log/iptables.log
{
        rotate 7
        size 100k
        weekly
        missingok
        notifempty
        compress
        delaycompress
        sharedscripts
        postrotate
                invoke-rc.d rsyslog rotate >/dev/null 2>&1 || true
        endscript
}
systemctl restart logrotate
```


```
# rpm -qf /etc/logrotate.d
logrotate-3.8.6-19.el7.x86_64

# rpm -ql logrotate-3.8.6-19.el7.x86_64
/etc/cron.daily/logrotate
/etc/logrotate.conf
/etc/logrotate.d
/etc/rwtab.d/logrotate
/usr/sbin/logrotate
/usr/share/doc/logrotate-3.8.6
/usr/share/doc/logrotate-3.8.6/CHANGES
/usr/share/doc/logrotate-3.8.6/COPYING
/usr/share/man/man5/logrotate.conf.5.gz
/usr/share/man/man8/logrotate.8.gz
/var/lib/logrotate
/var/lib/logrotate/logrotate.status
```
