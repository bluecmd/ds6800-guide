# ZP Processor

The ZebraPack (ZP) processors are using the Renesas H8S/2166 (for SES) and Renesas H8/3687 (for RDP). Why they are named ZebraPack is not known.

## RDP

The Rear display panel (RDP) contains a ZP processor and an EEPROM. It controls
the power-supplies and general chassis functions like LEDs.
The panel contains an ST M24C08 EEPROM (8 kbit, 2.5 V to 5.5 V, –40 °C to 85 °C).
This EEPROM seems to contain the vital product data (VPD) used by the ZP processors.

Example EEPROM content:

```
00000000  01 f9 50 4e 3d 20 32 33  52 32 32 31 34 46 4e 3d  |..PN= 23R2214FN=|
00000010  20 32 32 52 36 33 30 35  53 4e 3d 59 4d 31 30 4d  | 22R6305SN=YM10M|
00000020  59 39 43 xx xx xx xx 45  43 3d 20 20 20 48 38 31  |Y9CxxxxEC=   H81|
00000030  xx xx xx 46 4d 46 52 3d  20 20 20 20 4d 59 43 4c  |xxxFMFR=    MYCL|
00000040  3d 20 20 52 30 35 2d 30  31 6e 61 6d 65 3d 52 72  |=  R05-01name=Rr|
00000050  20 44 69 73 70 6c 61 79  44 46 4c 3d 30 30 30 30  | DisplayDFL=0000|
00000060  30 50 43 3d 44 30 30 30  30 30 30 30 00 00 00 00  |0PC=D0000000....|
00000070  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000080  00 00 42 52 3d 30 30 53  4e 3d 30 30 30 30 30 30  |..BR=00SN=000000|
00000090  30 53 47 3d 36 38 39 39  xx xx xx 54 4d 3d 30 30  |0SG=6899xxxTM=00|
000000a0  30 30 30 30 30 30 54 4e  3d 31 37 35 30 2d 35 31  |000000TN=1750-51|
000000b0  31 49 44 3d 30 30 4d 4e  3d 30 30 30 30 30 30 30  |1ID=00MN=0000000|
000000c0  4e 4e 3d 35 30 30 35 30  37 36 xx xx xx xx xx xx  |NN=5005076xxxxxx|
000000d0  xx xx xx 00 00 00 00 00  00 00 00 00 00 00 00 00  |xxx.............|
000000e0  00 00 50 54 53 3d 30 30  30 30 30 30 30 30 30 30  |..PTS=0000000000|
000000f0  30 30 43 43 30 50 43 3d  30 30 30 30 30 30 30 30  |00CC0PC=00000000|
00000100  43 43 30 53 4e 3d 30 30  30 30 30 30 30 30 30 30  |CC0SN=0000000000|
00000110  30 30 43 43 31 50 43 3d  30 30 30 30 30 30 30 30  |00CC1PC=00000000|
00000120  43 43 31 53 4e 3d 30 30  30 30 30 30 30 30 30 30  |CC1SN=0000000000|
00000130  30 30 41 46 4c 3d 30 30  30 30 30 30 30 30 30 30  |00AFL=0000000000|
00000140  30 30 45 4b 3d 00 00 00  00 00 00 00 00 00 00 00  |00EK=...........|
00000150  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000180  00 00 00 00 00 00 00 00  00 00 50 43 4d 3d 30 50  |..........PCM=0P|
00000190  52 53 3d 30 00 00 00 00  00 00 00 00 00 00 00 00  |RS=0............|
000001a0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
000001f0  0f fe xx xx 00 00 00 19  a3 df 00 cd ff ff ff ff  |................|
00000200  ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff  |................|
*
00000800
```

The SCL and SDA lines are exposed on the golden fingers board on 14 (SCL) and 29 (SDA).

Removing RDP when powered seems to be fine in terms of PSU still providing power to the system.

## SES

The ZP SES processor seems to be the responsible party to read product data from the RDP.
This is a processor that is present on both the main controller and the disk
expansion controller board. SES likely refers to
[SCSI Enclosure Services](https://en.wikipedia.org/wiki/SCSI_Enclosure_Services).
It interfaces with the SOC422 FC switch, seemingly providing enclosure control over FC.

The SES also has a VPD EEPROM, unknown location and chip as of right now.

Example EEPROM content:

```
00000000  00 fd 50 4e 3d 20 32 33  52 31 32 37 34 46 4e 3d  |..PN= 23R1274FN=|
00000010  20 32 33 52 31 32 37 34  53 4e 3d 59 4d 31 30 4d  | 23R1274SN=YM10M|
00000020  59 37 34 33 30 30 36 45  43 3d 20 20 20 48 38 33  |Y7xxxxxEC=   H83|
00000030  39 36 35 42 4d 46 52 3d  20 20 20 20 4d 59 43 4c  |9xxBMFR=    MYCL|
00000040  3d 30 30 30 30 30 30 30  30 6e 61 6d 65 3d 20 20  |=00000000name=  |
00000050  20 20 20 20 4b 6f 6e 61  44 46 4c 3d 30 30 30 30  |    KonaDFL=0000|
00000060  30 50 43 3d 52 30 34 46  32 34 46 32 6d 41 3d 30  |0PC=R04xxxF2mA=0|
00000070  30 34 36 00 00 00 00 00  00 00 00 00 00 00 00 00  |046.............|
00000080  00 00 50 54 53 3d 30 30  30 30 30 30 30 30 30 30  |..PTS=0000000000|
00000090  30 30 41 46 4c 3d 30 30  30 30 30 30 30 30 30 30  |00AFL=0000000000|
000000a0  30 30 4e 4e 3d 30 30 30  30 30 30 30 30 30 30 30  |00NN=00000000000|
000000b0  30 30 30 30 30 43 43 23  50 43 3d 30 30 30 30 30  |00000CC#PC=00000|
000000c0  30 30 30 43 43 23 53 4e  3d 30 30 30 30 30 30 30  |000CC#SN=0000000|
000000d0  30 30 30 30 30 4f 50 50  50 43 3d 30 30 30 30 30  |00000OPPPC=00000|
000000e0  30 30 30 4f 50 50 53 4e  3d 30 30 30 30 30 30 30  |000OPPSN=0000000|
000000f0  30 30 30 30 30 00 00 00  00 00 00 00 00 00 00 82  |00000...........|
```

You can connect to the SES using the blocked serial port using 57600 baud 8n1.

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

It seems to have a console indicated by the `> ` symbol if you press enter.

## Logging in

By default you have access to some minor commands in the SES prompt like
`psinfo` and `sysinfo`, but to access the really useful commands of the SES
you need to log in. Do this by pressing Ctrl+L in a serial console.
Use any of the credentials below.

| Username        | Password    |
|-----------------|-------------|
| test            | inject      |
| mfg             | vime        |
| debug           | sdf         |

Out of these accounts the `debug` account seems to have access to all the commands
of the other users, likely this is the highest privileged account.

The commands supported are:

```
> help
db [addr] [cnt]
df [mask32]
ec <element#> <val32>
el
es [start] [end]
et [cnt]
ge ge <event #>
ir <prt> <slave> <cnt> [timeout]
irs <prt> <slave> <sub> <cnt> [timeout]
iw <prt> <slave> <B1>..[BN]
iws <prt> <slave> <sub> <B1>..[BN]
nowd
ping <bus#> [cnt]
psctl <ps#> <on|off>
psinfo
pp <phase> [cnt]
pt <post#> [cnt]
r8 <addr>
r16 <addr>
r32 <addr>
rdpcmd <cmd#> [param...]
reboot
ss <card#>
sysinfo
tasks
valpa
vd [port/router/all] [start] [end]
vfcc
vlip
vpc <port> [bypass|unbypass|loopback]
vpd <r/w/reset/erase> [offset] [data]
vpra <RAR>
vps ['c']
vr <RAR> [cnt]
vrs
vsnoop <rx|tx> <src> <dest>
vw <RAR> <val16>
w8 <addr> <val8>
w16 <addr> <val16>
w32 <addr> <val32>
wwnn [m|p]
```

## Example: psinfo

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

## Example: sysinfo

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

## Example: psctl

```
> psctl right on
Powering-on ...

>
Powering-on supply 1
Exiting stand-by mode ...
Starting WWNN Configuration........
Partner not present
[...]
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
