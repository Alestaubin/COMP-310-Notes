## Condition variables
- A thread may want to check some condition before continuing
- E.g. when a parent thread wants to wait until a child thread has finished executing.
- It does so by calling join

**Implementation**
To wait for a condition to be true, threads can use a **Condition Variable**
1. call `wait()`.
	This is a queue that threads can put themselves on when some condition is not true. 
2. A waiting thread will go to **sleep** (and thus not waste CPU time).
3. `signal()` is called by another thread when the condition is true
4. then the (first) waiting thread can wake up and continue.

### Problems 
- if child runs and exits immediately, before parent calls `wait()`, parent will be stuck forever. 
  **Solution**: flag variable initialized to 0. When thread exits, set to 1 before signalling. Other thread should only wait for signal if flag is 0 (otherwise, it knows other thread is already done).
- The condition variable is a shared resource between threads, so there is a possible **race condition**
  **Solution**: Make exit/join functions atomic.
- It a thread calls wait, it'll sleep while holding the lock
- `wait()` might return even though the condition variable has not actually been signalled (due to OS signal)

###  Pthread 
- `pthread_cond_init(pthread_cond_t, *cv, pthread_condattr_t *cattr)` 
  Initialize the conditional variable, cattr can be NULL 
- `pthread_cond_wait(pthread_cond_t *cv, pthread_mutex_t *mutex)`
  Block thread until condition is true, and atomically unblock mutex. 
- `pthread_cond_signal(pthread_cond_t *cv)` 
  Unblock one thread at random that is blocked by the condition variable 
- `pthread_cond_broadcast(pthread_cond_t *cv)`
  Unblock all threads that are blocked on the condition variable pointed to by cv.

### Producer/consumer problem
Imagine there is a **producer thread** and one or more **consumer threads**
- producer thread produces data items, places them into buffer
- consumer threads can consume the data items from buffer

- Buffer is a shared resource between threads, must implement synchronized access to avoid race conditions. 

**Solution:**
- Use locks to make buffer accesses atomic
- Use two separate condition variables. 
  - One to signal if the buffer is empty, and 
  - one to signal if the buffer is full. 
- Producer thread will wait on the empty condition, and consumer threads will wait on the full condition. Thus, a consumer thread can never signal another consumer thread, and a producer can never signal another producer.
