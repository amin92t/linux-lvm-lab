# Storage Layout — app01

## Implementation

The lab uses `/dev/nvme0n1` as a whole-disk LVM physical volume.

No partition exists on this disk in the captured layout.
This differs from a partition-based PV design and reflects the actual implementation.

## Physical Volume and Volume Group

| Item | Value |
|---|---|
| PV device | /dev/nvme0n1 |
| PV format | lvm2 |
| VG name | vg_app |
| PV count | 1 |
| LV count | 2 |
| Snapshot count | 0 |
| VG size reported by vgs | <12.00g |
| Free space reported by vgs | <3.00g |

The output indicates slightly less than 3 GiB of unallocated VG space.
It must not be treated as an exact 3 GiB allocation budget.

## Logical Volumes

| LV | Device path | Size | Filesystem | Intended mount point |
|---|---|---:|---|---|
| app-data | /dev/vg_app/app-data | 4 GiB | XFS | /srv/app |
| app-logs | /dev/vg_app/app-logs | 5 GiB | XFS | /var/log/app |

The device-mapper names contain escaped hyphens:

```text
/dev/mapper/vg_app-app--data
/dev/mapper/vg_app-app--logs
```

## Filesystem UUIDs

| Filesystem | UUID |
|---|---|
| app-data | 4f1411c9-e60f-43e8-9363-f878735a2035 |
| app-logs | 35dbaa51-5ff9-4d16-96a8-2e47a11396c0 |

These are filesystem UUIDs, not the LVM PV UUID.

## Current Mount State

Neither logical volume has a mount point in the captured `lsblk` output.

Neither appears in the captured `df -hT` output.

The captured `/etc/fstab` contains only the operating system mounts:

- `/`
- `/boot`
- `/home`

Therefore, the application filesystems are not currently mounted and
their persistent mount entries are not yet configured.

## Planned Persistent Mount Entries

The following entries are proposed configuration, not the current contents
of `/etc/fstab`:

```fstab
UUID=4f1411c9-e60f-43e8-9363-f878735a2035 /srv/app xfs defaults 0 0
UUID=35dbaa51-5ff9-4d16-96a8-2e47a11396c0 /var/log/app xfs defaults 0 0
```

Recheck UUIDs before applying these entries if the filesystems are recreated.

## Remaining Work

1. Confirm that the destination directories remain safe to mount over.
2. Back up `/etc/fstab` before editing.
3. Add the application filesystem entries.
4. Reload systemd's generated mount configuration.
5. Validate the configuration and mount the filesystems.
6. Verify filesystem type, source, and target for both mounts.
7. Reboot and confirm persistence.
8. Record log expansion with before/after LV and filesystem sizes.
9. Perform the controlled fstab recovery exercise in the lab.

## Expansion Status

`app-logs` currently has a logical volume size of 5 GiB.

No earlier size or expansion command output is available in the supplied
evidence. Expansion is therefore not marked as completed.

Future expansion must validate both:

- The logical volume size
- The XFS filesystem size

XFS can grow while mounted, but shrinking is not supported.

## Evidence

See [storage-before-mount.txt](../evidence/app01/storage-before-mount.txt).