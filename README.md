# PCAP SIP Analyzer

**COEO AI Enablement** · single-file HTML tool

Upload a `.pcap` / `.pcapng` capture and get a SIP signaling dashboard: call-flow ladder, registration analysis, endpoint roles, NAT detection, warnings, and a raw message viewer. Same single-file, GitHub-Pages + Confluence-iframe pattern as the CLIR tool.

All parsing runs **in the browser** — capture files are never uploaded. Safe for CPNI / customer signaling.

## What it does

| Area | Detail |
|------|--------|
| **Containers** | Classic `pcap` (LE/BE, µs/ns) and `pcapng` (multi-interface, per-IDB timestamp resolution) |
| **Link types** | Ethernet (+ 802.1Q/QinQ VLAN), raw IPv4/IPv6, Linux SLL, NULL/loopback |
| **Transport** | SIP over UDP and TCP (compact header forms `v f t i m k l c` expanded) |
| **Call flow** | SVG ladder diagram — actors as lifelines, time down the gutter, click any message for full headers |
| **Registrations** | Classifies REGISTER / refresh / **unregister** (`expires=0`), auth realm, latency, returned bindings, MAC |
| **NAT** | Flags `received`/`rport` rewrites and private-Contact-behind-public-path |
| **De-dup** | Collapses SPAN/mirror duplicate frames; flags true retransmissions separately |
| **Endpoints** | Role inference: client (UA), proxy/relay, registrar, internal node |
| **Warnings** | 4xx/5xx, missing final responses, retransmissions, short re-register intervals, unregisters, NAT |
| **Export** | Copy a Confluence-markup summary |

## Hosting

**GitHub Pages** — commit `sip-pcap-analyzer.html`, enable Pages, done. No build step, no dependencies.

**Confluence embed** — use an Iframe / HTML macro pointing at the Pages URL:
```
https://<org>.github.io/<repo>/sip-pcap-analyzer.html
```

## Validated against

`Operations_Monitor_registration-20321_snt-mt1_voip_chi_snetcom_net.pcap` — 18 frames → 6 canonical SIP messages (12 mirror dupes collapsed), two registrations correctly resolved:
- **20321** via *S-NET Connect Mobile iOS* over **TCP**, `expires=0` → unregister, 200 OK
- **20321** via *Yealink W70B* over **UDP**, `Expires: 30`, 200 OK, NAT rewrite `192.168.1.79 → 99.101.51.28`, MAC + returned bindings

## Roadmap ideas (not yet built)

- RTP/RTCP stream analysis: packet loss, jitter, estimated MOS
- INVITE dialog / call-setup ladder (currently registration-focused but handles all methods)
- 401/407 challenge → authed-retry pairing on the ladder
- Multi-message TCP stream reassembly across segments

## Version history

See [CHANGELOG.md](./CHANGELOG.md) for the full, versioned list of changes. The current version is also shown in the tool's header and footer.
