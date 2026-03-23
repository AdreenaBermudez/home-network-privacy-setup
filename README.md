\# Home Network Privacy Setup



A complete home network privacy and ad blocking setup.



\## Components

\- Pi-hole v6 (Primary and Backup)

\- AdGuard Home

\- Unbound DNS Resolver

\- Tailscale VPN



\## DNS Chain

Devices → Pi-hole :53 → AdGuard Home :5353 → Unbound :5335 → Internet



\## Results

\- 258,089 domains blocked

\- 96/100 ad blocking score

\- Zero DNS leaks

\- Automatic failover tested and working

\- Remote access via Tailscale

