## IBM PC & CP/M
 - **IBM**: founded in 1911 as a record keeping / measuring system company. Initial forays in computers: 1940s. 1964: dominant in computer mainframe industry (system/360, OS/360)
 - IBM was late to the microcomputer game, they needed to enter the market with a strong product
- They started work on the **IBM Personal Computer** (1980), released in 1981.
- The IBM PC used Intel's newest 8088 microprocessor, a variant of the 8086 (x86).
- IBM talked with **Gary Kildall** (creator of CP/M) about having an x86 version of CP/M come pre-installed on their PC, could not reach a deal, so IBM looked elsewhere.
- Tim Paterson of ==Seattle Computer Products== was awaiting CP/M for the 8086/88 chip, since sales of their computer kits were falling. 
- Tim Paterson wrote his own version of CP/M for x86 (QDOS). 
## 1981: MS-DOS
why it was made? 
- 1975: Bill Gates (19 years old) & Paul Allen (22) found Microsoft to distribute an implementation of BASIC for the Altair 8800 (first microcomputer)
- ==IBM, Microsoft, and SCP== were business partners
- Bill gates bought QDOS fro $75,000, renamed it to ==MS-DOS==, and licensed it to IBM
what happened to it and why?
- MS-DOS became the OS of choice for 8086-like chips.
- Manufacturers would license the OS and add their own device drivers, then pre-install it on their machines.
- IBM PC was a success: High build quality, competitive price
- Highest sales in microcomputer market. 200,000 sold per month by second year. IBM becomes most valuable company in world by 1984.
## I/O devices
A device has 2 important components:
1. ==Hardware interface (protocol):== Allows the OS to control its operations
2. ==Internal Structure:== hardware and firmware (software within hardware)
#### Protocol
consists of 3 registers, by reading and writing these , the OS can **control device behavior**
1. ==**status** register==, the current status of the device;
2. ==**command** register,== to tell the device to perform a certain task
3. ==data register==, to pass to or get data from device
#### Polling
- OS repeatedly checks the status register until the device is ready to receive a command, and then until the device is done executing the command.
- inefficient 
![[Pasted image 20240410102047.png]]

#### Interrupts
To avoid wasting ressources when waiting for I/O
1. Put process requesting I/O to **sleep**
2. **Context switch** to a different process 
3. When I/O finishes, wake sleeping process with an **interrupt**
==**Remember: Interrupts vs Syscalls.**== 
- **Interrupt:** generated by hardware to initiate a context switch
- **Syscall:** generated by process, to request functionality from kernel mode. Also, initiates a context switch.
Advantage: No waste of CPU cycles
Disadvantages: Expensive to context switch 
### Device drivers 
- The OS should be device-neutral.
- E.g., the filesystem should not need to know the specifics of what kind of disk it is writing to or reading from.
- We encapsulate specifics of device interaction in a ==**device driver**==. (70% of linux kernel is device drivers)
## Hard drives
- The main form of persistent storage. 
- A drive consists of an array of **sectors** (512-byte blocks) that can be read/written. 
- a single sector write is atomic
#### Disk Geometry
- made up of ==platters==: circular surfaces on which data is stored by applying magnetic charges.
- platters are bound together around a ==spindle== that spins (around 7,200 rpm)
- Each surface of a platter has thousands of concentric circles called **==tracks,==** which contain the sectors
- Disk arm moves across surface to position disk head above track
- a cylinder consists of a stack of platters
#### Time to read:
1. select platter (electronic switch) ~nanoseconds
2. seek arm to right track (linear in number of cylinders on a platter) ~ 3-12 milliseconds
3. rotational latency (linear to the number of sectors)
	Average rotational latency: 1/2 revolution
	rotational speed: 4500-15000 RPM
	One revolution: 1/ (RPM / 60) sec
	~2-7.1 milliseconds
4. Transfer data ~ 1GB /sec = 0.5 microsecond / sector (512 bytes)
