## 1964: OS/360
- General-purpose mainframe computers called **System/360**.
- Large range of performance, some cheaper, small, some larger, scientific.
- All used the same set of instructions
why it was made? 
- The goal was that all System/360 computers could run **the same programs** without need for modification.
- System/360 computers would use the same set of instructions
what made it special?  
- **First** OS that could work on different hardware configurations and **abstract** the details of the differences from programs (this is one of the most important purposes of today's computers).
- major shift: before that, software had to be rewritten to work on different models of a computer.
what happened to it and why
- More than 1,000 developers hired.
- Tight deadlines, and infancy of software engineering practices, led to huge delays, and over 1,000 known bugs at launch.

## Free space management

A **free list** contains a set of elements that describe the free space remaining in the heap.

1. Splitting memory
	Occurs when the number of bytes requested is smaller than the smallest free chunk. It then needs to be split.
2. Coalescing memory
	When memory is freed, the allocator coalesces (merges) that memory with any adjacent free chunk on the list.
3. Keeping track of allocated chunks
	When `free` is called, no need to specify size, allocator keeps track of the size of each allocated piece of memory. 
2. Memory for the free list
	Stored as a linked list in the memory
	Each allocated piece of memory has a **header** block. (So if the caller asked for N bytes, the allocator must find a chunk of size N + sizeof header).
	Free chunks also have headers
### Allocation strategies 
- Best fit: returns smallest chunk that will fit the requested amount.  Need to check all of memory.
- Worst fit: Find largest chunk, split it and return the first part. Keep the second on the free list. Need to check all of memory. Leads to excess external fragmentation.
- First fit: return first chunk that is large enough. Fast: no need to check all free chunks. Beginning of free list gets polluted with small objects. 
- Next fit: Like first fit, but maintains a pointer to the location in the list where the last search ended, starts looking there for the next search. Allocations are made more uniformly through the list, so the beginning is not crowded with small objects. Also fast. 

## Paging
- **Paging**: chop space up into fixed-sized pieces. 
- **Pages**: fixed-sized units to store a process' address space (code, heap stack).
- **Page frames:** (physical memory) an array of fixed-sized slots.
- **Virtual Address Space:** linear from 0 up to a multiple of page size.
- **Physical Address Space:** Noncontiguous set of frames, one per page (frame size is same as page size)
- **Page table:** a data structure kept by the OS for each process, stores the mapping between each virtual page of the address space and its place in physical memory. 
#### Advantages of Paging:
**Flexibility**: no assumptions needed about how heap and stack are used, or which direction they grow, etc. 
**Simplicity**: free list of free pages. Easy to slot in pages.

**Two components in the virtual address**
- VPN: virtual page number 
- Offset (page frame number): offset within the page; Page size = $2^{\text{offset}}$
#### MMU for paging
- **Page table**, Indexed by page number, Contains frame number of page in memory
- Each process has a page table
- _Also need:_ **Pointer** to page table in memory, **Length** of page table

Page table has length **2^p**
Page table covers only the pages that the process has allocated.
Have valid bit in each page table entry:
- Set to valid for used portions of address space
- Invalid for unused portions

### Facts: 
- easier to find memory than with segmentation (fixed size)
- part of the last page of a process may be unused
In Reality:
- Base-and-bounds only for niche 
- Segmentation abandoned (High complexity for little gain) 
- **Paging is now universal**

