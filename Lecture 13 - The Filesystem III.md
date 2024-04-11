## 1989-1995: NeXTSTEP
Why was it made?

- September 16, 1985: Steve Jobs resigns from Apple
- starts a new computer company: NeXT, hiring apple employees
- decided to make a computer workstation (designed for scientific applications. more powerful (and expensive) hardware than a PC)
- $6,500

What made it special?

- ran the NeXTSTEP operating system, a Unix-like OS.
- Based on the Mach kernel and BSD Unix.
- Dock and shelf, Better drag and drop, Real-time scrolling, window-dragging
- written in Objective C (OOP)
- sold to student and universities, 50,000 units sold

What happened to it?

- Acquired by Apple in 1997.
- Steve jobs becomes interim CEO

## In-memory Data Structures
#### Cache 
- Fixed contiguous area of kernel memory 
- Size = max number of cache blocks x block size 
- A large chunk of memory of the machine

**Cache directory**

- usually a hash table 
- index = hash(disk address)
- holds a dirty bit 

**Cache Replacement**

- Keep LRU list: easy to do (less accesses)
- If no more free entries in the cache: Replace “clean” block according to LRU, Replace “dirty” block according to LRU

**Cache Flush**

- find dirty entries, write back to disk

### File tables
1. (System-Wide) Active File Table
- One array for the entire system 
- One entry per _open file_
- Each entry contains 
- File inode Additional information • Reference count of number of file opens