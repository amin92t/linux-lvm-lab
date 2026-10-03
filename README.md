# Linux LVM Storage Lab

A hands-on Linux storage administration lab on a Fedora virtual machine.

The project focuses on LVM, XFS, persistent mounts, troubleshooting,
and documenting technical work with Git.

## Scenario

An application server needs dedicated storage for application data and logs.
The lab objective is to provision that storage and later expand the log
filesystem without reinstalling the operating system.

Planned mount points:

- `/srv/app`: application data
- `/var/log/app`: application logs

## Scope

This repository documents `app01` only.

The default gateway address is recorded as part of the network configuration.
Configuration and validation of other servers are outside this repository's scope.

## Environment

| Item | Value |
|---|---|
| Hostname | app01 |
| Operating system | Fedora Linux 44 Workstation Edition |
| Virtualization | VMware |
| Network interface | ens160 |
| IPv4 address | 192.168.56.20/24 |
| Default gateway | 192.168.56.10 |
| OS disk | /dev/nvme0n2 — 20 GiB |
| Lab disk | /dev/nvme0n1 — 12 GiB |
| Volume group | vg_app |
| Lab filesystems | XFS |

Device names describe the captured environment and must not be assumed
to be identical on another machine.

## Current Status

The LVM objects and XFS filesystems exist.

Neither application filesystem is currently mounted, and `/etc/fstab`
does not yet contain their entries.

Both destination directories exist and were empty when inspected.

### Progress

- [x] A separate 12 GiB virtual disk is present.
- [x] A physical volume exists on the whole lab disk.
- [x] Volume group `vg_app` exists.
- [x] Logical volume `app-data` exists with a size of 4 GiB.
- [x] Logical volume `app-logs` exists with a current size of 5 GiB.
- [x] Both logical volumes contain XFS filesystems.
- [x] Destination directories have been inspected and are empty.
- [ ] Mount the application filesystems.
- [ ] Configure persistent mounts using filesystem UUIDs.
- [ ] Validate mounts after reboot.
- [ ] Document log LV and filesystem expansion with before/after evidence.
- [ ] Perform and document a controlled fstab failure and recovery exercise.

The current 5 GiB size of `app-logs` does not, by itself, prove that an
expansion exercise has been completed.

## Storage Design

```text
/dev/nvme0n1 — 12 GiB
└── LVM physical volume
    └── vg_app — slightly less than 12 GiB usable
        ├── app-data — 4 GiB — XFS
        │   └── Intended mount point: /srv/app
        ├── app-logs — 5 GiB — XFS
        │   └── Intended mount point: /var/log/app
        └── Unallocated VG space: slightly less than 3 GiB
```

This implementation uses a whole-disk PV. No partition was created on
the lab disk.

## Documentation

- [Host baseline](docs/app01-baseline.md)
- [IP plan](docs/ip-plan.md)
- [Storage layout](docs/storage-layout.md)

## Evidence

- [Network evidence](evidence/app01/network.txt)
- [Storage evidence before mounting](evidence/app01/storage-before-mount.txt)

Evidence files contain selected excerpts from captured command output.
They are not complete terminal transcripts.

## Safety Notes

This is a lab, not a production deployment.

Before applying a similar storage change to a real application server:

- Identify the actual application service and file ownership requirements.
- Back up existing data.
- Stop relevant writers before transferring data or replacing mount points.
- Remember that mounting over a populated directory hides its existing contents.
- Verify target devices before any destructive operation.
- XFS supports growth, but shrinking is not supported.

The captured output does not identify an application service or establish
the required production ownership and SELinux policy.

## Evidence Limitations

- Successful external DNS resolution has not been demonstrated.
- Upstream DNS server addresses have not been verified.
- Mount persistence after reboot has not been tested in the supplied evidence.
- LV expansion and recovery exercises have not been demonstrated.
