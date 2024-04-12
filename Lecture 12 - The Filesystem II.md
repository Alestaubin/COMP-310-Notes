Same UI as Windows 3.0, but new kernel written from scratch -- no longer reliant on MS-DOS.## 1991: Linux 

why was it made? 

- Unix couldn't run on microcomputers
- But unix had its advantages (written in C, portable, open source)
- Because of unix's soft license, by 1991, there were over 30 commercial versions of Unix
- 1988: IEEE introduces the Portable OS Interface (POSIX) specification (many of today's OS's are POSIX compliant)
- Unix experienced a decline: compatibility problems between variants, high license fee compared to windows, rise of Microcomputers
- MINIX (mini-Unix): a Unix-like OS created in 1987 by Andrew Tanenbaum, who sold source code for $69. Microkernel architecture.
- Linus Torvalds, Finnish CS student (21 years old) makes a post on the comp.os.minix newsgroup. Announces a "hobby" Unix-like kernel.

what made it special?

- Linux was based on GNU tools/C compiler and MINIX kernel. But had a monolithic kernel as opposed to MINIX. 
- Linux was distributed under GPL license: anyone can run, study, share, modify.
- Linux development took place through email and newsgroups, unlike prior OS's. 

what happened to it?

- 1990s: interest begins to coalesce around Linux (free, open-source, companies could modify it and re-sell it) 
- IBM, Intel and other big companies began to adopt it.
- today, linux is the leading unix-like OS
- 13 million lines of code, 5 patches per hour, 15% of contributions are from independent developers 
- linux hosts 85% of websites, all 500 top supercomputers, amazon servers
- Torvalds would later go on to create Git in 2005

## User Data Allocation in a Filesystem
- Internal fragmentation
	How much of a file's allocated blocks is actually being used by the file?
- External fragmentation 
	Location and size of free blocks (could pose a problem to allocate new files);
	Location and size of allocated blocks (could pose a speed problem).
#### Contiguous allocation 
- **Strategy:** Allocate file data blocks contiguously on disk 
- **Metadata:** Starting block + size of file
- **Advantages:** sequential access perfect, low metadata overhead, random access simple
- **Disadvantages:** High fragmentation (unused holes), may not be able to grow file without moving it.
#### Extent-based Allocation - used in HFS+ / NTFS
- **Strategy:** Allocate multiple contiguous regions (called **extents**) per file 
- **Metadata:** Array of extents. Each entry contains extent starting block and size
- **Advantages:** files can grow, less external fragmentation, good sequential access, random access simple
- **Disadvantages:** Metadata can get large if many extents
#### Linked-List Allocation - used in Unix
- **Strategy:** Allocate linked-list of blocks 
- **Metadata:** Location of first block of file, plus each block contains pointer to next block.
- **Advantages:** no external fragmentation, low internal, can grow
- **Disadvantages**: sequential access is not guaranteed, random access slow, pointers waste space in data blocks 

#### File-allocation Tables (FAT) - used in MS-DOS
- **Strategy:** Keep links information for all files in a data structure on disk, called the **file allocation table (FAT).** _Optimization: the FAT can be cached in memory._ 
- **Metadata:** Location of first block of file, plus FAT table itself.
- **Advantages**: same as linked list, except better for random access if FAT is cached in memory + metadata is low for small files 

#### Indexed Allocation - Used in linux
- **Strategy:** Keep pointers to blocks of file in **an index in the file’s inode.** Cap at maximum N pointers. 
- **Metadata:** Index for each file.
- **Advantages:** No external fragmentation, low internal fragmentation, Can grow easily, Efficient Random and sequential access, metadata is low for small files. 

**Indirect Blocks** 
- if max number of data blocks is reached in Indexed allocation
- do not contain data, but instead pointers to data blocks
- can also have double indirect blocks
- Combined with Indexed Allocation, makes the storage of very large files possible