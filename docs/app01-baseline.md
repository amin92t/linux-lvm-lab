---

###  `app01-baseline.md`

```markdown
# app01 — Host Baseline

## Overview
Pre-configuration audit and baseline state of `app01` captured prior to mounting application storage volumes.

## System Information

| Attribute | Details |
| :--- | :--- |
| **Hostname** | app01 |
| **OS** | Fedora Linux 44 (Workstation Edition) |
| **Kernel** | 6.19.10-300.fc44.x86_64 |
| **Architecture** | x86_64 |
| **Hypervisor** | VMware, Inc. (VMware Virtual Platform) |

## Network Summary

| Interface | State | IPv4 | Gateway | DNS Stub |
| :--- | :--- | :--- | :--- | :--- |
| `ens160` | UP | 192.168.56.20/24 | 192.168.56.10 | 127.0.0.53 |

## Storage Layout

### OS Disks (`/dev/nvme0n2` - 20 GiB)

| Partition | Size | Filesystem | Mount Point | Notes |
| :--- | :--- | :--- | :--- | :--- |
| `/dev/nvme0n2p1` | 1 MiB | — | — | BIOS/Boot partition |
| `/dev/nvme0n2p2` | 2 GiB | ext4 | `/boot` | Dedicated boot |
| `/dev/nvme0n2p3` | 18 GiB | Btrfs | `/`, `/home` | Shared Btrfs subvolumes |

### Dedicated Application Storage (`/dev/nvme0n1` - 12 GiB)
Provisioned under Volume Group: **`vg_app`** (~3 GiB unallocated space remaining).

| Logical Volume | Size | Filesystem | Target Mount Point | Status |
| :--- | :--- | :--- | :--- | :--- |
| `app-data` | 4 GiB | XFS | `/srv/app` | Created, not mounted |
| `app-logs` | 5 GiB | XFS | `/var/log/app` | Created, not mounted |

## Mount Target Directory Baseline

| Target Path | Owner:Group | Permissions | Current State |
| :--- | :--- | :--- | :--- |
| `/srv/app` | `root:root` | `0755` (drwxr-xr-x) | Clean / Empty |
| `/var/log/app` | `root:root` | `0755` (drwxr-xr-x) | Clean / Empty |

## Next Steps / Pending Actions
- [ ] Mount `/dev/vg_app/app-data` to `/srv/app`.
- [ ] Mount `/dev/vg_app/app-logs` to `/var/log/app`.
- [ ] Update `/etc/fstab` with persistent UUID mounts.
- [ ] Adjust directory ownership and SELinux contexts based on application service requirements.