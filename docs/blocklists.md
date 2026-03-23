\# Block Lists

\## Pi-hole Ad Blocking Configuration



\---



\## Current Block Lists



| List | URL | Domains Blocked |

|---|---|---|

| StevenBlack hosts | https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts | 86,064 |

| Adaway | https://adaway.org/hosts.txt | 6,540 |

| Easylist | https://v.firebog.net/hosts/Easylist.txt | 57,002 |

| Easyprivacy | https://v.firebog.net/hosts/Easyprivacy.txt | 42,503 |

| AdguardDNS | https://v.firebog.net/hosts/AdguardDNS.txt | 161,982 |

| \*\*Total Unique\*\* | | \*\*258,089\*\* |



\---



\## How to Add Block Lists



1\. Go to Pi-hole dashboard → \*\*Lists → Adlists\*\*

2\. Paste URL into \*\*Address\*\* box

3\. Add a comment describing the list

4\. Click \*\*Add blocklist\*\*

5\. Run gravity update:

```bash

pihole -g

```



\---



\## Whitelisted Domains



These domains are whitelisted to ensure Android devices work correctly:



\### Android Connectivity

```

connectivitycheck.gstatic.com

connectivitycheck.android.com

connectivitycheck.googleapis.com

clients3.google.com

clients.l.google.com

android.clients.google.com

```



\---



\## Update Block Lists



Run this command to update all block lists:

```bash

pihole -g

```



It is recommended to update block lists at least once a month to ensure the latest ad and tracker domains are blocked.



\---



\## Check Block List Status

```bash

pihole -g status

```



\---



\## Test Ad Blocking

```bash

dig doubleclick.net @127.0.0.1 -p 53

\# Should return 0.0.0.0

```



Visit https://adblock-tester.com for a full ad blocking score test.



\---



\## Ad Blocking Score

\- Current score: \*\*96/100\*\*

\- Tested with Brave browser + Pi-hole + AdGuard Home

