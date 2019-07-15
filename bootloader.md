# Kona Bootloader

The Kona bootloader is the first thing you see when attached to the serial console. It appears remarkably non-interactive.

```
--------------------------------
- MR1750
--------------------------------

loaded at:     00800000 008C01E8
zimage at:     00805880 008BCCD4
avail ram:     008C1000 08000000

Linux/PPC load: console=ttyS0,38400 ro --- i --- ?

Uncompressing Linux...done.
Now booting the kernel
Setting up ppc_ide_md
Memory BAT mapping: BAT2=128Mb, BAT3=0Mb, residual: 0Mb
Total memory = 128MB; using 256kB for hash table (at c01c0000)
map_page IOBASE returned 0
map_page INTS returned 0
Internal registers found at 0x30040000.
map_page BAR24 returned 0
map_page INTREGSBASE returned 0
Linux version 2.4.19-178 (root@mcpbuild3) (gcc version 3.2.3) #1 Tue Jun 26 20:56:22 UTC 2007
[..]
```

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
