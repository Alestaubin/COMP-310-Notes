## OS history 
- Tape systems: could read up to 15,000 characters per second, 45 times faster than punch card readers.
- The ENIAC, IBM 701 necessitated an **operator**. The operator was the person who would take the punch cards, e.g., and place them in the card reader, start the program and supervise the use of the computer's resources, deal with malfunctions or bad commands.
1955: The Mock 701 Monitor  
- Made by Owen Mock at NAA for the IBM 701. 
- It could switch between jobs in a batch automatically, and handle basic I/O operations, without the need for an operator.
- Known as a **resident monitor** ("resident" since it was always in memory).
1956: GM-NAA I/O 
- made for the IBM 704. 
- Based on the 701 Monitor
- More sophisticated, with better I/O control/automation. 
- Considered to be the **first Operating System!**

## Memory allocation
Early days: Uniprogramming.
- One process runs at a time
- process directly sees physical memory

#### Address space 
OS provides an abstraction of physical memory to each process, called the **address space**, consists of:
- the code (instructions), 
- the stack (local vars, params, return vals), and 
- the heap (dynamically allocated memory).
The address space for each process starts at address 0. But the addresses in this space are **virtual addresses**, not real addresses into physical memory. 

**Virtual/logical** address space = What the program thinks is its memory **Physical** address space = actual physical memory

Goals of address space
**Transparency**:  
- Processes should not need to know that memory is shared
- programmer should not worry about where in physical memory the program is.
**Protection**: 
- Privacy: Should not be able to read/corrupt data of other processes 
- **Must check** every access from the CPU to memory (return error if process 1 tries to access process 2 code)
**Efficiency**: in terms of time and memory space.

#### Memory Management Unit (MMU) 
Hardware that provides **mapping** virtual-to-physical 

### Stack and Heap 
two types of dynamic memory allocation 
1. Stack 
	- OS uses stack for procedure call frames (local variables and parameters)
	- first in last out 
	- Pointer separates allocated and freed space 
	- Allocate: Increment pointer 
	- Free: Decrement pointer 
	- No **fragmentation**. 
2. Heap
	- Allocate from any random location 
	- consists of allocated areas and free areas (holes) 
	- Order of allocation and free is unpredictable
	- Programmers manage allocations/deallocations **malloc/free**
	- slower than stack
	- fragmentation 

The **memory leak**. 
	Slowly leaking memory causes one to run out of memory! 
	_Note: In short programs, free is not needed, since the OS will reclaim all memory of the process when the process exits, even non-freed memory._

- `malloc` and `free` are library calls, not system calls, since they manage space within the process' address space. But they are implemented using system calls.
- `calloc` allocates memory and zeroes it before returning.
- free'ing twice: Unspecified behaviour, but possibly bad things. For example: after freeing once, the same memory may have been allocated later by another malloc call. So freeing a second time would free that memory, causing a dangling pointer somewhere else