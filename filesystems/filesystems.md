#  Filesystems


## Inodes
Inodes are a key-concept in unix/linux filesystems. They represent the metadata related to a file or a directory, such as file permissions, ownership, file content location, attributes etc... Since in Linux directories are files, directories have inodes too.
Some inodes facts:
- Inode is unique within a filesystem. The combination of the filesystem ID and inode ID makes a file uniquely identifiable within a running system.
- all inodes are held in one table
- inode number is a 32-bit unsigned long integer (max 2^32 inodes, approx. over 4 billions)
- each inodes has pointers to the blocks that store the contents of the file they represent
- inode is fixed size

To see the inode number of a file:
```bash
ls -i .bash_history

63102 .bash_history
```

### Commands to check inodes
Check the inode stats on a filesystem (here on a partition):
```bash
df -i /dev/sda1
```

See the inode content:
```bash
# since inodes are unique within a filesystem, filesystem must be passed
debugfs -R "stat <63102>" /dev/sda1

Inode: 63102   Type: regular    Mode:  0600   Flags: 0x80000
Generation: 2588075403    Version: 0x00000000:00000001
User:  1000   Group:  1000   Project:     0   Size: 167
File ACL: 0
Links: 1   Blockcount: 8
Fragment:  Address: 0    Number: 0    Size: 0
 ctime: 0x60aca338:c81d0280 -- Tue May 25 07:11:52 2021
 atime: 0x60aca338:c81d0280 -- Tue May 25 07:11:52 2021
 mtime: 0x60a7a6b8:63913d4c -- Fri May 21 12:25:28 2021
crtime: 0x60a7a49a:64865da8 -- Fri May 21 12:16:26 2021
Size of extra inode fields: 32
Inode checksum: 0xe17c25a8
EXTENTS:
(0):40670

# Inode: The number of the inode we’re looking at.
# Type: This is a regular file, not a directory or symbolic link.
# Mode: The file permissions in octal.
# Flags: Indicators that represent different features or functionality. The 0x80000 is the “extents” flag (more on this below).
# Generation: A Network File System (NFS) uses this when someone accesses remote file systems over a network connection as though they were mounted on the local # machine. The inode and generation numbers are used as a form of file handle.
# Version: The inode version.
# User: The owner of the file.
# Group: The group owner of the file.
# Project: Should always be zero.
# Size: The size of the file.
# File ACL: The file access control list. These were designed to allow you to give controlled access to people who aren’t in the owner group.
# Links: The number of hard links to the file.
# Blockcount: The amount of hard drive space allocated to this file, given in 512-byte chunks. Our file has been allocated eight of these, which is 4,096 bytes. # So, our 98-byte file sits within a single 4,096-byte disk block.
# Fragment: This file is not fragmented. (This is an obsolete flag.)
# Ctime: is the last change time, which updates when the file’s properties (i.e. permissions, name, location) change.
# Atime: The time at which this file was last accessed.
# Mtime: The time at which this file was last modified.
# Crtime: The time at which the file was created (doesn't update)
# Size of extra inode fields: The ext4 file system introduced the ability to allocate a larger on-disk inode at format time. This value is the number of extra # bytes the inode is using. This extra space can also be used to accommodate future requirements for new kernels or to store extended attributes.
# Inode checksum: A checksum for this inode, which makes it possible to detect if the inode is corrupted.
# Extents: If extents are being used (on ext4, they are, by default), the metadata regarding the disk block usage of files has two numbers that indicate the start # and end blocks of each portion of a fragmented file. This is more efficient than storing every disk block taken up by each portion of a file. We have one extent # because our small file sits in one disk block at this block offset.
```

## Directories and directory entries
In an ext4 filesystem, a directory is more or less a flat file that maps an arbitrary byte string (usually ASCII) to an inode number on the filesystem.
A single mapping element is called directory entry or dentry.
A directory is just a file in Linux, so its metadata are represented by an inode. Rather than pointing to disk blocks that contain file data, though, a directory inode points to disk blocks that contain directory structures. Is the VFS that, when handling a directory, build an in-memory representation of it and build the directory graph of the filesystem.




## Sectors and blocks
### sector
It's the physical spot on a disk (generally is 512 bytes).
To see the sector size of a disk:
```bash
fdisk -l /dev/sda

Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x309a5396

Device     Boot Start      End  Sectors Size Id Type
/dev/sda1  *     2048 83886046 83883999  40G 83 Linux
```

### block
A block can be made of a single sector or several sectors (2,4,8 or even 16). Is the smallest unit of data the OS can read/write from disk.
To see the block size of a disk:
```bash
blockdev --getbsz /dev/sda

4096
```
Disk usage expressed in block size of 4096 bytes:
```bash
df -B 4096 /dev/sda1

Filesystem     4K-blocks    Used Available Use% Mounted on
/dev/sda1       10148403 1772352   8371955  18% /
```

## Hard and Symbolic links
Hard and Symbolic links are both directory entries.

A hard link:
- is a directory entry that points to an inode (just like a regular directory entry for a file). This means it has the same inode number as the original file.
- since they refer directly to an inode, they cannot be cross-filesystem
- they count as references to a file, so a file with hard links is deleted only if the link count goes to 0
- deleting an hard link could lead to file deletion (if the hard link is the only thing left that points to a file)
- they cannot point to a directory (except `.` and `..` that are hard links)

A symbolic link (or symlink):
- is a directory entry (in the directory where the link resides) that points to an inode that provides the name of another directory entry (where the real file resides). This means it has a different inode number than the original file.
- they can be cross-filesystem
- they can point to a file or a directory
- deleting the symlink does not delete the directory or the file
- deleting a directory or a file does not delete symlinks pointing to them (the symlink remain dangling)


## Links
https://www.howtogeek.com/465350/everything-you-ever-wanted-to-know-about-inodes-on-linux/
https://developer.ibm.com/tutorials/l-linux-filesystem/
https://developer.ibm.com/technologies/linux/tutorials/l-lpic1-104-6/#:~:text=A%20hard%20link%20is%20a,the%20length%20of%20the%20name.