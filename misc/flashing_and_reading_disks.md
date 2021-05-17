# Flashing and Reading disks
This is a collections of possible ways to flash images on disk and read raw data from them.

## The dd command
The dd command is the best tool to flash images on disks.
```bash
dd if=<source> of=<destination>
# Example
dd if=/root/ubuntu-16.img of=/dev/sda
```

Write starting from an offset
```bash
dd if=/root/ubuntu-16.img of=/dev/sda bs=1k seek=8
```

Read starting from an offset
```bash
dd if=/root/ubuntu-16.img bs=1k skip=8 of=/dev/sda
```

Progress:
```bash
# If dd version supports it
dd if=/root/ubuntu-16.img of=/dev/sda status=progress

# By using the pv command
dd if=/root/ubuntu-16.img | pv | of=/dev/sda
```

## The hexdump command
The `hedump` command is really useful to read the content of any files in hexadecimal and ASCII format.
Read the content of the first 512 bytes of the disk (MBR):
```bash
hexdump -n 512 -s 0x0000000 -C /dev/mmcblk0

# output
00000000  fa b8 00 10 8e d0 bc 00  b0 b8 00 00 8e d8 8e c0  |................|
00000010  fb be 00 7c bf 00 06 b9  00 02 f3 a4 ea 21 06 00  |...|.........!..|
00000020  00 be be 07 38 04 75 0b  83 c6 10 81 fe fe 07 75  |....8.u........u|
00000030  f3 eb 16 b4 02 b0 01 bb  00 7c b2 80 8a 74 01 8b  |.........|...t..|
00000040  4c 02 cd 13 ea 00 7c 00  00 eb fe 00 00 00 00 00  |L.....|.........|
00000050  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
000001b0  00 00 00 00 00 00 00 00  b1 7e 26 27 00 00 80 03  |.........~&'....|
000001c0  e0 ff 0c 03 e0 ff 00 00  08 00 00 00 04 00 00 03  |................|
000001d0  e0 ff 83 03 e0 ff 00 00  0c 00 00 00 30 00 00 03  |............0...|
000001e0  e0 ff 83 7b e1 b6 00 00  3c 00 9c fe ac 00 00 00  |...{....<.......|
000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa  |..............U.|
00000200
```
The leftmost column it's a byte counter in hexadecimal form. Each row is composed by 8bytes. Those 8 bytes are represented in hexadecimal from in the second (first 4 bytes) and third (second 4 bytes) column. The final column is the ASCII representation for each one of the 8 bytes of the row. If there's one or more row/s where every byte is 0 a `*` is placed.

## The fdisk command
The `fdisk` command is a really powerful tool that can be used to rewrite the MBR and partition the disk space. For the time being will focus on checking the MBR of a disk.
```bash
root@localhost:$ fdisk -l /dev/mmcblk1
Disk /dev/mmcblk1: 7.3 GiB, 7818182656 bytes, 15269888 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x27267eb1

Device         Boot   Start      End  Sectors  Size Id Type
/dev/mmcblk1p1 *     524288   786431   262144  128M  c W95 FAT32 (LBA)
/dev/mmcblk1p2       786432  3932159  3145728  1.5G 83 Linux
/dev/mmcblk1p3      3932160 15269531 11337372  5.4G 83 Linux
```
The smallest unit of the disk is the sector (here 512 bytes).
All the numbers reported for Start, End and Sectors are the number of sectors. This means that to get the partition size for partition 3, you have to perform: `11337372 * 512 = 5804734464 bytes`.
As you may notice, the first partition does not start at the beginning of the disk. This is done to left room for the MBR (first 512 bytes on the disk) and the second stage bootloader (Example: U-Boot generally starting at byte 8192). Generally 100MB are left before the first partition but this may vary (as here where ~250MB are left between the beginning of the disk and the beginning of the first partition).