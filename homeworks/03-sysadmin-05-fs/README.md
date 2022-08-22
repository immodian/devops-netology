# Домашнее задание к занятию "3.5. Файловые системы"

## 1. Узнайте о sparse (разряженных) файлах.

Разряженные файлы, у котрых последовательность нулевых байтов заменена на их список, полезны для экономии дискового простанства. Требется поддержка файловой системы и прикладного ПО.

## 2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?

Файлы не могут иметь разные права, так как ссылаются на одну иноду, котора содержит информацию о правах и владельце.

## 4. Используя `fdisk`, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.

```bash
root@vagrant:~# fdisk /dev/sdb

Welcome to fdisk (util-linux 2.34).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x2bdc7050.

Command (m for help): p
Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x2bdc7050

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p):

Using default response p.
Partition number (1-4, default 1):
First sector (2048-5242879, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2G

Created a new partition 1 of type 'Linux' and of size 2 GiB.

Command (m for help): p
Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x2bdc7050

Device     Boot Start     End Sectors Size Id Type
/dev/sdb1        2048 4196351 4194304   2G 83 Linux

Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p):

Using default response p.
Partition number (2-4, default 2):
First sector (4196352-5242879, default 4196352):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242879, default 5242879):

Created a new partition 2 of type 'Linux' and of size 511 MiB.

Command (m for help): p
Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x2bdc7050

Device     Boot   Start     End Sectors  Size Id Type
/dev/sdb1          2048 4196351 4194304    2G 83 Linux
/dev/sdb2       4196352 5242879 1046528  511M 83 Linux

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

root@vagrant:~# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0                       7:0    0 43.6M  1 loop /snap/snapd/14978
loop1                       7:1    0 61.9M  1 loop /snap/core20/1328
loop2                       7:2    0 67.2M  1 loop /snap/lxd/21835
loop3                       7:3    0   47M  1 loop /snap/snapd/16292
loop4                       7:4    0   62M  1 loop /snap/core20/1611
loop5                       7:5    0 67.8M  1 loop /snap/lxd/22753
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0  1.5G  0 part /boot
└─sda3                      8:3    0 62.5G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm  /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
└─sdb2                      8:18   0  511M  0 part
sdc                         8:32   0  2.5G  0 disk
```

## 5. Используя `sfdisk`, перенесите данную таблицу разделов на второй диск.

```bash
root@vagrant:~# sfdisk -d /dev/sdb | sudo sfdisk /dev/sdc
Checking that no-one is using this disk right now ... OK

Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Created a new DOS disklabel with disk identifier 0x2bdc7050.
/dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
/dev/sdc2: Created a new partition 2 of type 'Linux' and of size 511 MiB.
/dev/sdc3: Done.

New situation:
Disklabel type: dos
Disk identifier: 0x2bdc7050

Device     Boot   Start     End Sectors  Size Id Type
/dev/sdc1          2048 4196351 4194304    2G 83 Linux
/dev/sdc2       4196352 5242879 1046528  511M 83 Linux

The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
root@vagrant:~# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0                       7:0    0 43.6M  1 loop /snap/snapd/14978
loop1                       7:1    0 61.9M  1 loop /snap/core20/1328
loop2                       7:2    0 67.2M  1 loop /snap/lxd/21835
loop3                       7:3    0   47M  1 loop /snap/snapd/16292
loop4                       7:4    0   62M  1 loop /snap/core20/1611
loop5                       7:5    0 67.8M  1 loop /snap/lxd/22753
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0  1.5G  0 part /boot
└─sda3                      8:3    0 62.5G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm  /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
└─sdb2                      8:18   0  511M  0 part
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
└─sdc2              
```

## 6. Соберите `mdadm` RAID1 на паре разделов 2 Гб.

```bash
root@vagrant:~# mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
mdadm: size set to 2094080K
Continue creating array?
Continue creating array? (y/n) y
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
root@vagrant:~#
root@vagrant:~#
root@vagrant:~#
root@vagrant:~# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 43.6M  1 loop  /snap/snapd/14978
loop1                       7:1    0 61.9M  1 loop  /snap/core20/1328
loop2                       7:2    0 67.2M  1 loop  /snap/lxd/21835
loop3                       7:3    0   47M  1 loop  /snap/snapd/16292
loop4                       7:4    0   62M  1 loop  /snap/core20/1611
loop5                       7:5    0 67.8M  1 loop  /snap/lxd/22753
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0  1.5G  0 part  /boot
└─sda3                      8:3    0 62.5G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdb2                      8:18   0  511M  0 part
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdc2                      8:34   0  511M  0 part

```

## 7. Соберите `mdadm` RAID0 на второй паре маленьких разделов.

```bash
root@vagrant:~# mdadm --create --verbose /dev/md1 --level=0 --raid-devices=2 /dev/sdb2 /dev/sdc2
mdadm: chunk size defaults to 512K
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md1 started.
root@vagrant:~# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 43.6M  1 loop  /snap/snapd/14978
loop1                       7:1    0 61.9M  1 loop  /snap/core20/1328
loop2                       7:2    0 67.2M  1 loop  /snap/lxd/21835
loop3                       7:3    0   47M  1 loop  /snap/snapd/16292
loop4                       7:4    0   62M  1 loop  /snap/core20/1611
loop5                       7:5    0 67.8M  1 loop  /snap/lxd/22753
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0  1.5G  0 part  /boot
└─sda3                      8:3    0 62.5G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdb2                      8:18   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdc2                      8:34   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0

```

## 8. Создайте 2 независимых PV на получившихся md-устройствах.

```bash
root@vagrant:/home/vagrant# pvcreate /dev/md0
  Physical volume "/dev/md0" successfully created.
root@vagrant:/home/vagrant# pvcreate /dev/md1
  Physical volume "/dev/md1" successfully created.
root@vagrant:/home/vagrant# pvdisplay
  --- Physical volume ---
  PV Name               /dev/sda3
  VG Name               ubuntu-vg
  PV Size               <62.50 GiB / not usable 0
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              15999
  Free PE               8000
  Allocated PE          7999
  PV UUID               x7S6t2-at3n-E9kU-cz28-gAH3-QU9H-vyVuNf

  "/dev/md0" is a new physical volume of "<2.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/md0
  VG Name
  PV Size               <2.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               FChec5-WESO-2fHH-FJfc-txYS-Z4NG-1kofRX

  "/dev/md1" is a new physical volume of "1018.00 MiB"
  --- NEW Physical volume ---
  PV Name               /dev/md1
  VG Name
  PV Size               1018.00 MiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               9rciDZ-GmmH-943s-d8WP-odbe-FGf2-cz9g5T

```

## 9. Создайте общую volume-group на этих двух PV.

```bash
root@vagrant:/home/vagrant# vgcreate vg1 /dev/md0 /dev/md1
  Volume group "vg1" successfully created
root@vagrant:/home/vagrant# pvs
  PV         VG        Fmt  Attr PSize    PFree
  /dev/md0   vg1       lvm2 a--    <2.00g   <2.00g
  /dev/md1   vg1       lvm2 a--  1016.00m 1016.00m
  /dev/sda3  ubuntu-vg lvm2 a--   <62.50g   31.25g
```

## 10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

```bash
root@vagrant:/home/vagrant# lvcreate -L 100M vg1 /dev/md1
  Logical volume "lvol0" created.
root@vagrant:/home/vagrant# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 43.6M  1 loop  /snap/snapd/14978
loop1                       7:1    0 61.9M  1 loop  /snap/core20/1328
loop2                       7:2    0 67.2M  1 loop  /snap/lxd/21835
loop3                       7:3    0   47M  1 loop  /snap/snapd/16292
loop4                       7:4    0   62M  1 loop  /snap/core20/1611
loop5                       7:5    0 67.8M  1 loop  /snap/lxd/22753
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0  1.5G  0 part  /boot
└─sda3                      8:3    0 62.5G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdb2                      8:18   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
    └─vg1-lvol0           253:1    0  100M  0 lvm
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdc2                      8:34   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
    └─vg1-lvol0           253:1    0  100M  0 lvm
```

## 11. Создайте `mkfs.ext4` ФС на получившемся LV.

```bash
root@vagrant:/home/vagrant# mkfs.ext4 /dev/vg1/lvol0
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 25600 4k blocks and 25600 inodes

Allocating group tables: done
Writing inode tables: done
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done

root@vagrant:/home/vagrant# lsblk -f
NAME    FSTYPE        LABEL     UUID                                   FSAVAIL FSUSE% MOUNTPOINT
loop0   squashfs                                                             0   100% /snap/snapd/1
loop1   squashfs                                                             0   100% /snap/core20/
loop2   squashfs                                                             0   100% /snap/lxd/218
loop3   squashfs                                                             0   100% /snap/snapd/1
loop4   squashfs                                                             0   100% /snap/core20/
loop5   squashfs                                                             0   100% /snap/lxd/227
sda
├─sda1
├─sda2  ext4                    1347b25b-64dd-4d97-80ce-90cd82397358      1.3G     7% /boot
└─sda3  LVM2_member             x7S6t2-at3n-E9kU-cz28-gAH3-QU9H-vyVuNf
  └─ubuntu--vg-ubuntu--lv
        ext4                    d940a45b-2440-4ece-9c0c-45ced4c52e39     25.4G    12% /
sdb
├─sdb1  linux_raid_me vagrant:0 7eeef5c2-79ba-b7a8-e55d-cbcf4d78cffd
│ └─md0 LVM2_member             FChec5-WESO-2fHH-FJfc-txYS-Z4NG-1kofRX
└─sdb2  linux_raid_me vagrant:1 b19a3051-3a99-65ff-1ec9-6b17876128c5
  └─md1 LVM2_member             9rciDZ-GmmH-943s-d8WP-odbe-FGf2-cz9g5T
    └─vg1-lvol0
        ext4                    4bcd324c-b821-4537-aec6-0496fd425d88
sdc
├─sdc1  linux_raid_me vagrant:0 7eeef5c2-79ba-b7a8-e55d-cbcf4d78cffd
│ └─md0 LVM2_member             FChec5-WESO-2fHH-FJfc-txYS-Z4NG-1kofRX
└─sdc2  linux_raid_me vagrant:1 b19a3051-3a99-65ff-1ec9-6b17876128c5
  └─md1 LVM2_member             9rciDZ-GmmH-943s-d8WP-odbe-FGf2-cz9g5T
    └─vg1-lvol0
        ext4                    4bcd324c-b821-4537-aec6-0496fd425d88
```

## 12. Смонтируйте этот раздел в любую директорию, например, `/tmp/new`.

```bash
root@vagrant:/home/vagrant# mkdir /tmp/new
root@vagrant:/home/vagrant# mount /dev/vg1/lvol0 /tmp/new

```

## 13. Поместите туда тестовый файл, например `wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz`.

```bash
root@vagrant:/home/vagrant# wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz
--2022-08-22 07:58:27--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 22271967 (21M) [application/octet-stream]
Saving to: ‘/tmp/new/test.gz’

/tmp/new/test.gz         100%[=================================>]  21.24M  1.65MB/s    in 12s

2022-08-22 07:58:39 (1.73 MB/s) - ‘/tmp/new/test.gz’ saved [22271967/22271967]

root@vagrant:/home/vagrant# ls /tmp/new/
lost+found  test.gz
```

## 14. Прикрепите вывод `lsblk`.

```bash
root@vagrant:/home/vagrant# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 43.6M  1 loop  /snap/snapd/14978
loop1                       7:1    0 61.9M  1 loop  /snap/core20/1328
loop2                       7:2    0 67.2M  1 loop  /snap/lxd/21835
loop3                       7:3    0   47M  1 loop  /snap/snapd/16292
loop4                       7:4    0   62M  1 loop  /snap/core20/1611
loop5                       7:5    0 67.8M  1 loop  /snap/lxd/22753
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0  1.5G  0 part  /boot
└─sda3                      8:3    0 62.5G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdb2                      8:18   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
    └─vg1-lvol0           253:1    0  100M  0 lvm   /tmp/new
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
└─sdc2                      8:34   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
    └─vg1-lvol0           253:1    0  100M  0 lvm   /tmp/new
```

## 15. Протестируйте целостность файла:

```bash
root@vagrant:/home/vagrant# gzip -t /tmp/new/test.gz
root@vagrant:/home/vagrant# echo $?
0
```

## 16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.

```bash
root@vagrant:/home/vagrant# pvmove /dev/md1 /dev/md0
  /dev/md1: Moved: 32.00%
  /dev/md1: Moved: 100.00%
root@vagrant:/home/vagrant# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                       7:0    0 43.6M  1 loop  /snap/snapd/14978
loop1                       7:1    0 61.9M  1 loop  /snap/core20/1328
loop2                       7:2    0 67.2M  1 loop  /snap/lxd/21835
loop3                       7:3    0   47M  1 loop  /snap/snapd/16292
loop4                       7:4    0   62M  1 loop  /snap/core20/1611
loop5                       7:5    0 67.8M  1 loop  /snap/lxd/22753
sda                         8:0    0   64G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0  1.5G  0 part  /boot
└─sda3                      8:3    0 62.5G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0 31.3G  0 lvm   /
sdb                         8:16   0  2.5G  0 disk
├─sdb1                      8:17   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
│   └─vg1-lvol0           253:1    0  100M  0 lvm   /tmp/new
└─sdb2                      8:18   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
sdc                         8:32   0  2.5G  0 disk
├─sdc1                      8:33   0    2G  0 part
│ └─md0                     9:0    0    2G  0 raid1
│   └─vg1-lvol0           253:1    0  100M  0 lvm   /tmp/new
└─sdc2                      8:34   0  511M  0 part
  └─md1                     9:1    0 1018M  0 raid0
```

## 17. Сделайте `--fail` на устройство в вашем RAID1 md.

```bash
root@vagrant:/home/vagrant# mdadm /dev/md0 --fail /dev/sdb1
mdadm: set /dev/sdb1 faulty in /dev/md0
```

## 18. Подтвердите выводом `dmesg`, что RAID1 работает в деградированном состоянии.

```bash
root@vagrant:/home/vagrant# dmesg | grep raid
[    2.705101] raid6: avx2x4   gen() 34860 MB/s
[    2.753065] raid6: avx2x4   xor() 22653 MB/s
[    2.801078] raid6: avx2x2   gen() 28954 MB/s
[    2.849085] raid6: avx2x2   xor() 21437 MB/s
[    2.897104] raid6: avx2x1   gen() 25687 MB/s
[    2.945085] raid6: avx2x1   xor() 18187 MB/s
[    2.993104] raid6: sse2x4   gen() 14861 MB/s
[    3.041090] raid6: sse2x4   xor() 10168 MB/s
[    3.089103] raid6: sse2x2   gen() 12286 MB/s
[    3.137089] raid6: sse2x2   xor()  9342 MB/s
[    3.185103] raid6: sse2x1   gen() 10909 MB/s
[    3.233128] raid6: sse2x1   xor()  7288 MB/s
[    3.233473] raid6: using algorithm avx2x4 gen() 34860 MB/s
[    3.233815] raid6: .... xor() 22653 MB/s, rmw enabled
[    3.234143] raid6: using avx2x2 recovery algorithm
[17283.330081] md/raid1:md0: not clean -- starting background reconstruction
[17283.330082] md/raid1:md0: active with 2 out of 2 mirrors
[31378.234990] md/raid1:md0: Disk failure on sdb1, disabling device.
               md/raid1:md0: Operation continuing on 1 devices.
```

## 19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

```bash
root@vagrant:/home/vagrant# gzip -t /tmp/new/test.gz
root@vagrant:/home/vagrant# echo $?
0
```