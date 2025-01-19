## I.service ssh

## 1. analyse du service



S'assurer que le service sshd est démarré

```bash 
[unuser@web ~]$ systemctl status sshd
● sshd.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; preset: enabled)
     Active: active (running) since Fri 2024-11-29 23:40:05 CET; 35min ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 717 (sshd)
      Tasks: 1 (limit: 11084)
     Memory: 5.0M
        CPU: 63ms
     CGroup: /system.slice/sshd.service
```



analyser les processus liés au service ssh
```bash
root         717       1  0 Nov29 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root        1271     717  0 Nov29 ?        00:00:00 sshd: unuser [priv]
unuser      1275    1271  0 Nov29 ?        00:00:00 sshd: unuser@pts/0
unuser      1334    1276  0 00:06 pts/0    00:00:00 grep --color=auto sshd
```



Déterminer le port sur lequel écoute le service ssh




```bash
[unuser@web ~]$ sudo ss -tlnp
[sudo] password for unuser:
State     Recv-Q    Send-Q         Local Address:Port         Peer Address:Port    Process
LISTEN    0         128                  0.0.0.0:22                0.0.0.0:*        users:(("sshd",pid=717,fd=3))
LISTEN    0         128                     [::]:22                   [::]:*        users:(("sshd",pid=717,fd=4))
```

il écoute donc sur le port 22


```bash
[unuser@web ~]$ journalctl -u sshd
Nov 29 23:40:05 web.tp1.b1 systemd[1]: Starting OpenSSH server daemon...
Nov 29 23:40:05 web.tp1.b1 sshd[717]: Server listening on 0.0.0.0 port 22.
Nov 29 23:40:05 web.tp1.b1 sshd[717]: Server listening on :: port 22.
Nov 29 23:40:05 web.tp1.b1 systemd[1]: Started OpenSSH server daemon.
Nov 29 23:53:53 web.tp1.b1 sshd[1271]: Accepted password for unuser from 10.1.1.3 port 65387 ssh2
Nov 29 23:53:53 web.tp1.b1 sshd[1271]: pam_unix(sshd:session): session opened for user unuser(uid=1000) by unuser(ui>
```