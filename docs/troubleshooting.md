# Home Network Privacy Setup - Troubleshooting Guide

## Setup Overview
- **Pi1** (192.168.4.214) - Primary Pi-hole + Unbound
- **Pi2** (192.168.4.216) - Secondary Pi-hole backup
- **Router** (192.168.4.1) - Eero
- **DNS Flow**: Devices → Pi-hole (port 53) → Unbound (port 5335) → Root DNS

## Issues Fixed

### 1. SSH Rule Duplicating
**Problem**: allow-ssh.service added duplicate iptables rules on every restart.
**Fix**: Made service idempotent using -C flag to check before adding.

### 2. SSH Drops from Mac
**Problem**: Mac switching between WiFi (en1) and ethernet (en7) dropped SSH sessions.
**Fix**: Set static IP on ethernet adapter + added ~/.ssh/config with BindAddress.

### 3. Samsung Losing Internet
**Problem**: Samsung had same IP as Pi1 (192.168.4.214) causing conflict.
**Fix**: Changed Samsung to 192.168.4.218.

### 4. Samsung SSL Certificate Errors
**Problem**: All HTTPS sites showed NET::ERR_CERT_COMMON_NAME_INVALID with *.eero.com certificate.
**Fix**: Disabled IPv6 in Eero settings. IPv6 was bypassing Pi-hole entirely.

### 5. iPhone No Internet
**Problem**: Tailscale VPN was intercepting all DNS and traffic.
**Fix**: Disconnected Tailscale when on home network.

### 6. ADP Mobile Not Working
**Problem**: Caused by IPv6 bypass through Eero.
**Fix**: Fixed by disabling IPv6 in Eero settings.

## Key Settings

### Eero Router
- DNS Primary: 192.168.4.214 (Pi-hole)
- DNS Secondary: 192.168.4.216 (Pi2)
- IPv6: OFF (important - causes SSL interception if enabled)

### Samsung Galaxy S25
- Private DNS: Off
- DNS 1: 192.168.4.214
- DNS 2: 192.168.4.216

### Mac SSH Config (~/.ssh/config)
```
Host 192.168.4.214
    BindAddress 192.168.4.210
    ServerAliveInterval 30
    ServerAliveCountMax 3
```
