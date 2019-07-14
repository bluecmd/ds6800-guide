# ZP Processor

The ZP processor seems to be the responsible party to read product data from the rear display panel (RDP).
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

It seems to have a console indicated by the `> ` symbol if you press enter. Currently there are no known commands.

## Library

The main controller firmware has references to things like ZP_ReadRDPVpd which seems to be referring to this processor. Probably
this means that all the read/writes to the RDP is handled by the ZP. 

These are the currently known utilities that seems to be communicating with the ZP:

 * dadloadenclosureg
 * iss_diskvpdg
 * issses_activateeventsg
 * issses_bypassg
 * issses_checkpostg
 * issses_checkstatusg
 * issses_cleareventsg
 * issses_connectorsg
 * issses_controlidlightg
 * issses_diskinfobyencg
 * issses_disklocationg
 * issses_encvpdg
 * issses_getdevicedatag
 * issses_getdiskcountg
 * issses_getdiskinfog
 * issses_getencidg
 * issses_getencserialg
 * issses_getenctempg
 * issses_getfanspeedg
 * issses_healthcheckg
 * issses_pcmg
 * issses_porterrorcountg
 * issses_poweroffg
 * issses_queryidlightg
 * issses_readdatetimeg
 * issses_readeventsg
 * issses_reseticg
 * issses_resetswapbitg
 * issses_setdatetimeg
 * issses_setencidg
 * issses_setfanspeedg
 * issses_showencconng
 * issses_slotinfog
 * issses_statesaveg
 * issses_testg
 * issses_updatevpdg
