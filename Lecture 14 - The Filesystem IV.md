## 1995-2000: Windows 95
Why was it made? 

- Windows 3.0 was problematic: based on MS-DOS with 16 bit architecture (16 bit memory adresses = max 64KB of memory)
- Unix had multitasking and was in general much superior
- 1993: **Windows NT**: Same UI as 3.0, but no longer relies on MS-DOS. 32 bit architecture, NTFS FS instead of FAT
- Windows NT was better than 3.0, but slow, targeted to businesses

What made it special? 

- **Windows 95**: consumer oriented follow up from 3.0, lower system requirements than NT, but less features
- 32 bit, multitasking
- Apps could access 2GB RAM
- desktop metaphor, start button, taskbar

What happened to it?
- Major commercial success. One million copies shipped in first four days. 65 million in first 16 months
- Considered one of the most important products in the PC industry.
- Why such a success? Much easier to use than 3.0, nicer GUI, booted directly from GUI, not MS-DOS, better apps, plug and play (auto configuration)
- Windows 98: small update (web integration), positive reception 

## Memory-mapped files 
Alternative file access method (instead of read, write, seek):
#### mmap() Map the contents of a file in memory 
- On `mmap()`: Allocate page table entries in the virtual memory, Set valid bit to “invalid"
- On access, Page fault, causes page/block to be brought into memory.
- After page fault: set valid bit to true
- Get data to disk for mmap through normal page replacement, or through `msync()`
- `mmap()` is ==good for random access== to large file
- `addr = mmap()`, then use memory addresses in `[addr, addr+len-1]`
- as opposed to `open(file), read(file, buffer),` then use memory address in buffer
- `munmap()` Remove the mapping

**Advantages**: 
- Only accessed portions brought in memory 
- **For large files**
- Much easier programming model: Follow pointer in memory. As opposed to (Seek, Read) every time

## Log-structured Filesystem
Recall log-based FTLs in SSDs, takes a read/write request on logical block and turns them into low level read, erase and program commands on device.
Most FTLs are log-structured: 
- Upon a write to logical block N, the device appends the write to the **next free spot in the currently-being-written-to block**. 
- A mapping table stores the physical address of each logical block in the system.
#### Log-structured filesystem (LFS)
- log means append-only data structure 
- idea is to use disk purely sequentially (easy for writes, hard for read)

**Strategy:**
1. File system buffers writes in main memory until “enough” data (write both Inodes and data)
2. Write buffered information _sequentially_ to new segment on disk Segment: large (MBs) contiguous regions on disk 
3. Never overwrite old info: old copies left behind and garbage collected later

**Implementation:**
- **Map:** file inode number $\rightarrow$ inode location on disk 
- Data structure is called **imap (inode map)**
- It is a table of inode disk addresses. Maps  inode number to disk address of last inode for that inode number
- updated everytime inode is written to disk

Where to store imap? 
- too large for memory 
- write imap in segments, keep pointers to pieces of imap in memory 

**Crash**
- need to recover imap after a crash
- solution: write a copy of imap in a fixed location on disk and update it periodically (called checkpoints) .