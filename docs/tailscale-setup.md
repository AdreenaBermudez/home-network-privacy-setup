# Tailscale Setup Guide
## Remote Access Configuration

---

## What is Tailscale?
Tailscale creates a secure private network between all your devices no matter where they are. It extends your Pi-hole protection to work everywhere — not just at home.

---

## Installation

### Install on Raspberry Pi
```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up --accept-dns=false --ssh
```

### Authenticate
The command will provide a URL — open it in your browser and sign in with your GitHub account.

---

## Device Connections

### Tailscale IP Addresses
| Device | Tailscale IP | Home IP |
|---|---|---|
| Pi1 (Primary) | 100.101.235.58 | 192.168.4.214 |
| Pi2 (Backup) | 100.109.243.96 | 192.168.4.216 |

---

## Web Dashboards

### At Home (Home IP)
| Dashboard | URL |
|---|---|
| Pi-hole Pi1 | http://192.168.4.214/admin |
| Pi-hole Pi2 | http://192.168.4.216/admin |
| AdGuard Pi1 | http://192.168.4.214:3000 |
| AdGuard Pi2 | http://192.168.4.216:3000 |

### Remote Access (Tailscale IP)
| Dashboard | URL |
|---|---|
| Pi-hole Pi1 | http://100.101.235.58/admin |
| Pi-hole Pi2 | http://100.109.243.96/admin |
| AdGuard Pi1 | http://100.101.235.58:3000 |
| AdGuard Pi2 | http://100.109.243.96:3000 |

---

## SSH Access

### From Windows
```cmd
ssh YOUR_USER@100.101.235.58
ssh YOUR_USER@100.109.243.96
```

### From Mac/Linux
```bash
ssh YOUR_USER@100.101.235.58
ssh YOUR_USER@100.109.243.96
```

---

## Configure Pi-hole as Tailscale DNS

1. Go to https://login.tailscale.com/admin/dns
2. Click **Add nameserver → Custom**
3. Enter Pi1 Tailscale IP: `100.101.235.58`
4. Add Pi2 as backup: `100.109.243.96`
5. Enable **Override local DNS** ✅

---

## Install on Other Devices

### iPhone/iPad
- Download Tailscale from App Store
- Sign in with GitHub account
- Toggle ON and allow VPN permission

### Windows
- Download from https://tailscale.com/download/windows
- Install and sign in with GitHub account
- Enable Launch at Login in Preferences

### Mac
- Download from https://tailscale.com/download/mac
- Sign in with GitHub account
- Enable Launch at Login in menu bar

### Android
- Download from Google Play Store
- Sign in with GitHub account
- Toggle ON and allow VPN permission

---

## Verification

### Check Tailscale Status
```bash
tailscale status
```

### Test Connection
```cmd
ping 100.101.235.58
```

### Test DNS Leak
Visit: https://dnsleaktest.com
Should only show Sonic.net with no leaks.

### Test Ad Blocking
Visit: https://adblock-tester.com
Should show 96/100 score.

---

## Important Notes
- Always use Tailscale IPs (100.x.x.x) when away from home
- Always use Home IPs (192.168.x.x) when on home WiFi
- Make sure Tailscale is set to Launch at Login on all devices
- Always sign in with the same GitHub account on all devices
