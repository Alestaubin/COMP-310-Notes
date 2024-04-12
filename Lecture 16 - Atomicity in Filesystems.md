## 2001-present: Mac OS X

Why was it made?
- When Jobs came back to Apple from NeXT, he killed the clones.
- Restructures Apple product line. Simple distinction between consumer and pro line.
- Dispute with Microsoft is settled
- iMac, iPod introduced (big success)
- online store (1997), retail stores (2001)

What made it special?

- Mac OS X was completely different code than original Mac OS
- Built on NeXTSTEP OS (Mac kernel / BSD Unix)
- New UI with features of both Mac OS + NeXTSTEP.
- new features that windows had, but Mac OS was lacking such as pre-emptive multitasking, memory protection, command line interface (Mac OS was one of the few without a shell)
- New UI: Aqua (Transparency, shadows, animation, anti-aliasing)

What happened to it?
- Mac users thrilled by first big new update (ever)
- first cost $129, then free

### APFS
- New filesystem
- introduced in 2017, succeeded HFS+
- Optimized for SSDs: Copy-on-write (less wear)
- Actually performs worse on mechanical HDs
- Prioritizes user I/O requests over background activity.
- Full disk encryption.
- Does not use journaling! Uses redirect-on-write approach (similar to shadow paging).
## Atomicity in Filesystems
- means "all or nothing"
- In a FS, this means either all updates are on disk, or none are (no in between)
- Assumption: a single sector write is atomic (this is true 99.999% of the time)
#### Snapshots
- A "snapshot" of a filesystem at a particular point in time.
- Later modifications to the filesystem are tracked, but do not overwrite the data saved in the snapshot.
- Good for backup and reliability purposes.
### Implement Atomicity
#### Approach 1: Shadow Paging
- make sure you have old copy on disk 
- make sure you have new copy on disk
- Switch atomically between the two 

**With Write-through**
1. `open()` read old inode into active file table (memory) 
2. `write()` in the *in-memory copy* of Inode, allocate new blocks for new data, fill in address of new blocks, write data blocks to cache and disk
3. `close()` overwrite *in-memory copy* of Inode to disk Inode

**With Write-behind**
1. `open()` read old inode into active file table (memory) 
2. `write()` in the *in-memory copy* of Inode, allocate new blocks for new data, fill in address of new blocks, write data blocks to cache, but NOT to disk yet.
3. `close()` write all cached blocks to new disk blocks, overwrite *in-memory copy* of Inode to disk 

**What happens to old blocks?** de-allocate them

**downsides of shadow paging**
- disk allocation gets messed up
- fragmentation

**Advantages**
- less writes than intentions log
- crash recovery much simpler than intentions log
- enables snapshots + cloning
#### Approach 2: Intentions Log
- reserve an area of disk for intentions log

on `write()`:
  1. write to cache
  2. write to log (on disk)
  3. make *in-memory inode* point to update in log

on `close()`:
1. write old and new inode to log in one disk write
2. copy updates from log to original disk locations 
3. when all updates done, overwrite inode with new value (Later in background)
4. remove updates and old and new inodes from log (Later in background)

**Crash recovery**
- search forward through log
- for each new inode found:
  1. find and copy updates to their original location 
  2. write new inode
  3. remove updates, old inode, and new inode from log

**On crash:**
- If new inode is in the log: rewrite new copy
- If new inode is not in the log: write old copy

**Advantages:**
- writes to log are sequential (no seek)
- data blocks stay in place 
- good disk allocation is preserved (unlike shadow paging)
- can write from cache or log to the disk in the background, when disk is at idle
#### Comparing FS methods
1. count the number of disk I/O 
2. count the number of random disk I/O

Technique 1: Shadow Paging
- **two** disk writes: one for data block, one for inode.

Technique 2: Intentions Log
- **four** disk writes: two for data block, two for inode
