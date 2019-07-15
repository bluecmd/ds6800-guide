# Kona Bootloader

The Kona bootloader is the first thing you see when attached to the serial console. It appears remarkably non-interactive.

You can extract a copy of it like this:

 * Use the r33f passphrase to get into initrd
 * Create /dev/mem using `mknod -m 660 /dev/mem c 1 1`
 * Load `modules/smc91111.o` and configure eth0 as usual
 * Mount /dev/hda5 somewhere
 * Run `cat /dev/mem | usr/bin/nc 10.1.1.1 8000` and receive it on other machine (`nc -l -p 8000 > mem.img`)
 * Carve out the bootloader using `dd if=mem.img skip=16384 bs=512 count=2048 of=bootloader`

Notably the kernel and initrd seems to not be loaded from the CompactFlash on boot. If you remove the CF card the kernel will still boot as well as the initrd.
This means that during updates the bootloader flash is re-written with the new kernel and initrd.

This is further confirmed by looking at the disassembly of the bootloader where it can be seen that the initrd size is coded into the bootloader, so likely never was meant to change dynamically but is rather compiled in. You can also find traces of NFS booting that appears to be inaccessible.

There is a boot mode called Special Boot but it appears to only print the "Special Boot" message and do nothing else.
It can be triggered by holding the 'i' key when the Kona boots.
