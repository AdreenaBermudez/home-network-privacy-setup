# Installation Guide
## Home Network Privacy Setup

---

## Prerequisites
- Raspberry Pi (any model with WiFi)
- MicroSD card (16GB or larger)
- Raspberry Pi OS installed
- SSH access enabled
- Internet connection

---

## System Update
```bash
sudo apt update && sudo apt upgrade -y
```

---

## Step 1 — Install Pi-hole

```bash
curl -sSL https://install.pi-hole.net | sudo bash
```

### Installer Selections
| Option | Selection |
|---|---|
| Interface | wlan0 |
| Upstream DNS | Google (temporary) |
| Install web interface | Yes |
| Install lighttpd | No |
| Enable logging | Yes |
| Privacy mode | Option 0 (Show everything) |

### Change Pi-hole Password
```bash
pihole setpassword
```

### Configure Pi-hole Listening Mode
```bash
sudo nano /etc/pihole/pihole.toml
```
Set the following:
```toml
[dns]
  listeningMode = "ALL"
  upstreams = ["127.0.0.1#5353"]
```

---

## Step 2 — Install Unbound

```bash
sudo apt install unbound -y
```

### Create Unbound Config
```bash
sudo nano /etc/unbound/unbound.conf.d/pi-hole.conf
```

Paste the following:
```
server:
    verbosity: 0
    interface: 127.0.0.1
    port: 5335
    do-ip4: yes
    do-udp: yes
    do-tcp: yes
    do-ip6: no
    prefer-ip6: no
    harden-glue: yes
    harden-dnssec-stripped: yes
    use-caps-for-id: no
    edns-buffer-size: 1232
    prefetch: yes
    num-threads: 1
    so-rcvbuf: 1m
    private-address: 192.168.0.0/16
    private-address: 169.254.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
    private-address: fd00::/8
    private-address: fe80::/10
```

### Start and Enable Unbound
```bash
sudo systemctl start unbound
sudo systemctl enable unbound
sudo systemctl status unbound
```

### Test Unbound
```bash
dig google.com @127.0.0.1 -p 5335
```

---

## Step 3 — Install AdGuard Home

```bash
curl -s -S -L https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sudo sh -s -- -v
```

### Configure AdGuard Home
```bash
sudo systemctl stop AdGuardHome
sudo nano /opt/AdGuardHome/AdGuardHome.yaml
```

Add the following configuration:
```yaml
http:
  address: 0.0.0.0:3000
dns:
  bind_hosts:
    - 0.0.0.0
  port: 5353
  upstream_dns:
    - 127.0.0.1:5335
```

### Fix Permissions
```bash
sudo chmod 700 /opt/AdGuardHome
```

### Start AdGuard Home
```bash
sudo systemctl start AdGuardHome
sudo systemctl enable AdGuardHome
sudo systemctl status AdGuardHome
```

---

## Step 4 — Disable Avahi Daemon
```bash
sudo systemctl stop avahi-daemon.socket
sudo systemctl stop avahi-daemon
sudo systemctl disable avahi-daemon.socket
sudo systemctl disable avahi-daemon
```

---

## Step 5 — Add Block Lists to Pi-hole

Go to Pi-hole dashboard → **Lists → Adlists** and add:
```
https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts
https://adaway.org/hosts.txt
https://v.firebog.net/hosts/Easylist.txt
https://v.firebog.net/hosts/Easyprivacy.txt
https://v.firebog.net/hosts/AdguardDNS.txt
```

### Update Gravity
```bash
pihole -g
```

---

## Step 6 — Install Tailscale

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

### Authenticate Tailscale
```bash
sudo tailscale up --accept-dns=false --ssh
```
Open the URL provided in your browser to authenticate.

### Verify Tailscale Status
```bash
sudo tailscale status
```

---

## Step 7 — Configure Eero Router

In the Eero app:
- Go to **Settings → Network Settings → DNS**
- Set **Primary DNS:** Pi1 IP address
- Set **Secondary DNS:** Pi2 IP address

---

## Verification Commands

### Test Full DNS Chain
```bash
dig google.com @127.0.0.1 -p 53
dig google.com @127.0.0.1 -p 5353
dig google.com @127.0.0.1 -p 5335
```

### Test Ad Blocking
```bash
dig doubleclick.net @127.0.0.1 -p 53
# Should return 0.0.0.0
```

### Check All Services
```bash
sudo systemctl status pihole-FTL
sudo systemctl status AdGuardHome
sudo systemctl status unbound
```

### Check Ports
```bash
sudo ss -tulnp | grep -E ':53|:5353|:5335'
```
