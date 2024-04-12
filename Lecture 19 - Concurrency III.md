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