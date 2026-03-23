# Tailscale Setup Guide
## Remote Access Configuration

---

## What is Tailscale?
Tailscale creates a secure private network between all your devices, no matter where they are. It extends your Pi-hole protection to work everywhere — not just at home.

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
| Pi1 (Primary) | 100.XXX.XXX.XXX | 192.XXX.X.XXX |
| Pi2 (Backup) | 100.XXX.XXX.XXX | 192.XXX.X.XXX |

---

## Web Dashboards

### At Home (Home IP)
| Dashboard | URL |
|---|---|
| Pi-hole Pi1 | http://192.XXX.X.XXX/admin |
| Pi-hole Pi2 | http://192.XXX.X.XXX/admin |
| AdGuard Pi1 | http://192.XXX.X.XXX:3000 |
| AdGuard Pi2 | http://192.XXX.X.XXX:3000 |

### Remote Access (Tailscale IP)
| Dashboard | URL |
|---|---|
| Pi-hole Pi1 | http://100.XXX.XXX.XX/admin |
| Pi-hole Pi2 | http:/100.XXX.XXX.XX/admin |
| AdGuard Pi1 | http://100.XXX.XXX.XX:3000 |
| AdGuard Pi2 | http://100.XXX.XXX.XX:3000 |

---

## SSH Access

### From Windows
```cmd
ssh YOUR_USER@100.XXX.XXX.XX
ssh YOUR_USER@100.XXX.XXX.XX
```

### From Mac/Linux
```bash
ssh YOUR_USER@100.XXX.XXX.XX
ssh YOUR_USER@100.XXX.XXX.XX
```

---

## Configure Pi-hole as Tailscale DNS

1. Go to https://login.tailscale.com/admin/dns
2. Click **Add nameserver → Custom**
3. Enter Pi1 Tailscale IP: `100.XXX.XXX.XX`
4. Add Pi2 as backup: `100.XXX.XXX.XX`
5. Enable **Override local DNS** ✅

---

## Install on Other Devices

### iPhone/iPad
- Download Tailscale from the App Store
- Sign in with a GitHub account
- Toggle ON and allow VPN permission

### Windows
- Download from https://tailscale.com/download/windows
- Install and sign in with a GitHub account
- Enable Launch at Login in Preferences

### Mac
- Download from https://tailscale.com/download/mac
- Sign in with a GitHub account
- Enable Launch at Login in the menu bar

### Android
- Download from the Google Play Store
- Sign in with a GitHub account
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
Should show a high score.

---

## Important Notes
- Always use Tailscale IPs (100.x.x.x) when away from home
- Always use Home IPs (192.168.x.x) when on home WiFi
- Make sure Tailscale is set to Launch at Login on all devices
- Always sign in with the same GitHub account on all devices
