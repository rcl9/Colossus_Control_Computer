# The 'Colossus Control Computer' -- A Beast of a Z80 Machine!

This repository documents my "Colossus Control Computer" which was my main go-to development machine ("It has every feature including the kitchen sink") that was started in March 1985.

<div style="text-align:center">
<img src="/Images/Img052.webp" alt="" style="width:75%; height:auto;">
<br>
<img src="/Images/Img12.webp" alt="" style="width:75%; height:auto;">
</div>

I call it a "beast of a Z80 machine" because it has more odds and ends + add-on gadgets than with
most self-built computers. It was also the test bed and development platform for my follow-on
March 1987 <a href="https://github.com/rcl9/Phoenix_MIDI_Computer">Phoenix MIDI Computer</a>.
An interesting sub-project for Colossus was my Dec 1985 '[Drum Pad Triggers for a MIDI Drum Machine](https://github.com/rcl9/MIDI_Drum_Pad_Triggers)'.

## Table of Contents

- [Overview of the Colossus Computer](/Overview.md)
- [The Basic Feature Set](#The-Basic-Feature-Set)
- [Source Files](#Source-Files)
- [Block Diagrams](#Block-Diagrams)
- [Schematics](#Schematics)
- [System Function Overviews](#System-Function-Overviews)
- [Images](#Images)

## The Basic Feature Set

- Z80A 8-bit processor running at 4Mhz.
- Z80 CTC general purpose timer
- Z80 SIO (serial I/O). Port A = RS-232, Port B = MIDI with MIDI to MIDI pass-through mode
- Z80 PIO (parallel I/O) to interface
- 8253 programmable timer
- 8255 parallel ports A, B and C
- 8279 to interface to the keyboard and the 8 segment LED display
- 58167 real time clock with Dallas DS1210 battery backup controller
- 2764 EPROM memory
- 8K 6264 static memory (used for code execution during CP/M3 DRAM bank swapping)
- 1MB of DRAM + controller
- RCL9's "Universal I/O Port" interface. The pinout can be found at the end of [this page](https://github.com/rcl9/Cypher-Z80-68000-Single-Board-Computer----Expansion-Board).
- Atari 800 cartridge 30-pin interface
- Roland M-16C and M-64C synthesizer patch cartridge interface
- 5-1/4" floppy disk controller and interface
- 2 line, 40-position Hitachi HD44780 LCD display
- 8 segment custom LED display
- 8 channel, 8-bit multiplexed analog-to-digital converter based on the TI ADC0809 to measure 8 potentiometers
- [9 drum pad triggers](https://github.com/rcl9/MIDI_Drum_Pad_Triggers) (using piezo-electric microphones)
- MIDI in and out connectors (for a Roland TR-707 drum machine interface)
- Votrax SC-01A speech synthesis + sound mixer and amplifier + supporting software in the monitor ROM
- Numeric keypad
- Scanned ASCII keyboard
- PAL16L8 for I/O port decode (refer to 'decode1.abl')
- Kepco switched mode power supply from Exceltronix (+5v at 4.65A, +12v at 2.8A, +12v at 2A and -12v at 0.5A).

## Source Files

| Filename  | Description |
| :-----: | :---: |
|   |   |
|  [sbcmon.mac](/src/sbcmon.mac) | The firmware for Colossus  |
| [lcd.mac](/src/lcd.mac) | Hitachi HD44780  2x40 custom LCD driver |
| [speak.mac](/src/speak.mac) |  Votrax SC-01A speech synthesis driver  |
| [xmodem.mac](/src/xmodem.mac) |  Xmodem send/recieve routines |
| [simon.mac](/src/simon.mac) | The game of Simon for an [external hardware device](/Images/Img13.webp)  |
| [keytbls.mac](/src/keytbls.mac) |  Keyboard translation tables |
| [dacsynth.mac](/src/dacsynth.mac) |  A simple music tone generator using two DACs |

## Block Diagrams

<div style="text-align:center">
<img src="/Schematics/Overview document/Main board block diagram - Dec 2 1985.webp" alt="" style="width:75%; height:auto;">
</div>

<div style="text-align:center">
<img src="/Schematics/Overview document/Auxiliary Board Block Diagram - Dec 2 1985.webp" alt="" style="width:75%; height:auto;">
</div>

Memory map:

<div style="text-align:center">
<img src="/Schematics/Overview document/Memory map and ROM contents - Dec 2 1985.webp" alt="" style="width:75%; height:auto;"
</div>

## Schematics

<table style="width:100%">
<tr>
    <th style="width:80%">Function</th>
    <th style="width:20%">Schematic</th>
</tr>

<tr><td>1MB dynamic RAM controller - Page 1</td>
<td><img src="/Schematics/1MB dynamic RAM controller - page 1 of 2 - Dec 31 1985.webp" alt="" style="width:20%; height:auto;"></td></tr>

<tr><td>1MB dynamic RAM controller - Page 2</td>
<td><img src="/Schematics/1MB dynamic RAM controller - Page 2 of 2 - Dec 31 1985.webp" alt="" style="width:20%; height:auto;"></td></tr>

<tr><td>5-1/4" disk driver interface</td>
<td><img src="/Schematics/5-25in disk driver interface - June 21 1985.webp" alt="" style="width:20%; height:auto;"></td></tr>

<tr><td>8 channel multiplexed ADC interface</td>
<td><img src="/Schematics/8 channel multiplexed ADC interface - Feb 12 1986.webp" alt="" style="width:20%; height:auto;"></td></tr>

<tr><td>Votrax SC-01 speech synthesizer</td>
<td><img src="/Schematics/Votrax SC-01 speech synthesizer (aux board) - Oct 25 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>8255 I/O port (aux board)</td>
<td><img src="/Schematics/8255 IO port (aux board) - Oct 25 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Atari 800 cartridge interface</td>
<td><img src="/Schematics/Atari 800 cartridge interface - Sept 2 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Auxiliary board decode</td>
<td><img src="/Schematics/Auxiliary board decode - Oct 25 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>CPU - Bus buffering - I/O and memory decode</td>
<td><img src="/Schematics/CPU - Bus buffering - IO and memory decode - March 1 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Configuration I/O ports</td>
<td><img src="/Schematics/Configuration IO ports - March 1 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>LCD interface - Hitachi HD44780 2x40</td>
<td><img src="/Schematics/LCD - Hitachi HD44780.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Inter-board buffers </td>
<td><img src="/Schematics/Inter-board buffers - Oct 24 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Interrupt structure and LED board layout</td>
<td><img src="/Schematics/Interrupt structure and LED board layout.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Keyboard-LED driver</td>
<td><img src="/Schematics/Keyboard-LED driver.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Keypad multiplexing</td>
<td><img src="/Schematics/Keypad multiplexing.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Memory and I/O decode</td>
<td><img src="/Schematics/Memory and IO decode - Aug 27 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Z80-PIO (parallel I/O) interface</td>
<td><img src="/Schematics/PIO interface.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Real time clock (aux board) </td>
<td><img src="/Schematics/Real time clock (aux board) - Oct 25 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Resident 4K RAM and Banked EPROM</td>
<td><img src="/Schematics/Resident 4K RAM and Banked EPROM - March 1 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Roland synth cartridge interface</td>
<td><img src="/Schematics/Roland synth cartridge interface - Sept 2 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Z80-SIO Serial interface</td>
<td><img src="/Schematics/Serial interface - March 2 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Timers</td>
<td><img src="/Schematics/Timers.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>General input port 0x24</td>
<td><img src="/Schematics/General input port 0x24.webp" alt="" style="width:20%; height:auto;"</td></tr>

</table>

## System Function Overviews

<table style="width:100%">
<tr>
    <th style="width:80%">Function</th>
    <th style="width:20%">Document</th>
</tr>

<tr><td>Software and hardware interrupts</td>
<td><img src="/Schematics/Overview document/ROM Software Structure - Dec 2 1985.webp" alt="" style="width:20%; height:auto;"></td></tr>

<tr><td>8253 programming</td>
<td><img src="/Schematics/8253 programming - Aug 31 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>8255 bit assignments + Universal I/O Port</td>
<td><img src="/Schematics/8255 bit assignments.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Auxiliary board I/O map</td>
<td><img src="/Schematics/Auxiliary board IO map - Oct 25 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>I/O driver functions</td>
<td><img src="/Schematics/IO driver functions - Nov 30 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Keyboard special function numbers </td>
<td><img src="/Schematics/Keyboard special function numbers - Nov 30 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Main board I/O map</td>
<td><img src="/Schematics/Main board IO map.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>MIDI driver functions</td>
<td><img src="/Schematics/MIDI driver functions - Feb 14 1987.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>RS-232 serial driver functions</td>
<td><img src="/Schematics/RS-232 driver functions - Feb 14 1987.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>Static RAM vector table</td>
<td><img src="/Schematics/Static RAM vector table - Nov 30 1985.webp" alt="" style="width:20%; height:auto;"</td></tr>

<tr><td>System driver functions</td>
<td><img src="/Schematics/Systen driver functions - Jan 9 1986.webp" alt="" style="width:20%; height:auto;"</td></tr>

</table>

## Images

<img src="/Images/Img1.webp" alt="" style="width:75%; height:auto;">

The main controller board:

<img src="/Images/Img8.webp" alt="" style="width:75%; height:auto;">

And its auxiliary daughter board:

<img src="/Images/Img9.webp" alt="" style="width:75%; height:auto;">
<img src="/Images/Img12.webp" alt="" style="width:75%; height:auto;">
<img src="/Images/Img11.webp" alt="" style="width:75%; height:auto;">
<img src="/Images/Img3.webp" alt="" style="width:75%; height:auto;">
<img src="/Images/Img4.webp" alt="" style="width:75%; height:auto;">
<img src="/Images/Img5.webp" alt="" style="width:75%; height:auto;">
<img src="/Images/Img6.webp" alt="" style="width:75%; height:auto;">
<img src="/Images/Img7.webp" alt="" style="width:75%; height:auto;">

Rear view of the LED display panel and its drivers:

<img src="/Images/Img10.webp" alt="" style="width:75%; height:auto;">

[Drum pad triggers for a MIDI drum machine](https://github.com/rcl9/MIDI_Drum_Pad_Triggers):

<img src="/Images/Drum Pads System.webp" alt="" style="width:75%; height:auto;">

The game of Simon which connects to the parallel port:

<img src="/Images/Img13.webp" alt="" style="width:75%; height:auto;">
