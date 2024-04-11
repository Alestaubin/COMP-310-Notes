## 1965 - 1972: Multics OS
Definition: An **information utility**  would provide **information** to the community, through phone lines. People would have a terminal in their home, and connect to a computer mainframe to receive information.
why it was made? 
- As a successor to the CTSS.
- Designed to be a 24/7 computing utility. 
- Could continue running while additional processors, memory or disks were attached or removed.
- Designed with security in mind since multiple users would be connecting.
- It also featured innovations in memory management and filesystems to better accommodate multiple users.
what made it special?  
- first OS with hierarchical filesystem (nested subdirectories permitted).
- introduced the idea of Access Control Lists for each file (read/ write/execute).
- **ring structure** of access: Ring 0 had the highest level of privilege, and higher rings had lesser and lesser privileges.
what happened to it and why
- Today's architectures still use protection rings

## Problem: Paging Address Translation Performance
We need to check the page table first in the virtual memory, and then access the physical memory (reduces access time by a factor of 2).

#### Solution: a new cache in hardware
- Add a cache into the MMU known as the **translation-lookaside buffer** (**TLB**). 
TLB:
- Upon each virtual memory reference, check the TLB to see if the requested translation is there. If so, we can use it without consulting the page table.
2 cases: 
- TLB holds the translation for the VPN (**TLB hit**)
	we can use the TLB entry to get the page frame number.
- TLB does not hold the translation (**TLB miss**)
	the hardware accesses the page table to get the translation, then updates the TLB with the translation.
**TLB is faster than page table**
Uses **associative memory**. Look up by contents instead of by address (same as a `dict` struct in python, (key; value) ). 
The key is the page number and the value is the frame number.
- TLB is small (64- 1024 entries).
- Want close to 100% TLB hit rate, use temporal and spatial locality
Temporal 
	an instruction that was recently accessed will be reaccessed soon 
Spacial 
	if an address x was recently accessed, addresses near x will be soon accessed (e.g. array)
	So, if page number 1 gets requested, and theres a miss, add the page to the TLB because the next access will most likely also be in page 1 (each page contains more than one address)

## Process switching
When switching process, entries in the tlb are no longer valid, as they belong to the old process. 

1. **Solution 1:** When switching, invalidate all tlb entries (slow, new process will have 100% miss initially)
2. **Solution 2:** Add process identifier to tlb entries. So to get a match, need to match with process and page number. 
	Makes the TLB more complicated, but switching is cheap. 
	All modern machines use this strategy.

## Page table size
Consider a 64-bit address space with 4-KB pages and 52 bits for VPN. 
If a page has 4KB (4096 bytes), then in order to address 4096 bytes, we need 12 bits (since 2^12 = 4096) for the offset (the offset determines how far into the page the address is). 
That leaves us with 64 - 12 = 52 bits for the Virtual Page Number. So we may represent $2^{52}$ pages with this system. 
Each of the $2^{52}$ entries in the page table may take up 4 bytes (say), so that makes for a page table taking $2^{52} \times 4$ bytes of memory (super huge)
### Make page tables smaller: 
bigger page sizes
disadvantage: larger pages have higher internal fragmentation.
4KB-8KB is the usual size 