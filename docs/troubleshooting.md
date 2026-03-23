\# Troubleshooting Guide

\## Home Network Privacy Setup



\---



\## Pi-hole Issues



\### Permission Denied on pihole status

\*\*Error:\*\*

```

/usr/local/bin/pihole: line 38: /etc/pihole/versions: Permission denied

```

\*\*Fix:\*\*

```bash

sudo chmod 644 /etc/pihole/versions

sudo chmod 755 /etc/pihole

sudo chown root:root /etc/pihole/versions

```



\---



\### Pi-hole Web Interface Shows 403 Forbidden

\*\*Cause:\*\* Pi-hole v6 uses its own built-in web server — lighttpd is not needed.



\*\*Fix:\*\*

```bash

sudo rm -rf /var/www/html/admin

sudo systemctl stop lighttpd

sudo systemctl disable lighttpd

sudo systemctl restart pihole-FTL

```



\---



\### Pi-hole Web Interface Not Loading

\*\*Check Pi-hole FTL is running:\*\*

```bash

sudo systemctl status pihole-FTL

sudo systemctl restart pihole-FTL

```



\*\*Check correct URL:\*\*

```

http://YOUR\_PI\_IP/admin

```

Note: Use http not https, and include /admin



\---



\### DNS Not Resolving

\*\*Test DNS chain:\*\*

```bash

dig google.com @127.0.0.1 -p 53

dig google.com @127.0.0.1 -p 5353

dig google.com @127.0.0.1 -p 5335

```



\*\*Check listening mode in config:\*\*

```bash

sudo cat /etc/pihole/pihole.toml | grep listeningMode

```

Should show:

```

listeningMode = "ALL"

```



\---



\## AdGuard Home Issues



\### AdGuard Home Not Starting - Port Conflict

\*\*Error:\*\*

```

listen tcp 0.0.0.0:80: bind: address already in use

```

\*\*Fix:\*\* Change AdGuard web port to 3000 in config:

```bash

sudo nano /opt/AdGuardHome/AdGuardHome.yaml

```

Set:

```yaml

http:

&#x20; address: 0.0.0.0:3000

```



\---



\### AdGuard Home DNS Port Conflict

\*\*Error:\*\*

```

listen tcp 0.0.0.0:53: bind: address already in use

```

\*\*Fix:\*\* Change AdGuard DNS port to 5353:

```bash

sudo nano /opt/AdGuardHome/AdGuardHome.yaml

```

Set:

```yaml

dns:

&#x20; port: 5353

```



\---



\### AdGuard Home mDNS Errors

\*\*Error:\*\*

```

write udp \[::]:5353->\[fe80::...]:5353: sendmsg: invalid argument

```

\*\*Cause:\*\* Avahi daemon conflicting with AdGuard on port 5353.



\*\*Fix:\*\*

```bash

sudo systemctl stop avahi-daemon.socket

sudo systemctl stop avahi-daemon

sudo systemctl disable avahi-daemon.socket

sudo systemctl disable avahi-daemon

```



\---



\## Unbound Issues



\### Unbound Not Resolving

\*\*Test Unbound directly:\*\*

```bash

dig google.com @127.0.0.1 -p 5335

```



\*\*Check Unbound status:\*\*

```bash

sudo systemctl status unbound

sudo systemctl restart unbound

```



\---



\## SSH Issues



\### Remote Host Identification Changed Warning

\*\*Error:\*\*

```

WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!

```

\*\*Fix:\*\*

```cmd

ssh-keygen -R YOUR\_PI\_IP

```

Then reconnect and type yes when prompted.



\---



\### SSH Connection Timeout

\*\*Check Pi is reachable:\*\*

```cmd

ping YOUR\_PI\_IP

```



\*\*Check SSH is running on Pi:\*\*

```bash

sudo systemctl status ssh

sudo systemctl start ssh

```



\---



\## Tailscale Issues



\### Tailscale SSH Timeout

\*\*Check Tailscale is running:\*\*

```cmd

tailscale status

```



\*\*Make sure you are using Tailscale IP not home IP:\*\*

```

Home IP:      192.168.x.x  (only works at home)

Tailscale IP: 100.x.x.x    (works everywhere)

```



\*\*Restart Tailscale:\*\*

```bash

sudo tailscale down

sudo tailscale up --accept-dns=false --ssh

```



\---



\### DNS Health Warning in Tailscale

\*\*Warning:\*\*

```

System DNS config not ideal. /etc/resolv.conf overwritten.

```

\*\*Fix:\*\*

```bash

sudo tailscale up --accept-dns=false

```



\---



\## Samsung Galaxy Issues



\### Internet May Not Be Available Warning

\*\*Cause:\*\* Android connectivity check domains being intercepted by Pi-hole.



\*\*Fix - Whitelist these domains in Pi-hole:\*\*

```bash

pihole allowlist connectivitycheck.gstatic.com

pihole allowlist connectivitycheck.android.com

pihole allowlist connectivitycheck.googleapis.com

pihole allowlist clients3.google.com

pihole allowlist clients.l.google.com

pihole allowlist android.clients.google.com

```



\---



\## General Network Issues



\### Check All Services Status

```bash

sudo systemctl status pihole-FTL

sudo systemctl status AdGuardHome

sudo systemctl status unbound

```



\### Check All Ports

```bash

sudo ss -tulnp | grep -E ':53|:5353|:5335|:80|:443|:3000'

```



\### Restart All Services

```bash

sudo systemctl restart pihole-FTL

sudo systemctl restart AdGuardHome

sudo systemctl restart unbound

```



\### Check Disk Space

```bash

df -h

```



\### Check Pi-hole Logs

```bash

sudo tail -20 /var/log/pihole/FTL.log

sudo tail -20 /var/log/pihole/pihole.log

```



\### Check AdGuard Logs

```bash

sudo journalctl -u AdGuardHome --no-pager -n 20

```

