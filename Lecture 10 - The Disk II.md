## 1984: GUIs and the Mac 
- December 9, 1968:  90 minute demo at an ACM/IEEE conference in San Francisco, summarizing work done at ARC.
- Included in demo: computer mouse, window-based interface, etc.
- one of the most important events in development of modern personal computing
- took time to have real impact, required advanced tech

### Xerox
- founded in 1904, sold paper
- 1970: formed a research lab (PARC)
- When ARC funding dried up, researchers moved to PARC
- first invention: laser printer
- ==Alto==: computer to prepare documents to print on laser printer, had a mouse
- 1974: Xerox makes Smalltalk for the Alto, a programming language and development environment.
- Desktop metaphor started
- WIMP: windows, icons, menus, pointers (developed at PARC)
- Xerox star: first commercial computer with GUI, failed (too costly)
- PARC failed

### Apple
- founded in 1976
- 1978: began working on a new computer, ==Lisa==, with a home-made operating system.
- Steve Jobs visited PARC in 1979, and was astounded by the ==Alto=='s GUI.
- brought his team to PARC to get a demo of GUI
- Lisa OS expanded on the Alto's GUI.
What made it special?
- keyboard shortcuts, trash can, icons, drag and drop
- Supported pre-emptive scheduling so it could run many programs at once! (CP/M, MS-DOS did not. Only unix, but no support for GUI)
- Supported virtual memory with segmentation + swapping!
- Had its own filesystem.
What happened to Lisa?
- The Lisa was released in 1983. 
- 1MB of RAM. 
- Price: $10,000 USD ($30,000 today). 
- Commercial failure.
- Apple lowered cost with the Macintosh (128kb ram), did not have multitasking, mmu, user/kernel modes, as did lisa. 
- Macintosh released in 1984, $2,495.
What happened to Macintosh?
- did not do well. 
- slow, no hard drive
- But the GUI had a huge impact on future UI design.
- 1986: The faster Mac Plus comes out. Becomes a hit with creative professionals.
Susan Kare
- designed icons for mac UI

## Flash based SSDs
- no moving parts
- Built out of transistors, like memory and CPU. But, can retain data without power.
- One or more bits are stored into a single transistor. (SLC: single bit, MLD: two bits, TLC: 3 bits)
- An SSD is a **random access device**. 
- Can read any page in about the same amount of time regardless of location. 
- one access takes 10s of microsecond
- Erasing is the first step in actually writing anything to the disk.
- to erase, set bit to the value one (expensive, milliseconds)
- then, change some of the bits to 0's to write (100s of microseconds)
- Organized into **banks** (or planes). 
- Each bank consists of a large number of **blocks** of size 128 KB or 256 KB. 
- Each block contains a large number of **pages** of size 4 KB (or thereabouts).

### Reliability
- Mechanical hard drives can fail for a number of reasons
- flash chips can also wear out (flash blocks slowly accrue a little bit of extra charge, becomes increasingly difficult to differentiate between a 0 and a 1.)
- SLC chips are rated at 100,000 P/E cycles. MLC 10,000.

### SSD structure
- flash chips, volatile memory (for caching, e.g.) and some control logic.
- control logic includes ==flash translation layer== (FTL). Takes in read and write requests and turns them into low-level read, erase and program commands on the device.

#### Direct mapped FTL
**direct mapping**: 
- a read to a logical page N maps to a read of the physical page N.
- a write to logical page N maps to a read of the block containing the physical page N, then erase of the block, and then programming of the block.
- bad performance (costly to write), bad reliability (data is frequently updated)
#### Log-based FTL
(the usual approach)
- Upon a write to logical block N, the device appends the write to the **next free spot in the currently-being-written-to block**. 
- A mapping table stores the physical address of each logical block in the system.
![[Pasted image 20240410120318.png]]
Garbage collection:
- Assume that blocks 100 and 101 are written to a second time, with contents c1 and c2. They will be written to the next free physical pages (4 and 5), after block 1 is erased. The mapping table will also be updated.
- pages 0 and 1 now contain garbage.
- device must reclaim these to make space in a process known as **garbage collection**.
1. read live data from the block 0 (pages 2,3)
2. store them
3. erase block 0
- GC is expensive
- **Overprovisioning**: add extra flash capacity to the drive so that GC can be delayed and done in the background
Advantages of Log-based FTL: 
- erases are only required once in a while in 
- the "read-modify-write" pattern of the direct-mapping approach is avoided completely
- greatly improves performance.
- improves reliability, since writes are spread across all pages.
