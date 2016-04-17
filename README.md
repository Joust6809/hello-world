# hello-world
following instructions from https://guides.github.com/activities/hello-world/
My Github user name, Joust6809 is related to a video game released in 1982
The name of the game was Joust from Williams Electronics, inc based in Chicago
The video game was using a microprocessor from Motorola, the 6809
Since I was more motivated and advanced than any students or teachers in the area where I lived
I had to learn the hard way.

I made a logic analyzer... with $50 worth of parts as one of my first project
to verify, clarify and prove to myself that the information given in the book "Z80 Processor"
was accurate, and that I could uncover the exact detail of how this video game was working.

A logic analyzer normally cost about $20 000 dollars, so how could I make such a cheap one?

The project started as a manual EPROM reader. I used a plastic box which came with
a weller soldering iron (consider it as free). I used 4 TTL chips, 74LS161, 4 bit counter, 
to create a 16 bit binary counter : this would be my address bus.
I installed 16 red LEDs in the plastic box to show the address from the 4 chips 74LS161
I installed a 24 pins connector to install an EPROM chip, 2732, 4K bytes
(4096 bytes * 8 = 32768 bits, which explain the "32" end of the EPROM 2732.
I installed a second row of red LEDs, 8 only, to display the output of the data bus from the 2732 EPROM
I installed 2 push button switches : one is Reset to set the address bus to 0 (all 16 LEDs for the address OFF)
and the other switch is "Clock" to increment the address by 1. 

I brought this EPROM reader at the college a few times to write a few pages of binary data
as found from the first EPROM of miss pacman, the one used by the Z80 processor when the reset input is applied
You may realize that I could not afford a printer at that time. I wrote about 30 pages
each with about 20 address in hexadecimal and the 8 bit data in binary.

You may assume that I was a strange kid to spend so much time doing something 
which could be a total waste of time. But I skipped some details here.
I first read about 10 bytes from the EPROM, using wires that I connected/disconnected from each pin
of the address bus of the EPROM and reading each bit with a radio shack logic probe on the data bus. 
I searched in the Z80 reference each of the opcode which were listed in alphabetical order 
and were described in binary. It took me dozens of hours to search thru the entire book 
for each 8 bit opcode. I got the first instruction that initialize a few registers
then a conditional branch using relative addressing with a small negative value. The code was performing a loop
On each loop, the content of a 8 bit register (set to zero before the loop started) was written to an address
pointed by a 16 bit register. The 16 bit register was incremented then a comparison was made
I knew that some device would need to be set to a known state when booting.

At that point, I got convinced that I was on the right path to study how the boot operation is done in miss pacman
Building the EPROM reader took about 10 hours, but made the reading of the first few hundreds of bytes
so much faster than stupidly disconnecting the address line A0 from ground, untwisting the wire and twisting it
on the +5 volt wires and deal with the wires that broke from such twisting/disconnecting and not be sure
if all the address lines are set to the value I think it should.

After I did write in a notebook about 1000 values and spent time to write the Z80 instruction corresponding
to each binary data, I wanted to verify if I understood correctly how to calculate the relative address of conditional instructions

I addess a 40 pin socket to the EPROM reader. I disconnected the 4 TTL chips that generated the address and connected 
these 16 LEDs to the 16 pins of the Z80 processor (out of the 40 pins of the socket). Therte was a resistor of 1K on each LED
to take little current, in order to not overload the Z80. The clock switch was connected to the clock input of the Z80 processor
The same for the reset switch. Brief, the weller plastic box was looking as follow:
one row of 16 LED connected to the Z80 address bus, with 12 of these wires also connecting to the EPROM address bus
one row of 8 LEDs connected to the Z80 data bus and the EPROM data bus
one 40 pin connector for the Z80 processor
one 24 pin connector for the 2732 EPROM
two switches : reset and clock
The 40 pins Z80 chips was using 16 pin for the address + 8 pins for the data : 24 pins, the remaining 16 pins inclided Vcc (+5 volt), Gnd (0 volt), Clock, reset and 12 more pins...

When I powered up this simple, very specialized logic analyzer, and pressed the clock switch a few times, the address LED showed a binary value that progressed in normal binary sequence : A0 light up, then A1 light up then both A1 and A0 light up, then A2 alone, etc. I was comparing the data displayed by the 8 LEDs with what I wrote in my notebook and everything matched.
Then, as the Z80 reached the conditional branch, the next clock effectively showed that the address bus was being set
to a value lower than the current one. Pressing the clock switch a few times showed that this sequence was repeating many times

A few years later, I was using an EPSON desktop computer (x86 at 4.77 MHz, the slowest that ever existed) to study Joust and Robotron.
I was thankfull of the "Norton Editor" (NE.exe), a DOS text editor which could open a file that exceed 64K. All other editor, including Notepad on windows 3.1 could not open any text file bigger than 65535 bytes. But the 48K ninary files (12 EPROMS of 4K each)
would take about 300K once the disassembler would transform that binary data into text editable file.

The reverse engineering of executable such as the video games Joust and Robotron required to search thru the entire text file
many times, in order to find every instance where a given 16 bit value was used. This indicated that a specific hardware
or a data structure was referenced in these location in the code.Using the schematic, I could identify the address at which
each hardware devices would be referenced. For example, I found that the viddeo RAM DAC which associate each of the 16 color (4 bit per pixel) to a 256 color value (3 bit for red, 8 levels, 3 bit for green and 2 bit for blue) was being referenced in two locations.
One location was in EPROM 12, which is the boot EPROM for the 6809 processor, since the vector table was set at
the end of the addressing values (for example, the address to execute after RESET is indicated by the two last bytes 
at address $FFFE and $FFFF). I would assume that the location in EPROM 12 which reference the RAM SAC was used to set a default value
for every of the 16 bytes. I would then assume that the other location was inside a timer interrupt routine, since Robotron
is known to cycle some of the color to create repetitive color variation of a large number of objects spread everywhere in the display.

I will edit that document later to describe in a more logical way some of the steps I took to study software engineering and explain
my motivation, the reason why I followed each of these steps.
