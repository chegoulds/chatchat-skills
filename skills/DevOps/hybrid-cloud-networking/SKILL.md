---
id: hybrid-cloud-networking
name: Hybrid Cloud Networking
description: Plan and troubleshoot hybrid cloud networking across on-prem and cloud environments with secure connectivity, routing clarity, and operational checks. Use when designing VPN/Direct Connect style topologies or resolving cross-environment traffic issues.
category: DevOps
requires: []
examples:
  - Design a hybrid network path for private app traffic between on-prem datacenter and cloud VPC.
  - Troubleshoot intermittent connectivity between Kubernetes services in cloud and legacy on-prem APIs.
---

# Hybrid Cloud Networking

Design and troubleshoot reliable connectivity between on-prem and cloud systems with explicit routing, DNS, and security controls.

## When to Use

- You need private, low-latency, or compliant connectivity across environments.
- You are planning CIDR, DNS, and route strategy for hybrid workloads.
- You are debugging packet loss, asymmetric routing, or policy blocks.

## Connectivity Patterns

**IPsec VPN vs WireGuard vs dedicated interconnect:** IPsec site-to-site VPNs are widely supported, integrate with cloud managed VPN offerings, and support BGP. Use for moderate throughput (typically under 1 Gbps per tunnel) and when cost matters. WireGuard offers simpler config, lower CPU overhead, and often better performance on commodity hardware; use when you control both endpoints and can run your own VPN stack. Dedicated interconnects (AWS Direct Connect, Azure ExpressRoute, GCP Partner Interconnect) provide predictable latency, higher bandwidth, and SLAs; use for production-critical or high-volume traffic.

**BGP route exchange:** Use BGP to advertise on-prem prefixes to cloud and cloud prefixes to on-prem. Configure route filters to avoid leaking internal routes. Set appropriate MED/local-pref and AS-path prepending for path control. Validate received routes before accepting into your routing table.

**Transit gateway hub-and-spoke:** Centralize connectivity through a transit gateway or virtual hub. Attach VPCs and on-prem VPNs as spokes. Use route tables to control which spokes can reach each other. Reduces tunnel count and simplifies policy but creates a single point of failureΓÇödesign for HA (multiple tunnels, redundant hubs).

## DNS Strategy

**Split-horizon:** Resolve internal names (e.g., `api.internal`) to private IPs from on-prem and cloud; resolve public names to public IPs. Use separate DNS servers or views per context.

**Private DNS zones:** Host private zones in cloud (Route 53 Private Hosted Zones, Azure Private DNS, Cloud DNS) and associate with VPCs. Use conditional forwarding from on-prem DNS to forward `*.internal` to cloud resolvers. Ensure resolvers are reachable over the hybrid path.

**Conditional forwarding:** Configure on-prem DNS to forward specific domains to cloud resolvers over the VPN/interconnect. Avoid forwarding everything; scope to required zones to reduce latency and failure blast radius.

## Common Pitfalls

- **CIDR overlap discovered late:** On-prem and cloud ranges (e.g., `10.0.0.0/16`) can overlap, breaking routing. Resolve before establishing tunnels. Use NAT, renumbering, or secondary ranges where supported.

- **Asymmetric routing through NAT gateways:** Return traffic may take a different path than ingress, causing drops. Ensure symmetric paths or use stateful NAT/firewall rules that allow return flows.

- **MTU issues on encrypted tunnels:** Encapsulation reduces effective MTU. Fragmentation causes drops and retransmits. Set TCP MSS clamping (e.g., 1400) or reduce interface MTU on tunnel endpoints. Test with large packets.

- **BGP route leakage:** Misconfigured filters can advertise internal routes to the internet or peer networks. Use strict import/export policies and prefix lists.

- **DNS cache poisoning across boundaries:** Stale or poisoned cache in one environment can affect the other. Use DNSSEC where possible; keep TTLs low during changes; validate resolver configuration.

## Troubleshooting Commands

- **traceroute / mtr:** Identify path and hop-by-hop latency. Use `mtr` for continuous monitoring. Compare from both directions to detect asymmetric paths.

- **tcpdump:** Capture on tunnel interfaces or host interfaces to verify packets. Filter by IP/port: `tcpdump -i eth0 host 10.0.0.1 and port 443`. Check for fragments and retransmits.

- **dig:** Verify DNS resolution path: `dig @resolver-ip api.internal` from both on-prem and cloud. Check `+trace` for delegation issues. Compare results across environments.

- **Route and BGP checks:** `ip route`, `show ip bgp`, `netstat -rn` to confirm expected routes. Validate that advertised prefixes match expectations.

## Workflow

1. Capture topology: networks, CIDRs, gateways, and trust boundaries.
2. Validate overlap risks (CIDR conflicts, NAT collisions, DNS split-horizon issues).
3. Define connectivity option (site-to-site VPN, dedicated interconnect, transit pattern).
4. Implement routing and segmentation:
   - Explicit route tables
   - Minimal required ports/protocols
   - East-west and north-south policy boundaries
5. Run end-to-end connectivity tests from both directions.
6. Add monitoring for tunnel health, latency, packet drops, and route changes.
7. Document ownership boundaries for network, DNS, and firewall changes.

## Output Format

```markdown
## Topology Summary
- On-prem: <CIDRs, gateways>
- Cloud: <VPCs, CIDRs, transit/hub>
- Connectivity: <IPsec | WireGuard | interconnect>, <tunnel count, BGP?>

## Connectivity Model and Rationale
- Chosen pattern: <hub-spoke | mesh | point-to-point>
- Trade-offs: <throughput, cost, complexity>

## DNS Strategy
- Split-horizon: <internal vs public resolution>
- Private zones: <cloud provider, conditional forwarding>
- Resolver path: <on-prem ΓåÆ cloud, cloud ΓåÆ on-prem>

## Route and Security Decisions
- BGP: <advertised/imported prefixes, filters>
- Firewall/NACL: <allowed ports, direction>
- Rationale: <per decision>

## Pitfalls Addressed
- <pitfall> -> <mitigation>

## Validation Results
- Connectivity: <success/failure, evidence>
- DNS: <resolution test results>
- Path symmetry: <traceroute/mtr findings>

## Troubleshooting Command Reference
- Path: `traceroute`, `mtr`
- Packets: `tcpdump -i <if> host <ip>`
- DNS: `dig @<resolver> <name>`

## Open Risks and Fallback
- <risk> -> <rollback path>
```

## Constraints

- Never accept overlapping CIDR ranges without a translation plan.
- Prefer least-privilege network policies over broad allow rules.
- Document assumptions about ownership at every trust boundary.