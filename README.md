# Awesome Self-Host VPN [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of open-source tools, methodologies, configurations, benchmarks and resources for **self-hosting your own VPN** on a VPS — WireGuard, OpenVPN, kill-switches, multi-peer, monitoring.

Self-hosting a VPN gives you complete control of the server, the logs (or absence of them), and the cost (~5€/month on a VPS vs ~3€/month on a commercial subscription with way more limits). This list collects everything needed to **deploy, harden and monitor** a self-hosted VPN reliably.

Maintained by [VPNSmith](https://www.vpnsmith.com) — independent tutorials and benchmarks for self-hosted VPN on VPS.

## Contents

- [VPN protocols](#vpn-protocols)
- [VPS hosting providers](#vps-hosting-providers)
- [Setup tutorials](#setup-tutorials)
- [Reference configs](#reference-configs)
- [Kill switches](#kill-switches)
- [Monitoring & observability](#monitoring--observability)
- [Hardening guides](#hardening-guides)
- [Benchmarks & studies](#benchmarks--studies)
- [Multi-peer / mesh VPN](#multi-peer--mesh-vpn)
- [Mobile clients](#mobile-clients)
- [Bypassing DPI / restrictive firewalls](#bypassing-dpi--restrictive-firewalls)
- [Useful CLIs / scripts](#useful-clis--scripts)
- [Communities](#communities)
- [Related awesome lists](#related-awesome-lists)
- [Contributing](#contributing)

## VPN protocols

| Protocol | Year | Cipher | Audited | Speed (vs raw) | Use case |
|----------|------|--------|---------|----------------|----------|
| [WireGuard](https://www.wireguard.com/) | 2016 | ChaCha20-Poly1305 | Cure53 (2018), Trail of Bits (2020) | -4% | Default modern choice |
| [OpenVPN](https://openvpn.net/community/) | 2001 | AES-GCM (configurable) | Multiple audits 2004-2024 | -27% UDP, -59% TCP | TCP-only environments, port 443 obfuscation |
| [IPsec](https://strongswan.org/) | 1995 | AES, ChaCha20 | Heavy | -8 to -15% | Enterprise, site-to-site, IKEv2 mobile |
| [OpenConnect](https://www.infradead.org/openconnect/) | 2008 | AES-GCM | Active | -20% | Cisco AnyConnect compatibility |
| [SoftEther](https://www.softether.org/) | 2014 | AES-256 | Limited | -30% | Multi-protocol gateway |

Per-protocol benchmark on Contabo VPS: see [VPNSmith WireGuard vs OpenVPN 100-run iperf3 study](https://www.vpnsmith.com/en/blog/wireguard-vs-openvpn-vps-benchmarks-2026) (Zenodo DOI `10.5281/zenodo.20512442`).

## VPS hosting providers

Providers known to allow VPN traffic (not all do — check ToS):

| Provider | Region | Price floor | Bandwidth | VPN-friendly |
|----------|--------|-------------|-----------|--------------|
| [Contabo](https://contabo.com) | EU/US | €4.50/mo | Unmetered (200 Mbps fair use) | ✅ explicit |
| [Hetzner](https://www.hetzner.com) | EU/US | €4.51/mo | 20 TB included | ✅ ToS-allowed |
| [OVH](https://www.ovhcloud.com) | EU | €3.50/mo | Unlimited | ✅ ToS-allowed |
| [Vultr](https://www.vultr.com) | Global | $5/mo | 1-2 TB | ✅ explicit |
| [DigitalOcean](https://www.digitalocean.com) | Global | $4/mo | 500 GB-4 TB | ✅ ToS-allowed |
| [Linode (Akamai)](https://www.linode.com) | Global | $5/mo | 1-11 TB | ✅ ToS-allowed |

Detailed pricing + RAM/CPU comparison: [VPNSmith VPS comparator](https://www.vpnsmith.com/fr/blog/contabo-vs-hetzner-vs-ovh-vps-vpn-2026).

## Setup tutorials

### WireGuard

- [VPNSmith — Self-host WireGuard on Contabo step by step (Ubuntu 22.04)](https://www.vpnsmith.com/en/blog/self-host-vpn-contabo-wireguard-2026) — complete walkthrough with firewall, DNS, and kill-switch
- [WireGuard official quickstart](https://www.wireguard.com/quickstart/) — minimal config
- [Algo VPN](https://github.com/trailofbits/algo) — automated Ansible playbook for WireGuard + IPsec
- [pivpn](https://github.com/pivpn/pivpn) — interactive installer for Raspberry Pi / Ubuntu

### OpenVPN

- [openvpn-install](https://github.com/angristan/openvpn-install) — one-script setup, mature project
- [Road Warrior installer](https://github.com/nyr/openvpn-install) — alternative one-script setup

### IPsec / strongSwan

- [strongSwan official wiki](https://docs.strongswan.org/) — reference documentation
- [IKEv2-server setup on Ubuntu](https://github.com/jawj/IKEv2-setup) — automated script

## Reference configs

| Source | Notes |
|--------|-------|
| [VPNSmith config templates](https://www.vpnsmith.com/en/blog/wireguard-config-templates-2026) | Server + client examples for home VPN, travel VPN, kill-switch strict, multi-peer |
| [WireGuard wiki - example configs](https://www.wireguard.com/quickstart/) | Minimal upstream examples |
| [Awesome WireGuard configs](https://github.com/awesome-wireguard/configs) | Community curated configs |

## Kill switches

- [VPNSmith — Linux VPN kill switch with iptables + systemd](https://www.vpnsmith.com/en/blog/vpn-killswitch-linux-iptables-systemd) — production-grade systemd-ordered drop policy
- [killswitch (script)](https://github.com/Nyr/killswitch) — minimal bash kill-switch

## Monitoring & observability

- [wireguard_exporter](https://github.com/mindflavor/prometheus_wireguard_exporter) — Prometheus exporter for WireGuard peers
- [VPNSmith — Monitor your VPS VPN with Prometheus + Grafana](https://www.vpnsmith.com/en/blog/vps-vpn-monitoring-prometheus-grafana) — full dashboard + alerts setup

## Hardening guides

- [Linux server hardening checklist (Lynis)](https://github.com/CISOfy/lynis) — security auditing tool
- [SSH hardening — Mozilla guidelines](https://infosec.mozilla.org/guidelines/openssh)
- [UFW best practices](https://help.ubuntu.com/community/UFW) — firewall basics
- [DNSCrypt-proxy](https://github.com/DNSCrypt/dnscrypt-proxy) — encrypt DNS to upstream resolvers

## Benchmarks & studies

- [VPNSmith — WireGuard vs OpenVPN 100-run iperf3 benchmark 2026](https://www.vpnsmith.com/en/blog/wireguard-vs-openvpn-vps-benchmarks-2026) — Contabo VPS Paris → Nuremberg, reproducible CSV, Zenodo DOI `10.5281/zenodo.20512442`
- [Phoronix VPN benchmarks](https://www.phoronix.com/scan.php?page=search&q=wireguard) — long-running open benchmark series

## Multi-peer / mesh VPN

- [Tailscale](https://tailscale.com/) — proprietary but free tier, NAT traversal handled
- [Headscale](https://github.com/juanfont/headscale) — open-source Tailscale control server
- [Nebula](https://github.com/slackhq/nebula) — Slack's mesh VPN, P2P routing
- [Netmaker](https://github.com/gravitl/netmaker) — open-source WireGuard mesh
- [innernet](https://github.com/tonarino/innernet) — private mesh VPN by Tonari

## Mobile clients

- [WireGuard for Android](https://play.google.com/store/apps/details?id=com.wireguard.android) — official client
- [WireGuard for iOS](https://apps.apple.com/app/wireguard/id1441195209) — official client
- [OpenVPN Connect](https://openvpn.net/client/) — official mobile client
- [OpenVPN for Android (community)](https://github.com/schwabe/ics-openvpn) — open-source alternative

## Bypassing DPI / restrictive firewalls

- [wstunnel](https://github.com/erebe/wstunnel) — tunnel TCP/UDP over WebSocket (for firewalls blocking UDP)
- [shadowsocks-rust](https://github.com/shadowsocks/shadowsocks-rust) — obfuscated proxy
- [Cloak](https://github.com/cbeuw/Cloak) — disguise traffic as HTTPS
- [V2Ray](https://github.com/v2ray/v2ray-core) — multi-protocol obfuscation framework

## Useful CLIs / scripts

- [wg-quick](https://git.zx2c4.com/wireguard-tools/about/) — official WireGuard config helper (`wg-quick up wg0`)
- [VPNSmith split-tunnel ip-route scripts](https://www.vpnsmith.com/en/blog/split-tunnel-vpn-routing-tables) — fwmark-based application routing
- [easyrsa](https://github.com/OpenVPN/easy-rsa) — OpenVPN PKI tool

## Communities

- [/r/selfhosted](https://www.reddit.com/r/selfhosted/) — broad self-hosting community, VPN threads frequent
- [/r/WireGuard](https://www.reddit.com/r/WireGuard/) — protocol-specific
- [/r/OpenVPN](https://www.reddit.com/r/OpenVPN/) — protocol-specific
- [Hacker News : "Show HN: VPN"](https://hn.algolia.com/?q=self-host+vpn) — long-running discussions
- [Lemmy.world /c/selfhost](https://lemmy.world/c/selfhost) — Fediverse alternative

## Related awesome lists

- [awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted) — broad self-hosting alternatives
- [awesome-vpn-leak-detection](https://github.com/ricco020/awesome-vpn-leak-detection) — testing your VPN once deployed
- [awesome-privacy](https://github.com/Lissy93/awesome-privacy) — broader privacy stack
- [awesome-shadowsocks](https://github.com/shadowsocks/awesome-shadowsocks) — obfuscation focus
- [awesome-hardening](https://github.com/decalage2/awesome-security-hardening) — server hardening

## Contributing

Contributions are welcome. Please read the [contribution guidelines](CONTRIBUTING.md) first.

The criteria for inclusion:
- Tool, paper, or resource directly related to **self-hosting** a VPN (not commercial VPN reviews — those go in `awesome-privacy`)
- Open-source preferred (closed-source flagged explicitly)
- Active maintenance (last release < 2 years) OR seminal documentation
- No affiliate spam — VPS providers are listed objectively; affiliate disclosures, when relevant, are made on the linked tutorial pages, not in this list

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the maintainers have waived all copyright and related or neighboring rights to this work.
