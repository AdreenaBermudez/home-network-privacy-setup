# Maintenance Guide
## Home Network Privacy Setup

---

## 🗓️ Maintenance Schedule

| Task | Frequency | Time Required |
|---|---|---|
| Update Pi-hole block lists | Weekly | 2 minutes |
| Update Raspberry Pi OS | Monthly | 10 minutes |
| Update Pi-hole software | Monthly | 5 minutes |
| Update AdGuard Home | Monthly | 5 minutes |
| Check all services running | Weekly | 2 minutes |
| Check disk space | Monthly | 1 minute |
| Verify DNS chain working | Weekly | 2 minutes |
| Test ad blocking score | Monthly | 5 minutes |
| Test DNS leak | Monthly | 5 minutes |
| Check Tailscale connected | Weekly | 1 minute |
| Reboot both Pis | Monthly | 5 minutes |
| Review Pi-hole query logs | Weekly | 5 minutes |
| Backup configuration files | Monthly | 10 minutes |

---

## 📅 Weekly Maintenance

### 1 — Update Pi-hole Block Lists
Run on both Pi1 and Pi2:
```bash
pihole -g
```
This downloads the latest versions of all block lists ensuring new ad and tracker domains are blocked.

### 2 — Check All Services Are Running
Run on both Pi1 and Pi2:
```bash
sudo systemctl status pihole-FTL
sudo systemctl status AdGuardHome
sudo systemctl status unbound
```
All three should show **active (running)**

### 3 — Verify DNS Chain is Working
```bash
dig google.com @127.0.0.1 -p 53
dig google.com @127.0.0.1 -p 5353
dig google.com @127.0.0.1 -p 5335
```
All three should return a valid IP address.

### 4 — Verify Ad Blocking is Working
```bash
dig doubleclick.net @127.0.0.1 -p 53
```
Should return **0.0.0.0**

### 5 — Check Tailscale is Connected
```bash
sudo tailscale status
```
All devices should show as connected.

### 6 — Review Pi-hole Query Logs
Go to Pi-hole dashboard:
```
http://192.168.4.214/admin
```
Check for any unusual activity or devices you don't recognize.

---

## 📅 Monthly Maintenance

### 1 — Update Raspberry Pi OS
Run on both Pi1 and Pi2:
```bash
sudo apt update && sudo apt upgrade -y
```
This installs the latest security patches and bug fixes.

### 2 — Update Pi-hole Software
```bash
pihole -up
```

### 3 — Update AdGuard Home
```bash
sudo systemctl stop AdGuardHome
cd /opt/AdGuardHome
sudo ./AdGuardHome --update
sudo systemctl start AdGuardHome
sudo systemctl status AdGuardHome
```

### 4 — Update Tailscale
```bash
sudo apt update
sudo apt install tailscale -y
sudo tailscale version
```

### 5 — Check Disk Space
```bash
df -h
```
Make sure no partition is above **80% full**. If disk is getting full:
```bash
sudo apt autoremove -y
sudo apt clean
```

### 6 — Reboot Both Pis
Reboot Pi1:
```bash
sudo reboot
```
Wait 2 minutes then SSH back in and verify all services are running. Then reboot Pi2.

### 7 — Test Ad Blocking Score
Visit from any device:
```
https://adblock-tester.com
```
Should score **96/100** or higher.

### 8 — Test DNS Leak
Visit from any device:
```
https://dnsleaktest.com
```
Should only show **Sonic.net** with no leaks.

### 9 — Backup Configuration Files
Run on both Pi1 and Pi2:
```bash
# Backup Pi-hole config
sudo cp /etc/pihole/pihole.toml ~/pihole-backup-$(date +%Y%m%d).toml

# Backup Unbound config
sudo cp /etc/unbound/unbound.conf.d/pi-hole.conf ~/unbound-backup-$(date +%Y%m%d).conf

# Backup AdGuard config
sudo cp /opt/AdGuardHome/AdGuardHome.yaml ~/adguard-backup-$(date +%Y%m%d).yaml
```

### 10 — Check Eero Firmware is Updated
- Open Eero app
- Go to **Settings → Software Updates**
- Make sure auto updates is enabled ✅

---

## 🔧 Quick Health Check Script

Run this on both Pis for a complete health check:
```bash
echo "=== Service Status ==="
sudo systemctl status pihole-FTL --no-pager | grep Active
sudo systemctl status AdGuardHome --no-pager | grep Active
sudo systemctl status unbound --no-pager | grep Active

echo "=== DNS Chain Test ==="
dig google.com @127.0.0.1 -p 53 | grep "ANSWER SECTION" -A1
dig google.com @127.0.0.1 -p 5353 | grep "ANSWER SECTION" -A1
dig google.com @127.0.0.1 -p 5335 | grep "ANSWER SECTION" -A1

echo "=== Ad Blocking Test ==="
dig doubleclick.net @127.0.0.1 -p 53 | grep "ANSWER SECTION" -A1

echo "=== Disk Space ==="
df -h | grep -v tmpfs

echo "=== Tailscale Status ==="
sudo tailscale status
```

---

## 🚨 Warning Signs to Watch For

| Warning | What it means | Action |
|---|---|---|
| Service shows inactive | Service has crashed | Restart service |
| Disk above 80% full | Running out of space | Clean up disk |
| Ad block score drops below 90 | Block lists outdated | Run pihole -g |
| DNS leak test shows unknown servers | DNS leak detected | Check Pi-hole config |
| Tailscale shows devices offline | VPN disconnected | Restart Tailscale |
| Pi-hole shows 0 queries | DNS not routing through Pi-hole | Check Eero DNS settings |

---

## 🔄 Restart Commands

### Restart All Services
```bash
sudo systemctl restart pihole-FTL
sudo systemctl restart AdGuardHome
sudo systemctl restart unbound
```

### Restart Tailscale
```bash
sudo systemctl restart tailscaled
```

### Full Pi Reboot
```bash
sudo reboot
```

---

## 📊 Dashboards to Check Regularly

| Dashboard | URL | What to check |
|---|---|---|
| Pi-hole Pi1 | http://192.168.4.214/admin | Query stats, blocked domains |
| Pi-hole Pi2 | http://192.168.4.216/admin | Query stats, blocked domains |
| AdGuard Pi1 | http://192.168.4.214:3000 | Query log, blocked queries |
| AdGuard Pi2 | http://192.168.4.216:3000 | Query log, blocked queries |
| Tailscale | https://login.tailscale.com/admin | Connected devices |

---

## 🔐 Security Maintenance

### Check for Unknown Devices on Network
- Open Eero app
- Go to **Devices** tab
- Review all connected devices
- If you see anything unknown change your WiFi password immediately

### Update WiFi Password (Every 6 months)
- Open Eero app
- Go to **Settings → Network Settings**
- Update WiFi password
- Reconnect all devices

### Check Pi-hole for Suspicious Queries
In Pi-hole dashboard go to **Query Log** and look for:
- Unusual domains being queried frequently
- Unknown client IP addresses
- Large volumes of queries from a single device

---

## 📝 Maintenance Log

Keep a log of maintenance performed:

| Date | Task | Notes |
|---|---|---|
| 03/23/2026 | Initial setup | Complete home network privacy setup |
| 03/23/2026 | Samsung S25 fix | Whitelisted connectivity domains, reserved IP |
| | | |
| | | |

---

## 🆘 Emergency Contacts & Resources

| Resource | URL |
|---|---|
| Pi-hole documentation | https://docs.pi-hole.net |
| AdGuard Home documentation | https://github.com/AdguardTeam/AdGuardHome/wiki |
| Tailscale documentation | https://tailscale.com/kb |
| Unbound documentation | https://nlnetlabs.nl/documentation/unbound |
| Pi-hole community | https://discourse.pi-hole.net |
