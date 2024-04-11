## OS History :ENIAC
first programable digital computer
designed for the US ballistic research lab
Program was input by flipping switches and connecting cables.
No OS, no memory.
ENIAC filled an entire room, weighed 30 tons, and consumed 200 kilowatts of power
# Process
program = executable code (file on disk)
process = executing the code (in memory)

- Process is an execution stream in a process state.
- Execution stream is a stream of executing instruction (the running code)
- Process state is everything surrounding the process, such as the registers, the stack, heap, code, open files, address space, etc.

### Process creation
1. load code in memory
2. allocate memory for the stack (local vars, return addresses, etc.), and heap (dynamically allocated memory).
3. set in/out file descriptors to let program read input from terminal and print output
4. start program at entry point (`main`) by pointing CPU PC to it.

### Direct execution #execution
process runs until it's done, has full control of the cpu, can do anything. Can be dangerous for the computer, e.g. corrupt files, crash OS.
Letting process perform sensitive operations (write, read to disk) directly would prevent important checks. 
A process with direct control could ignore file system permissions.

### User mode and kernel mode
The kernel is a subset of OS with special rights. Has full access to hardware. All other apps have limited access. 
1. User mode: Limited access
	Code has restrictions, e.g. cannot directly issue i/o request (process would get killed)
	No access privileges
2. Kernel mode: Privileged Instructions
	Os runs the code, can do anything (access to memory, devices)

System calls (`syscall`):
	 Special function call that goes into OS code and runs on the CPU in **kernel mode**.
	 POSIX API is a standard set of syscalls that most modern OS's implement.![[Pasted image 20240405100022.png]]
	e.g. `printf` calls the syscall `write`

Process APIs 
syscalls that involve process management
![[Pasted image 20240405101041.png]]

### Basic syscalls on processes 
1. `fork` create a new `child` process
	- creates an (almost) exact copy of the calling process, which starts running at the line directly after the **fork** call. It is called the **child process**, while the initial process (which is still running) is called the **parent**.
	- fork() returns 0 to the child, while it returns the PID of the child process to the parent.
	- It is not clear whether the child or parent will run before (i.e. not `deterministic`)
	- In Unix-like OS's, **every process is created by forking from another process!**
	- Child inherits parent's environment (it is cloned with same variables)
	- if variables are changed in child, doesn't affect parent.
1. `wait` lets the parent wait until the child process has finished executing.
	- makes the fork() deterministic.
	- will not return until the child has run and exited
	- A more general system call `waitpid` waits for a child process with a specific PID to end.
2. `exec` lets us run a program different from the current (calling) program. 
	- **transforms** the current process into the new one: it overwrites the current instructions in memory with the new program's instructions (after loading them from disk), and re-initializes the heap and stack and other parts of the program's memory space.
	- `exec` never returns! Since the current program's instructions will not continue to run.
	- the new process starts in a completely new environment

When you type a command into the shell, 
1. calls **fork** to make a new child process,
2. **exec** to make the child run the command,
3. and **wait** to wait until child completes before printing new prompt.