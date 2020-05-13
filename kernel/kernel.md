# Kernel related stuff

## useful links
  - About initrd location and loading https://unix.stackexchange.com/questions/89923/how-does-linux-load-the-initrd-image
    If it's 2.6+ kernel, it also has initramfs compiled within, so no need to load it. If it's an older kernel, bootloader also loads standalone initrd image into memory, so that kernel could mount it and get drivers for mounting real file system from disk.
  - Initramfs: https://wiki.ubuntu.com/Initramfs
    Initramfs is used as the first root filesystem that your machine has access to. It is used for mounting the real rootfs which has all your data.

