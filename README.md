# Awesome Self-Host VPN [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of open-source tools, methodologies, configurations, benchmarks and resources for **self-hosting your own VPN** on a VPS, WireGuard, OpenVPN, kill-switches, multi-peer, monitoring.

Self-hosting a VPN gives you control of the server, of the logs (or their absence) and of the exit IP, at the price of doing the maintenance yourself. This list collects everything needed to **deploy, harden and monitor** a self-hosted VPN reliably.

Maintained by [VPNSmith](https://www.vpnsmith.com?src=github&ref=gh-awesome-self-host), independent tutorials for self-hosted VPN on VPS.

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
- [Web dashboards for WireGuard](#web-dashboards-for-wireguard)
- [Mobile clients](#mobile-clients)
- [Bypassing DPI / restrictive firewalls](#bypassing-dpi--restrictive-firewalls)
- [Useful CLIs / scripts](#useful-clis--scripts)
- [Communities](#communities)
- [Related awesome lists](#related-awesome-lists)
- [Contributing](#contributing)

## VPN protocols

| Protocol | Year | Cipher | Use case |
|----------|------|--------|----------|
| [WireGuard](https://www.wireguard.com/) | 2016 | ChaCha20-Poly1305 | Default modern choice |
| [OpenVPN](https://openvpn.net/community/) | 2001 | AES-GCM (configurable) | TCP-only environments, port 443 obfuscation |
| [IPsec](https://strongswan.org/) | 1995 | AES, ChaCha20 | Enterprise, site-to-site, IKEv2 mobile |
| [OpenConnect](https://www.infradead.org/openconnect/) | 2008 | AES-GCM | Cisco AnyConnect compatibility |
| [SoftEther](https://www.softether.org/) | 2014 | AES-256 | Multi-protocol gateway |

## VPS hosting providers

Check each provider's terms of service before running a VPN, and its current price page (prices and bandwidth change often):

- [Contabo](https://contabo.com)
- [Hetzner](https://www.hetzner.com)
- [OVHcloud](https://www.ovhcloud.com)
- [Vultr](https://www.vultr.com)
- [DigitalOcean](https://www.digitalocean.com)
- [Linode (Akamai)](https://www.linode.com)

Pricing and RAM/CPU comparison for a VPN workload: [VPNSmith VPS comparator](https://www.vpnsmith.com/en/blog/contabo-vs-hetzner-vs-ovh-vps-vpn-2026?src=github&ref=gh-awesome-self-host).

## Setup tutorials

### WireGuard

- [VPNSmith, Self-host WireGuard on Contabo step by step (Ubuntu 22.04)](https://www.vpnsmith.com/en/blog/self-host-vpn-contabo-wireguard-2026?src=github&ref=gh-awesome-self-host), complete walkthrough with firewall, DNS, and kill-switch
- [WireGuard official quickstart](https://www.wireguard.com/quickstart/), minimal config
- [Algo VPN](https://github.com/trailofbits/algo), automated Ansible playbook for WireGuard + IPsec
- [pivpn](https://github.com/pivpn/pivpn), interactive installer for Raspberry Pi / Ubuntu

### OpenVPN

- [openvpn-install](https://github.com/angristan/openvpn-install), one-script setup, mature project
- [Road Warrior installer](https://github.com/nyr/openvpn-install), alternative one-script setup

### IPsec / strongSwan

- [strongSwan official wiki](https://docs.strongswan.org/), reference documentation
- [IKEv2-server setup on Ubuntu](https://github.com/jawj/IKEv2-setup), automated script

## Reference configs

| Source | Notes |
|--------|-------|
| [VPNSmith config templates](https://www.vpnsmith.com/en/blog/wireguard-config-templates-2026?src=github&ref=gh-awesome-self-host) | Server + client examples for home VPN, travel VPN, kill-switch strict, multi-peer |
| [WireGuard wiki - example configs](https://www.wireguard.com/quickstart/) | Minimal upstream examples |

## Kill switches

- [VPNSmith, Linux VPN kill switch with iptables + systemd](https://www.vpnsmith.com/en/blog/vpn-killswitch-linux-iptables-systemd?src=github&ref=gh-awesome-self-host), production-grade systemd-ordered drop policy

## Monitoring & observability

- [wireguard_exporter](https://github.com/mindflavor/prometheus_wireguard_exporter), Prometheus exporter for WireGuard peers
- [VPNSmith, Monitor your VPS VPN with Prometheus + Grafana](https://www.vpnsmith.com/en/blog/vps-vpn-monitoring-prometheus-grafana?src=github&ref=gh-awesome-self-host), full dashboard + alerts setup

## Hardening guides

- [Linux server hardening checklist (Lynis)](https://github.com/CISOfy/lynis), security auditing tool
- [SSH hardening, Mozilla guidelines](https://infosec.mozilla.org/guidelines/openssh)
- [UFW best practices](https://help.ubuntu.com/community/UFW), firewall basics
- [DNSCrypt-proxy](https://github.com/DNSCrypt/dnscrypt-proxy), encrypt DNS to upstream resolvers

## Benchmarks & studies

- [Phoronix VPN benchmarks](https://www.phoronix.com/scan.php?page=search&q=wireguard), long-running open benchmark series

## Multi-peer / mesh VPN

- [Tailscale](https://tailscale.com/), proprietary but free tier, NAT traversal handled
- [Headscale](https://github.com/juanfont/headscale), open-source Tailscale control server
- [Nebula](https://github.com/slackhq/nebula), Slack's mesh VPN, P2P routing
- [Netmaker](https://github.com/gravitl/netmaker), open-source WireGuard mesh
- [innernet](https://github.com/tonarino/innernet), private mesh VPN by Tonari

## Web dashboards for WireGuard

Plain WireGuard has no user interface: every peer is a block of text in `wg0.conf` and a key pair you generate by hand. These tools manage peers, addresses and client configs for you. Pick by where the keys live.

| Tool | Runs where | Keys stored | Notes |
|------|-----------|-------------|-------|
| [wg-easy](https://github.com/wg-easy/wg-easy) | Docker on your server, next to WireGuard | your server | AGPL-3.0, web admin, QR codes, actively maintained |
| [wg-portal](https://github.com/h44z/wg-portal) | your server (Go binary or Docker) | your server | MIT, LDAP / Active Directory login, several interfaces |
| [wireguard-ui](https://github.com/ngoduykhanh/wireguard-ui) | your server (Go binary or Docker) | your server | MIT, writes `wg0.conf`, few commits since August 2024 |
| [PiVPN](https://github.com/pivpn/pivpn) | Raspberry Pi or Debian server | your server | MIT, command line only (`pivpn add`), no web UI |
| [WireGuard Manager](https://wg.vpnsmith.com/?ref=gh-awesome-self-host) (ours, by VPNSmith) | hosted, nothing to install | hosted, encrypted at rest | several servers in one account, next free IP per peer, `.conf` and QR download, imports an existing `wg0.conf`, free for 1 server and 3 peers, paid plan for more |

If you do not want a third party to hold private keys, use one of the self-hosted dashboards above.

## Mobile clients

- [WireGuard for Android](https://play.google.com/store/apps/details?id=com.wireguard.android), official client
- [WireGuard for iOS](https://apps.apple.com/app/wireguard/id1441195209), official client
- [OpenVPN Connect](https://openvpn.net/client/), official mobile client
- [OpenVPN for Android (community)](https://github.com/schwabe/ics-openvpn), open-source alternative

## Bypassing DPI / restrictive firewalls

- [wstunnel](https://github.com/erebe/wstunnel), tunnel TCP/UDP over WebSocket (for firewalls blocking UDP)
- [shadowsocks-rust](https://github.com/shadowsocks/shadowsocks-rust), obfuscated proxy
- [Cloak](https://github.com/cbeuw/Cloak), disguise traffic as HTTPS
- [V2Ray](https://github.com/v2ray/v2ray-core), multi-protocol obfuscation framework

## Useful CLIs / scripts

- [wg-quick](https://git.zx2c4.com/wireguard-tools/about/), official WireGuard config helper (`wg-quick up wg0`)
- [VPNSmith split-tunnel ip-route scripts](https://www.vpnsmith.com/en/blog/split-tunnel-vpn-routing-tables?src=github&ref=gh-awesome-self-host), fwmark-based application routing
- [easyrsa](https://github.com/OpenVPN/easy-rsa), OpenVPN PKI tool

## Communities

- [/r/selfhosted](https://www.reddit.com/r/selfhosted/), broad self-hosting community, VPN threads frequent
- [/r/WireGuard](https://www.reddit.com/r/WireGuard/), protocol-specific
- [/r/OpenVPN](https://www.reddit.com/r/OpenVPN/), protocol-specific
- [Hacker News : "Show HN: VPN"](https://hn.algolia.com/?q=self-host+vpn), long-running discussions
- [Lemmy.world /c/selfhost](https://lemmy.world/c/selfhost), Fediverse alternative

## Related awesome lists

- [awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted), broad self-hosting alternatives
- [awesome-vpn-leak-detection](https://github.com/ricco020/awesome-vpn-leak-detection), testing your VPN once deployed
- [awesome-privacy](https://github.com/Lissy93/awesome-privacy), broader privacy stack
- [awesome-hardening](https://github.com/decalage2/awesome-security-hardening), server hardening

## Contributing

Contributions are welcome. Please read the [contribution guidelines](CONTRIBUTING.md) first.

The criteria for inclusion:
- Tool, paper, or resource directly related to **self-hosting** a VPN (not commercial VPN reviews, those go in `awesome-privacy`)
- Open-source preferred (closed-source flagged explicitly)
- Active maintenance (last release < 2 years) OR seminal documentation
- No affiliate spam, VPS providers are listed objectively. Affiliate disclosures, when relevant, are made on the linked tutorial pages, not in this list

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the maintainers have waived all copyright and related or neighboring rights to this work.
