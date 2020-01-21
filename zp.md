# ZP Processor

The ZebraPack (ZP) processors are using the Renesas H8S/2166 (for SES) and Renesas H8/3687 (for RDP). Why they are named ZebraPack is not known.

## RDP

There is a ZP processor on the rear display panel (RDP). Not much is known about it but getting access to it should
be possible using TTL or RS232.

## SES
The ZP SES processor seems to be the responsible party to read product data from the RDP.
This is a processor that is present on both the main controller and the disk expansion controller board.

You can connect to it using the blocked serial port using 57600 baud 8n1.

The ZP processor boots when the chassis receives power, before you press the power-on on the RDP. This
is what you will see on the console:

```
Bootstrap v02
00000200: B0 0D C0 DE 00 00 00 02 00 04 09 04 00 0E 14 37
00000210: 00 03 00 00 00 01 00 00 00 04 00 00 40 63 67 C7
00000220: F0 62 BA 28
Primary
00010000: FE ED FA CE 00 00 00 00 00 08 0C 11 00 05 26 02
00010010: BE 40 39 BE 40 A0 E9 63
Secondary
00040000: FE ED FA CE 00 00 00 00 00 08 0C 11 00 05 26 02
00040010: BE 40 39 BE 40 A0 E9 63 
No update necessary.
Booting firmware ...

ZP SES R10-00 Dec 17 2008 05:45:56
(c) Copyright IBM Corp. 2001, 2005 All rights reserved.

Location = 0x02 
Ping partner RC = 0x0043
SPOST:
POST#:0x0005   Passed!
POST#:0x0006   Passed!
POST#:0x0007   Passed!
POST#:0x0008   Passed!
POST#:0x0015   Passed!
POST done!
```

If you then choose to power on the chassis, you will see this:

```
>  
Powering-on supply 1 
Exiting stand-by mode ... 

Starting WWNN Configuration........ 
Partner not present 
RDP WWNN = 0x50050763 0x0CFE1119
Partner WWNN = 0x00000000 0x00000000
Use RDP WWNN!
WWNN = 0x50050763 0x0CFE1119
WWPN = 0x50050763 0x0CF21119
WWNN Configuration Done.... done

OPOST:
POST#:0x0005   Passed!
POST#:0x0006   Passed!
POST#:0x0007   Passed!
POST#:0x0008   Passed!
POST#:0x0015   Passed!
POST#:0x0014   Passed!
POST#:0x0000   Passed!
POST#:0x0020   Passed!
POST#:0x0022   Passed!
POST#:0x0030   Passed!
POST#:0x0031   Passed!
POST#:0x0032   Passed!
POST#:0x0033   Passed!
POST#:0x0034   Passed!
POST#:0x0046   Passed!
POST#:0x0047   Passed!

POST done!
```

It seems to have a console indicated by the `> ` symbol if you press enter. The firmware talks about logging in, but
there has been no known successful ways of logging in. Typing "help" lists no commands, but there are commands that are available.

## psinfo

```
> psinfo
Power Supply 0:
   Absent
Power Supply 1:
   State:                           0x04
   PS Fault:                        FALSE
   Current Share Fault:             FALSE
   12v Over-Voltage:                FALSE
   5v Over-Voltage:                 FALSE
   12v Over-Current/Under-Voltage:  FALSE
   5v Over-Current/Under-Voltage:   FALSE
   Standby Fault:                   FALSE
   Over-Temperature                 FALSE
   Power Good:                      FALSE
   AC Present                       TRUE
   Power Control                    0x07
```

## sysinfo

Card needs to be powered up before the model is known and temperature is reported.

```
> sysinfo
Firmware Version:   R10-00 Dec 17 2008 05:45:08
Hardware Version:   0x09 (Kona Pass 2.5)
Bootstrap Version:  2
Firmware State:     0x04 (Idle-Normal)
Boot Cause:         0x05 (PowerOn)
COMMS State:        0x04 (Partner Hang)
Processor Time:     0x000831DB
System Date/Time:   2255/55/12 00:08:57.051
Pack ID:            0x00
Card Location:      2 (Bottom)
Temperature:        28 82 (C,F)
Voltages (mV):      12270 5120 3294 2518 1225
RDP FW Version:     0x05 0x01
Partner:            Absent
```

# Library

The main controller firmware has references to things like ZP_ReadRDPVpd which seems to be referring to this processor. Probably
this means that all the read/writes to the RDP is handled by the ZP. 

These are the currently known utilities that seems to be communicating with the ZP:

 * dadloadenclosure
 * iss_diskvpd
 * issses_activateevents
 * issses_bypass
 * issses_checkpost
 * issses_checkstatus
 * issses_clearevents
 * issses_connectors
 * issses_controlidlight
 * issses_diskinfobyenc
 * issses_disklocation
 * issses_encvpd
 * issses_getdevicedata
 * issses_getdiskcount
 * issses_getdiskinfo
 * issses_getencid
 * issses_getencserial
 * issses_getenctemp
 * issses_getfanspeed
 * issses_healthcheck
 * issses_pcm
 * issses_porterrorcount
 * issses_poweroff
 * issses_queryidlight
 * issses_readdatetime
 * issses_readevents
 * issses_resetic
 * issses_resetswapbit
 * issses_setdatetime
 * issses_setencid
 * issses_setfanspeed
 * issses_showencconn
 * issses_slotinfo
 * issses_statesave
 * issses_test
 * issses_updatevpd
