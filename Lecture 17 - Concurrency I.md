- **Concurrency** is the practice of running multiple tasks at once. The reason is that CPUs have multiple cores and we must take advantage of them.
- **Parallelism**: use each CPU core to do part of the work of a big task.
- **Efficiency**: to avoid program slowdowns from slow I/O. Enables overlap of I/O with other activities in a program.
#### Option 1: Processes
- Build apps from many communicating **Processes**
- they communicate through message passing (no shared memory)
- Pro: if one process crashes, the others are not affected
- Cons: High communication overhead (slow), expensive context switching

#### Option 2: Threads
- Create multiple threads in the same process 
- **Threads** are like processes, except they share an address space
- they communicate through variables in the address space
- if one thread crashes, the whole process crashes
- more efficient than having multiple processes

#### Library for Threads
```c
void *mythread(void *arg) {
	myarg_t *args = (myarg_t *) arg;
	printf("%d %d\n", args->a, args->b);
	return NULL;
}
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void*), void *arg);
```
where thread must be created beforehand, attr: null in most cases, start_routine: the function to be run in the thread, arg: to be passed to the function.
```c
int pthread_join(pthread_t thread, void **value_ptr);
```
where thread: which thread to wait for, value_ptr: to store the thread's return value.

### Multiple threads 
**Note:** Main thread creates (forks) collection of sub-threads passing them args to work on and then joins with them, collecting the results
- When a thread is created, it may start running right away, or not (depends on the scheduler)
- many different possibilities for execution of program

#### Threads with shared data 
- May produce different outputs
- Not *deterministic*

**Data Race**: the unexpected/unwanted access to shared data within threads.

**Race condition**: when the results depend on the timing of the code's execution. It occurs when there is a data race.

#### Solution
1. Divide work among multiple threads
2. share data
   - only the Global variables and the heap
   - not local 
   - shared data is accessed in **Critical section**

- **Critical section** is a section of the program that we want to be atomic. That is, it must be executed as a single block all at once. Need **Mutual Exclusion** for critical regions. 
- **Mutual Exclusion** prevents simultaneous access to a shared resource (shared memory region)

### Locks 
- A lock is a function that lets us execute a critical section atomically. 
- Making sure that only one thread executes the critical section at a time.

Calling `lock()` 
 tries to acquire the lock.
1. If no other thread currently holds the lock, thread will acquire the lock and will enter the critical section.
2. If another thread is already holding the lock, it will block until it can acquire the lock.

Goals of a lock:
- Mutual exclusion
- Fairness (each thread should get a fair shot at acquiring the lock)
- Performance (time overheads)

### Implementation: 
#### 1. Control interrupts
- disable interrupts for critical sections 
- simple, but this is a privileged operation that cannot be given to programs 
- doesn't work with multiple CPUs
- disabling interrupts also stops I/O requests...

#### 2. loads/stores
Use a variable to indicate whether lock is held by a thread.

When entering critical section, test if lock is 1.
- If no, set lock to 1 and enter, then set lock to 0 when done.
- If yes, then wait until lock is 0, then set lock to 1 and enter.

**Disadvantages:**
- Doesn't enforce mutual exclusion, still have the same timer interrupt problem.
- bad performance: wasting CPU resources while *spin-waiting* for lock to unlock.

#### 3. Test-and-set
Same as loads/stores, but adding the support in hardware to test and set a variable's value **atomically** (solving the timer interrupt-problem)
- "test" the old value of lock and at the same time change it to a new value.
- In the x86 architecture, the `xchg` instruction provides the test-and-set functionality.
```c
int TestAndSet(int *old_ptr, int new  
	int old = *old_ptr; // fetch old value at old_ptr
	*old_ptr = new; // store 'new' into old_ptr 
	return old; // return the old value 
}
```

