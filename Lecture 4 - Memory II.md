### OS History
Recap 

In 1959: 
- a system whereby a programmer could debug a job while another job was running on the same computer. Debugging would take place at an **auxiliary terminal** connected to the computer, such as a teletype machine.
- implementation of a special interrupt into the CPU (of the IBM 7094), called a **clock interrupt** to allow a computer's **processing time to be shared** amongst multiple users.
### CTSS
why it was made? 
	for programers to simultaneously work on the same computer.
	Designed for IBM 7090/7094.
what made it special?  
	**First** general purpose **time-sharing** OS. 
		 Supported four terminals. A clock interrupt would be triggered every 0.2 seconds. At each clock interrupt, the OS regained control and could assign the CPU to another user's job.
	**First** file system
		Each user was given their own tape to store their files. The tapes were stored in the computer room. 
	**First** invention of a computer password.
		To access the files on their tape, they were given an **account** with a **password** to login with. 
	**First** text commands 
- what happened to it and why
	 people began to develop tools to operate shared files: to generate program source, to use data in multiple ways, to create new things from old. 
	 People found ways to share information through the file system. This started with “common file directories” and led to electronic mail.

## Address translation 
Virtual addresses are passed through the MMU and translated  to physical addresses. This occurs during an instruction fetch, load or store.
## Mapping Schemes
### Base and bounds
Two registers are kept in the MMU: 
- **Base register:** stores where in physical memory the address space of the process should start. 
- **Bounds register:** stores the size of the process' address space.
- the MMU translates `virtural address` to a `physical address` by incrementing it by the **base** address.
- MMU checks that any memory reference is within bounds by comparing it to the bounds register. If not, generates a trap.
**Virtual Address Space**
	Linear address space from `0` to `MAX` 
**Physical Address Space** 
	Linear address space from `BASE` to `BOUNDS=BASE+MAX`X

- `base` and `bounds` can only be changed in kernel mode.
- Need to choose a large enough continuous hole in the main memory to fit the address space. Not ideal, because such a hole may not always exist. 
### Segmentation 
- Instead of a single base/bounds pair per process, we will have a base/bounds pair per **segment** of the address space.
- can place each segment in different places of memory
- try to refer to an address outside the bounds of a segment: **segmentation fault**!
Pros: 
- Unlike base and bounds, avoids assigning an entire address space of code, heap, stack and unused virtual memory to a process. 
- Instead, heap and stack can grow into unused physical memory.  Minimal overhead.

**Virtual memory - Segmentation**
![[Pasted image 20240408092623.png]]

