## 1974: CP/M
**Microcomputer:** small, inexpensive computer, made for hobbyists instead of large corporations, first introduced in the mid 1970s. They were small enough to put onto a desk.
**Floppies:** small, inexpensive storage device introduced in 70s.
**Disk OS:** typically used a floppy disk to boot, provided a file system for the disk and the ability to run programs on it. They were for single users. 
why it was made? 
- It's a Control Program for Microcomputers. Created by Gary Kildall and his company Digital Research (DRI).
what made it special?  
- The most popular OS for microcomputers in the 70s.
- Sold by mail-order to hobbyists for $75. 
- 1982: Annual sales over $20 million.
Components:
- Basic Input/Output System (**BIOS**): Controlled hardware components (e.g., device input, output to display, reading/writing disk sectors). 
- Basic Disk Operating System (**BDOS**): Filesystem, set of system calls. 
- Console Command Processor (**CCP**): Shell 
what happened to it and why
- Since CP/M was dominant in this time period, software that targeted CP/M could run on a large array of different machines.
- Companies started making software products for microcomputer users to buy directly (became profitable).
## Demand Paging
- Reason for demand paging: no machine has 2^64 bytes of memory. 
- Shorter process startup latency 
- Can start process without all of it in memory (Even 1 page suffices)
- Better use of main memory (Program often does not use certain parts)
#### Where is the program?
- Part of it is in memory, (Typically) all of it is on disk in the **Backing Store**.
- Difference with swapping: 
	**Swapping** **= _all_** of program is **in memory OR _all_** of program is **on disk** 
	**Demand paging** **= part** of program is **in memory**
- Recall: CPU can access memory directly, but needs to go through OS to access disk.
- When program needs to access a page that's on disk (_page fault_), program suspended, OS runs, gets page, program restarts 
#### 1. Discover page faults
- valid bit = 0: page is invalid, OR page is on disk 
- valid bit = 1: page is valid AND is in memory
- OS needs an additional table to tell whether invalid/on disk
#### 2. Suspend faulting process
- invalid bit generates a trap into os
- save PCB before trapping
#### 3. Get page from disk
- allocate a frame 
- os finds page on disk
- transfer from disk to frame (DISK IS VERY SLOW)
While disk is busy transferring: 
- Invoke scheduler to run another process 
- when disk interrupt arrives, suspend process and get back to page fault handling
Back to handling page fault: 
- update the page table 
- set the valid bit to 1
- Invoke scheduler to run process 
#### 4. Process runs again
**Restart the previously faulting instruction**
 Process now finds 
 - Valid bit to be set to 1 
 - Page in corresponding frame in memory 
 - Note: different from context switch, which continues with the next instruction

## Page-replacement policies
If no free frame available: Pick a frame to be replaced, invalidate its page table entry (and TLB entry) 

#### Good replacement policy is important:
Normal memory access: ~nanoseconds 
Faulting memory access: Disk i/o ~ 10 milliseconds

### OPT
the optimal policy: replace the page that will be referenced the furthest in the future.
(impossible, but good basis for comparison)
### Random
easy to implement, but clearly not optimal (most likely)
### FIFO
- Oldest page is replaced (Age = time since brought into memory)
- Easy to implement: Keep a queue of pages 
- Fair: all pages have equal residency 
- But does not take into account hot pages that are used a lot
### LRU 
- Replace least recently accessed page 
- With locality, LRU approximates OPT
- Harder to implement, must track which pages have been accessed
Too expensive to implement exactly:
- Need to timestamp every memory reference 
- But can be (well) approximated

Implementation :
Use a **reference bit** in page table 
Hardware sets bit when page is referenced 
Periodically Read out and store all reference bits Reset all reference bits to zero 
Keep all reference bits for some time 
**Page with smallest value of reference bit history**

## TLB replacement policies
the TLB (which caches address translations) can also become full.
TLB replacement policies are similar to page replacement policies (e.g., LRU)
Implementations have to be very fast (since the TLB must be very fast), and are typically done in hardware to avoid going into the OS.