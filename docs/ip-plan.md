# ip-plan.md

## 1. Scope and Purpose
This document records the network addressing schema and interface configuration for host `app01` based on extracted system diagnostics.

## 2. Host Addressing Table

| Parameter | Value | Status / Source |
| :--- | :--- | :--- |
| Hostname | app01 | System log verified |
| Interface | ens160 | Active (UP) |
| IPv4 Address / Subnet | 192.168.56.20/24 | Verified (`ip addr`) |
| Subnet Mask | 255.255.255.0 | Derived from CIDR /24 |
| Default Gateway | 192.168.56.10 | Verified (`ip route`) |
| Broadcast Address | 192.168.56.255 | Subnet calculation |

## 3. Network Topology Diagram

+-----------------------------------+
|               app01               |
|      ens160: 192.168.56.20/24     |
+-----------------+-----------------+
                  |
                  | Broadcast Domain: 192.168.56.0/24
                  v
+-----------------+-----------------+
|       Gateway: 192.168.56.10      |
+-----------------------------------+

## 4. Kernel Routing Table

default via 192.168.56.10 dev ens160 proto static metric 20100
192.168.56.0/24 dev ens160 proto kernel scope link src 192.168.56.20 metric 100

## 5. DNS Configuration (Local Resolver)

File `/etc/resolv.conf`:

nameserver 127.0.0.53
options edns0 trust-ad
search .

(Note: `127.0.0.53` indicates local stub resolver usage managed by `systemd-resolved`.)

## 6. Assumptions & Verification Status

- [x] Static IPv4 address and prefix configured on interface `ens160`.
- [x] Default route statically directed to gateway `192.168.56.10`.
- [ ] Verify ICMP reachability to default gateway (`192.168.56.10`).
- [ ] Verify outbound external reachability and upstream recursive DNS resolution.

## 7. Audit & Evidence Mapping

| Configuration Item | Reference Command / File | Evidence Status |
| :--- | :--- | :--- |
| Interface & IP | `ip -4 addr show ens160` | Verified (`192.168.56.20/24`) |
| Routing Table | `ip route show` | Verified (Gateway `192.168.56.10`) |
| Local Resolver | `cat /etc/resolv.conf` | Verified (`127.0.0.53`) |
| Reachability | Ping / Traceroute utility | Pending live verification |