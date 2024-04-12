## 90s Mac OS
 - 1991: System 7. Updated UI. Virtual memory support. 32-bit memory addressing.
- In 1995, Apple decided to license the Mac OS (System 7) to other companies to put Mac OS on their own computers ("clones").
- But the clones started to eat into Apple's own profits...
- 1997: Mac OS 8. Multi-threaded Finder. Updated UI
- Mac OS was stagnating compared to Windows 95/98/NT.
- Apple needed an OS that could compete, one with preemptive multitasking, networking, and other features found in Windows.
- Scrapped the Mac OS and bought another company's OS
- 1997, Apple was 90 days away from bankruptcy

## Disk cleaning 
- With log-structured filesystem, no sector of the disk is ever overwritten. File data is always appended to the next free sector, and no sector is ever put on the free list.
- So disk will get full (quickly), need to clean it 
- Reclaim “old” data, i.e. data that was logically overwritten, but not physically overwritten

**Strategy:**
- Go through each segment
- write the data into a buffer (only the valid data, not the old)
- when buffer is full, write it as a new segment 
- mark the cleaned segment as free

### Summary of LFS 
- reads mostly from cache
- writes to disk are optimized (very few seeks)
- read from disk bit more expensive (not contiguous)
- cost of cleaning is heavy
- Has not become mainstream yet 

## File Recovery
#### Bad sectors
- sectors are often lost due to normal usage
- Disk firmware can detect a bad sector if it tries to read from that sector and fails. 
- It will then mark that sector as bad, and remap its sector number to a different spare sector that was kept free for this exact purpose

#### Recovering deleted files 
- when a file is deleted, it has no more links and its inode and data blocks are marked as free in the bitmap.
- But, data is not immediately garbage collected, so it may be recoverable

**Recovering the inode:**
- Then can check the pointers to allocated data blocks

**Recovering the data blocks:**
- If inode is not recoverable, must scan the data blocks marked as free. 
- This is known as ==File Carving==. File carvers search a disk for occurrences of particular headers and footers that are typical to certain files.
- Can be done even if filesystem metadata has been completely corrupted
- But, files must be stored in contiguous data blocks 
- Certain filesystems do try to allocate contiguously as much as possible, to avoid disk fragmentation.
- But not newer SSDs (since fragmentation is not an issue, not seek time)

### Crashes
- If a crash happens in the middle of an update, on-disk structure may be left in an inconsistent state.
- This is known as the **Crash Consistency Problem**. 
- What we'd like is to update file system state **atomically**
#### Solution 1: fsck
- Earlier file systems let these inconsistencies occur, and then repaired them later.
- With `fsck`, which finds inconsistencies and repairs them
- `fsck` is run before FS is mounted again. 
- does sanity checks on the **superblock**
- scans all inodes, indirect blocks, doubly indirect blocks to fin out which are free, then compares with bitmaps. 
- if any inconsistency, trusts info on inodes
- It checks each **inode** for corruption. E.g., if the type fields are valid
- It checks the **link count** of each allocated inode, verifies this count by scanning the entire directory tree to build its own link count.
- It checks for **bad block pointers**, i.e. if it points to memory out of its range.

**fsck is VERY slow**
- also irrational, it is very expensive to scan the entire disk to fix a small problem.

