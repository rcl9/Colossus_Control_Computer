# Overview of the Colossus Control Computer

## Hardware Description  

The hardware is based around the Z80A processor with the Z80 
PIO (programmable I/O processor), Z80 SIO (serial I/O) and Z80 
CTC (counter timers) (see block diagram sheets below) as the main limbs 
of the system. These communicate with the Z80 through vectored 
interrupts. As interrupts come in, the Z80 asks the peripherals 
where the driver is in memory - the peripheral responds with a 
pointer to a table of addresses in memory, the 'vector table' 
(refer to the '<a href="/Schematics/Overview document/ROM Software Structure - Dec 2 1985.jpg">hardware interrupts</a>' diagram). This table is the backbone 
of the flexibility of the system. A user application program can 
be loaded into the system which PATCHES its own interrupt handler 
addresses into this table, thus defining what the interrupts will 
do. The Z80 stops what it was doing and picks up the vector from 
the table and starts execution there (this is what I call a 
background process - it is not seen by software running in the 
foreground). The interrupt handler services the Z80 peripheral 
and returns control back to the main foreground program (which is 
usually the SBCMON Z80 resident monitor when no application is 
running).

<div style="text-align:center">
<img src="/Schematics/Overview document/Main board block diagram - Dec 2 1985.jpg" alt="" style="width:40%; height:auto;">   
<img src="/Schematics/Overview document/Auxiliary Board Block Diagram - Dec 2 1985.jpg" alt="" style="width:40%; height:auto;">
</div>

The <a href="Schematics/Serial interface - March 2 1985.jpg">SIO</a> is dedicated to a MIDI bus and a RS-232 channel, the 
<a href="Schematics/PIO interface.jpg">PIO</a> is free for interrupt driven applications (such as for the MIDI drum 
pads interface), and the <a href="Schematics/Timers.jpg">CTC</a> has 4 channels free for interrupt 
timing (ie. for timing incoming MIDI information for playback, or 
for keyboard auto-repeat).

These 4 hardware interrupts are the main communication 
channels to the outside world. They do not slow down system 
response since they are interrupt driven (the Z80 stops only when 
data is ready) and all data is stored in buffers until the 
application programs need it. Supplementing these basic chips 
(other than the standard memory) are:

- 32.25 kbaud MIDI bus (Musical Instrument Digital Interface) 
which allows the computer to talk with drum machines and 
synthesizers.

- 32.25 kbaud RS-232 serial interface. This is used to talk to the host 
development computer.

- 8279 keyboard driver (the keypad is mapped on top of the 
keyboard), and multiplexed 6 digit LED display with separately 
controlled colons and decimal points.

- 8255 programmable I/O controller. Two ports are used to 
implement my 'Universal I/O parallel port' to which I can attach my 
EPROM programmer, EPROM emulator, the demonstration SIMON game 
and a Centronics printer.

- 2, two line by 80 character EPSON EA-Y80025AT LCD displays which can be 
used by the application programs for displaying data or can be 
used as the main output (without external terminal).

- 2797 Double Density disk controller for 5" disk systems.

- 58167 real time clock for date and time information.

- GI 8910 complex sound chip.

- Votrax SC-01A speech synthesizer with sound tables in ROM
accessible through the SYSTEM$DRIVER software interrupt.

- ADC0809 8 channel multiplexed ADC for inputting of ratiometric 
inputs (ie, slider or potentiometer position of BEATS PER MINUTE 
control of the sequencer portion of this computer).

- Interrupt driven footswitch, application to be determined by 
user. Present use is as the Bass Drum Trigger, and as the MIDI 
Sequencer start/stop switch (re-definable through the vector 
table).

- Atari 800 cartridge interface mapped into the ROM bank. This is 
used to read Atari cartridges so they can be copied onto an EPROM 
cartridge.

- Roland standard M-64C cartridge interface. This is used to 
read/program a JX8P synthesizer RAM pack and back up the voices 
on disk (the contents are uploaded to the development system 
using XMODEM to be described later).

Expansion is very simple as I have decoded the 255 
possible Z80 I/O ports and sub-divided them into blocks of 16 
and 4 ports. So new hardware just picks for its CS* (low level 
chip select) either one of the decoded 16 port blocks (if device 
needs 16 ports - the clock chip), or the decoded 4 port block 
(the CTC, PIO, etc) or can further subdivide the 4 port block 
into single I/O port selects (the configuration output port uses 
this single block select).

## Memory Map (see <a href="Schematics/Overview document/Memory map and ROM contents - Dec 2 1985.jpg">accompanying page</a>)

The computer has been designed with large amounts of memory 
in mind for future uses in Synthesizer sampling data storage 
which needs about one megabyte. Since the Z-80 can only address 
64k, the memory has been cut into 16 banks of 60k of dynamic 
memory, one bank of system rom and cartridge I/O, and 4k of non-
banked static memory where parts of the monitor resides at all 
times. The static memory is partly used as a detour to gain 
access to the ROM bank from one of the dynamic memory banks (to 
be described later). Software running in the dynamic memory calls 
1 of 5 vectors in zero page that in turn jumps up to the static 
memory where it banks out the dynamic memory, banks in the ROM 
(or even another memory bank), then executes the indicated 
command of the program, returns to the static memory and reverses 
the process. This is my way of implementing inter-bank 
communication so we have a pseudo 1 megabyte of addressable 
program/data RAM + 60k of ROM/cartridge space.

## System Software

Refer to the "System Function Overview" section of the main [ReadMe file](/ReadMe.md) for 11 image documents.

The main system control software is a complex matrix of low 
level drivers and interrupt handlers that can be used to service 
the interrupts or be replaced with downloaded user application 
software which defines new handlers (ie. the drum pad software 
replaced the PIO and Footswitch interrupts). The matrix of 
routines is accessed through the interrupt vector table (see 
sheets) whose address is always fixed just below the top of 
memory. This is so external software (from disk or development 
computer) can modify the vectors in this table knowing that the 
locations won't change from version to version.

The function of the interrupt handlers is to acknowledge the 
interrupt, and set flags in the STATIC RAM VARIABLES AREA of 
memory, or get or put the data in one of the buffer tables. The 
users' program just looks at the buffers to see if data has 
arrived, it never actually touches the I/O to the outside world - 
that is the job of the low level drivers and handlers. Also the 
different buffers hold data for one process as another is doing 
I/O so no data is lost in the system.

 Apart from these low level routines, we have the following:

- SBCMON resident Z80 monitor. This allows a user to debug the 
computer hardware and software using the following commands:

```
 b(audrate)  <-- select new baudrate
 c(opy) c<source_start>,<source_end>,<dest_start>
 d(ump) d<start>,<end>
 e<address>  <-- enter data in memory
 f<start>,<end>,<byte> <-- fill memory with byte
 g<address>  <-- execute code
 i<port#>  <-- input from port
 m(submenu)  <-- enter Midi submenu
 o<port#>,<data> <-- output to port
 p<lays> game of Simon <-- Simon foreground software
 s<start>,<end> <-- Xmodem protocol send
 r<destination> <-- Xmodem protocol receive
 t<memory_start>,<memory_end> <-- test memory
 ? print this help menu
```

- An XMODEM transfer program. Xmodem is the most popular modem 
transfer program for microcomputers and has been implemented on 
this system so application software may be uploaded or downloaded 
from the host development system. Its present use has been to upload 
the contents of Atari cartridges and JX8P synthesizer RAM 
cartridges to the host development system's disk system for storage.

- 5 software interrupts. These are very important to user 
software communicating with the ROM. Z80 software interrupts are 
called 'RST XX'instructions, ie. RST 8 calls location 8, RST 10h 
calls location 10h, etc (see sheets for descriptions). I have 
defined these as the only legal way to call the ROM software from 
outside the ROM (ie from user software in alternate banks of 
memory). Each bank of memory has these 5 vectors at the bottom of 
memory, so jumping to them from any bank will switch in the ROM 
bank, execute the ROM code function, switch back the RAM bank and 
return the parameters (if any) to the RAM software. The user sets 
up the registers for a pre-determined function and does a RST to 
one of the 5 entry vectors (see list) which executes one of the 
resident monitor low level routines. Such an example would be to 
print '65535' to the LED. The program would be:

```
0e 10		ld c,10h 		; Function 10h = print DE on LED as decimal number.
11 ff ff	ld de,0ffffh	; Input parameter 65535 to be printed
cf			rst 08h 		; Call ROM IO$DRIVER
```


	This code would call the ROM IO$DRIVER, which would see it is a LED command, jump to the LED$DRIVER, which in turn would see it is the 'print register DE on LED display as a left justified ASCII number' and execute this low level driver routine.

	These five interrupts allow outside software to gain access to all parts of the ROM without having to know its address. This is a problem with other computers like IBM clones which have to have the ROM routines aligned with IBM's code so external software can run correctly (alot of IBM programs call the ROM code directly, not indirectly as all software should be written). The Colossus system allows us to modify the ROM code and not affect any previously written external software.

- MIDI submenu which selects one of a few MIDI processing 
programs. These utilize the built in hardware and require no 
external hardware (except for the drum hardware if using that 
part). The options include arpeggiator for a JX8P synth (which 
doesn't have one), software that emulates the $500 MPU-401 Roland 
MIDI Data Processor, and the drum pad mapping software (user can 
tell processor what pad maps to what MIDI channel, note on, and 
velocity).

- Extensive low level routines, some of which can be accessed 
from the SYSTEM$DRIVER software interrupt. These are routines 
used throughout the ROM to make programming easier, such as a 
decimal output routine, decimal input routine, get line from 
console with editing features, etc.

