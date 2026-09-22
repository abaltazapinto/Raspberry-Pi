# Pi-hole over Tailscale — Learning Guide

This is the condensed reference for the 2026-09-18 DNS incident.

## Topology

```text
Debian baltazar
100.92.44.113
      |
      | Tailscale
      v
Raspberry Pi
100.116.30.48
      |
      +--> Pi-hole :53
      |
      +--> 1.1.1.1 / 1.0.0.1
```

## Current Raspberry Pi DNS configuration

```bash
sudo pihole-FTL --config dns.listeningMode
# ALL

sudo pihole-FTL --config dns.upstreams
# [ 1.1.1.1, 1.0.0.1 ]
```

## Prove Pi-hole locally

```bash
dig @127.0.0.1 pi.hole
dig @127.0.0.1 github.com
```

The first tests local Pi-hole DNS.

The second forces Pi-hole to use an upstream resolver.

## Prove Pi-hole remotely through Tailscale

From another Tailscale device:

```bash
dig @100.116.30.48 github.com
```

Expected:

```text
status: NOERROR
SERVER: 100.116.30.48#53
ANSWER SECTION: ...
```

## Prove that packets reach the Raspberry Pi

```bash
sudo tcpdump -ni tailscale0 port 53
```

Useful interpretation:

```text
query appears, no reply
-> packet reached Raspberry; investigate Pi-hole/host/upstream

nothing appears
-> investigate Tailscale path / ACL / client
```

## Check who owns DNS port 53

```bash
sudo ss -lntup 'sport = :53'
```

On this Raspberry Pi the expected process is:

```text
pihole-FTL
```

## Check Raspberry route to Internet

```bash
ip route get 1.1.1.1
```

Known result during incident:

```text
1.1.1.1 via 192.168.1.1 dev wlan0 src 192.168.1.21
```

Meaning:

```text
gateway   = 192.168.1.1
interface = wlan0
source IP = 192.168.1.21
```

## Test DNS upstream directly

UDP:

```bash
dig @1.1.1.1 github.com
```

TCP:

```bash
dig +tcp @1.1.1.1 github.com
```

The distinction is useful because DNS normally uses UDP, but can also use TCP.

## Change Pi-hole upstream DNS

Current working configuration:

```bash
sudo pihole-FTL --config dns.upstreams '[ "1.1.1.1", "1.0.0.1" ]'
```

Verify:

```bash
sudo pihole-FTL --config dns.upstreams
```

## Debian DNS workaround used during incident

When Debian's `/etc/resolv.conf` was controlled by Tailscale:

```text
nameserver 100.100.100.100
```

DNS failed.

Temporary fix:

```bash
sudo tailscale set --accept-dns=false
```

Then NetworkManager restored normal DNS.

Important:

```text
Tailscale networking remains available.
Tailscale/MagicDNS automatic DNS is disabled on that client.
```

## The diagnostic sequence to memorize

```bash
ping 1.1.1.1
getent hosts github.com
cat /etc/resolv.conf

tailscale status
tailscale ping 100.116.30.48

dig @100.100.100.100 github.com
dig @100.116.30.48 github.com

sudo ss -lntup 'sport = :53'
sudo tcpdump -ni tailscale0 port 53

dig @127.0.0.1 pi.hole
dig @127.0.0.1 github.com

sudo pihole-FTL --config dns.upstreams
dig @1.1.1.1 github.com
dig +tcp @1.1.1.1 github.com

ip route get 1.1.1.1
```

## Mental model

```text
Application
   |
   v
Name resolution
   |
   v
DNS server
   |
   v
Network path
   |
   v
DNS upstream
   |
   v
Authoritative Internet DNS
```

Test one layer at a time.

## Important caveat from this incident

The switch from Quad9:

```text
9.9.9.9
149.112.112.112
```

to Cloudflare:

```text
1.1.1.1
1.0.0.1
```

restored Pi-hole external resolution.

However, the incident did not conclusively prove that Quad9 itself was the root cause. There were transient DNS/UDP observations during troubleshooting.

Record the evidence, not a stronger conclusion than the evidence supports.

## Future task

Configure Tailscale DNS/MagicDNS so Tailscale clients automatically use:

```text
100.116.30.48
```

as Pi-hole DNS, then verify with:

```bash
getent hosts github.com
cat /etc/resolv.conf
```

without needing:

```bash
dig @100.116.30.48 ...
```
