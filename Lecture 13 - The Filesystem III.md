## 1989-1995: NeXTSTEP
Why was it made?

- September 16, 1985: Steve Jobs resigns from Apple
- starts a new computer company: NeXT, hiring apple employees
- decided to make a computer workstation (designed for scientific applications. more powerful (and expensive) hardware than a PC)
- $6,500

What made it special?

- ran the NeXTSTEP operating system, a Unix-like OS.
- Based on the Mach kernel and BSD Unix.
- Dock and shelf, Better drag and drop, Real-time scrolling, window-dragging
- written in Objective C (OOP)
- sold to student and universities, 50,000 units sold

What happened to it?

- Acquired by Apple in 1997.
- Steve jobs becomes interim CEO

## In-memory Data Structures
#### Cache 
The goal is to store as much as possible in the cache to avoid I/O reading and writing (slow). The cache is :
- Fixed contiguous area of kernel memory 
- Size = max number of cache blocks x block size 
- A large chunk of memory of the machine

**Strategy:** Write behind cache
	write stuff to the cache (fast), and then in the background, it will progressively get written to file

**Cache directory**

- usually a hash table 
- index = hash(disk address)
- holds a dirty bit (indicates if data must be written to disk, vs read)

**Cache Replacement**

- Keep LRU list: easy to do (less accesses)
- If no more free entries in the cache: Replace “clean” block according to LRU, Replace “dirty” block according to LRU

**Cache Flush**

- find dirty entries, write back to disk

### File tables
1. (System-Wide) Active File Table
	- One array for the entire system (one entry per open file)
	- Contains little info: File inode, ref_count of number of file opens
2. Per-process Open File tables
	- One array per process (one entry per file open of that process)
	- indexed by file descriptor `fd` (returned from `fopen()`)
	- Contains: pointer to file inode, file pointer `fp`
## Setting up the filesystem
- By default, OS sees all storage devices as chunks of unallocated space (unusable) 
- Cannot start writing files to a blank drive, need to set up the FS first
#### Disk partitioning 
- A partition is a **container** for the storage device. 
- Multiple partitions can be installed on the same OS by slicing
- Each partition has a partition table containing the location and the size
- OS reads the partition table before any other part of the disk

#### Mounting a filesystem
- the FS lives in the partition, but before OS can read/write to it, must ==mount== the FS
- Mounting attaches FS to a directory 
- Directory is called **mount point**

**Multiple FS's**
- A user may want multiple filesystems such as main disk, backup, usb drive, etc.
- **Idea:** Stitch all the FS together into `root` directory
- `root` (/) is always mounted because of ==Booting==.
- Booting is the first thing that happens when computer starts
- **Mounted FS Table** keeps track of the mounted filesystems

**Booting**
- BIOS looks for what it needs to start OS at the **boot block**
- boot block contains boot loader and partition table

**FS startup**
- Common to “check” the file system (fsck)
- `fsck` checks that no sectors are allocated twice, no allocated sectors are on the free list, etc 
- 
