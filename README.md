 lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
sudo blkid
sudo pvs
sudo vgs
sudo lvs -o lv_name,vg_name,lv_size,lv_path
df -hT
cat /etc/fstab
ls -ld /srv/app /var/log/app
ls -la /srv/app /var/log/app
NAME                SIZE TYPE FSTYPE      MOUNTPOINTS
sr0                1024M rom
zram0               2.8G disk swap        [SWAP]
nvme0n1              12G disk LVM2_member
├─vg_app-app--data    4G lvm  xfs
└─vg_app-app--logs    5G lvm  xfs
nvme0n2              20G disk
├─nvme0n2p1           1M part
├─nvme0n2p2           2G part ext4        /boot
└─nvme0n2p3          18G part btrfs       /home
                                          /
[sudo] password for labadmin:
/dev/mapper/vg_app-app--logs: UUID="35dbaa51-5ff9-4d16-96a8-2e47a11396c0" BLOCK_SIZE="512" TYPE="xfs"
/dev/nvme0n1: UUID="EPRCst-Ipst-8ANJ-5lQ4-Tgab-9ev8-UQtpM6" TYPE="LVM2_member"
/dev/nvme0n2p3: LABEL="fedora" UUID="4543bf87-c49f-4454-a336-9ad905832c20" UUID_SUB="c37b6fca-59b9-40ff-8fde-f90891ea5433" BLOCK_SIZE="4096" TYPE="btrfs" PARTUUID="d9ecd39e-a9a7-4880-807e-caf07d73b1da"
/dev/nvme0n2p1: PARTUUID="ba2444f1-1b9d-4ef8-9723-8d2928a91880"
/dev/nvme0n2p2: UUID="fbca80d9-7f47-431b-96e5-c240a3e31df9" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="a24d1d39-9bfa-49e4-872b-1341511fe907"
/dev/mapper/vg_app-app--data: UUID="4f1411c9-e60f-43e8-9363-f878735a2035" BLOCK_SIZE="512" TYPE="xfs"
/dev/zram0: LABEL="zram0" UUID="a411e429-87a3-4bd1-b431-f2458a087fec" TYPE="swap"
  PV           VG     Fmt  Attr PSize   PFree
  /dev/nvme0n1 vg_app lvm2 a--  <12.00g <3.00g
  VG     #PV #LV #SN Attr    VSize   VFree
  vg_app   1   2   0 wz--n-- <12.00g <3.00g
  LV       VG     LSize Path
  app-data vg_app 4.00g /dev/vg_app/app-data
  app-logs vg_app 5.00g /dev/vg_app/app-logs
Filesystem     Type      Size  Used Avail Use% Mounted on
/dev/nvme0n2p3 btrfs      18G  4.2G   14G  24% /
devtmpfs       devtmpfs  1.4G     0  1.4G   0% /dev
tmpfs          tmpfs     1.5G  8.0K  1.5G   1% /dev/shm
tmpfs          tmpfs     583M  1.7M  582M   1% /run
none           tmpfs     1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
none           tmpfs     1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service
/dev/nvme0n2p3 btrfs      18G  4.2G   14G  24% /home
tmpfs          tmpfs     1.5G  8.0K  1.5G   1% /tmp
/dev/nvme0n2p2 ext4      2.0G  387M  1.5G  22% /boot
tmpfs          tmpfs     292M   84K  292M   1% /run/user/1000

#
# /etc/fstab
# Created by anaconda on Thu Sep 24 14:22:05 2026
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
UUID=4543bf87-c49f-4454-a336-9ad905832c20 / btrfs subvol=root,compress=zstd:1 0 0
UUID=fbca80d9-7f47-431b-96e5-c240a3e31df9 /boot ext4 defaults 1 2
UUID=4543bf87-c49f-4454-a336-9ad905832c20 /home btrfs subvol=home,compress=zstd:1 0 0
drwxr-xr-x. 1 root root 0 Sep 30 14:46 /srv/app
drwxr-xr-x. 1 root root 0 Sep 30 14:46 /var/log/app
/srv/app:
total 0
drwxr-xr-x. 1 root root 0 Sep 30 14:46 .
drwxr-xr-x. 1 root root 6 Sep 30 14:46 ..

/var/log/app:
total 0
drwxr-xr-x. 1 root root    0 Sep 30 14:46 .
drwxr-xr-x. 1 root root 1454 Oct  3 07:49 ..
