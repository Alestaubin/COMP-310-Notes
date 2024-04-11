## 1969-75: Unix
why it was made? 
- Multics was costly, not optimal and could only run on the very expensive GE-645 (7.78 million)
- Ken Thompson (Bell Labs) learned a neighbouring department had an unused PDP-7. He took it and started coding a new OS
- Unix v1.0 was released in 1971.
- At the same time that they were working on Unix, Thompson and Ritchie developed the **B programming language**. By 1972, it was renamed to **C**.
what made it special?  
- In 1973, Unix v4 was released. Its kernel was rewritten in C, one of the first OS kernels to be written in a high-level language. 
- The migration to C made Unix a **portable** OS.
what happened to it and why?
- Because of the continuing development, portability and low licensing cost, Unix became increasingly popular.
- New versions continued to be released in the 1980s, including versions for **microcomputers** (small inexpensive for hobbyists) .

## Page table sizes part 2
### segmentation + paging
- Divide address space into segments (code, heap, stack)
- Segments can be variable length 
- Divide each segment into fixed-sized pages 
- Virtual address divided into three portions: seg (4bits), page no (8bits), page offset (12 bits).
implementation: 
- Each segment has a page table 
- Each segment track base (physical address) and bounds of **page table** for that segment
### Steps for accessing a page
![[Pasted image 20240409203810.png]]
1. read the seg number (4 bits)
2. access the seg in the seg table
3. check if page number requested (blue) is within bounds of the bounds column of the seg table (this number corresponds to the number of elements in the page table)
4. read the page number requested from the base address + page number.
5. return the address of the page offset by adding the address of the page table + page offset.
Advantages of segmentation + paging: 
- unallocated pages between stack and heap no longer take space 
- no external fragmentation
### Multi-level page table
**2-level Page Table:** 
- Chop up the page table into page-sized units. 
- If an entire page of page-table entries is invalid, don’t allocate that page of the page table at all. 
- To track if a page of page table is valid, use a **page directory** (new structure).
Virtual address: 
![[Pasted image 20240409211956.png]]
#### Advantages 
- for sparse address spaces (otherwise useless)
- One-level page table must address the whole address space 
- Two-level page table only need whole address space for top level page table, not second level
#### More levels
- If address space is too large (e.g. 64 bits instructions), can add more levels. 
- more memory accesses: N-level page table means N + 1 memory accesses (N for the page tables, 1 for the physical memory)
- If TLB has good hit rate, then 1 memory access most of the time.
