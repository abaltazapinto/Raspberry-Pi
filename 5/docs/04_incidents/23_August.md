Incident 2026-08-22

CONFIRMED:
- Raspberry OS remained running.
- tailscaled remained running.
- Tailscale reported "network is unreachable".
- NetworkManager attempted to configure eth0.
- DHCPv4 transaction failed after ~45 seconds.
- eth0 reported ip-config-unavailable.
- No DHCP lease was obtained.
- Loss of network connectivity consequently made
  Tailscale, SSH, Nextcloud and Navidrome unreachable.

ROOT CAUSE LEVEL IDENTIFIED:
Ethernet/DHCP configuration layer.

UNDERLYING CAUSE:
Not yet established.
