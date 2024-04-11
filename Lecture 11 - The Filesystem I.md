## 1985: Windows

why was it made?

- bill gates goes to a computer show, sees GUI, thinks it is the future of computing.
- Microsoft hires PARC researchers to work on their GUI

what made it special?

- it was designed to work on top of the MS-DOS OS. 
- Windows was simply an executable that ran on top of the OS, providing a GUI.
- cost $99 usd
- Microsoft only sold OS, not a whole machine (unlike apple). And the OS was compatible with many different computers/brands.
- more primitive than the mac GUI

what happened to it and why?

- apple complains that windows is copying their data, but Microsoft threatens to stop developing Word for Mac, a very popular product.
- Windows 2 released in 1987 (2 years after version 1)
- 1988: Apple files lawsuit, accusing of copyright violation. Apple loses.
- GUI becomes open source.
- Windows 3: Released in May 1990. Total overhaul of GUI. Emphasis on icons for files, instead of simple list. Improved multitasking, customizability of appearance (desktop picture, etc.). Better memory use. Huge success.

## File API
#### The File 
- a linear array of bytes, has a low level name, ==inode number==
- A file is created by calling the `open()` system call and passing the `O_CREAT` flag, returns a **file descriptor**: an integer for that process that you can use to read/write to the file.
- Each running process already has three files open at the start: standard input (0), standard output (1) and standard error (2).
- `read()`, and `write()`
- Each open file has a current offset which is updated at each read/write, or when using `lseek`
- `lseek(int fd, off_t offset, int whence)` changes the current offset with `whence`, depending on `offset`:
	- SEEK_SET: use offset as given 
	- SEEK_CUR: use current location + offset 
	- SEEK_END: use file size + offset
- **File Struct**
```c
struct file {
	int ref; // reference count - TBD 
	char is_readable; 
	char is_writable; 
	struct inode inode_ptr; 
	uint current_offset; 
};
```
- **Open File table**
	An array containing the open files' file structs 
- **The Directory**
	Also has inode number, contains list of files in format `(filename, inode)`

### Links 

**Hard Links**

- `link()` is a system call taking as argument an `old_path` and a `new_path`. Creates the same file as the one in `old_path` into the `new_path`.
- The two files will be exactly the same. Not copies. They will have the same inode.
- link is made:`ref_count++;` in the inode struct 
- file is unlinked: file is removed from its directory struct, `ref_count--;`
- when `ref_count == 0;` file is deleted.
- hard links can be made with `ln` command.
- hard links can't be made to directories 

**Symbolic Links**

- made using `ln -s`
- A symbolic link is a file itself with its own inode. The contents of the file is a **pathname** to the linked-to file 
- if the linked-to file is deleted, symbolic link will remain, and become an invalid reference

**Mounting a filesystem**

- `mkfs`: a command-line tool that takes a device as input (e.g., /dev/sda1) and a filesystem type (e.g., ext3) and writes an empty filesystem (with root directory) to the disk. 
- `mount`: takes a device with a filesystem, and makes it available at a given directory. 
	 `mount -t ext3 /dev/sda1 /home/users`
## Implementing filesystem
Main role of filesystem is to translate from user interface functions (such as `read()`) to disk interface functions (e.g. `ReadSector()`).

### On disk data structures
1. **Data Region**  occupies most space in FS (User data, Free space)
2. **Metadata** (Inodes, bitmap, Superblock)

#### The Inodes
Inodes contain: 
- type of file (regular, directory, symbolic link)
- size (bytes)
- number of blocks allocated to it 
- owner + permissions
- Time info (created, modified)
- pointer to data blocks on disk
The inode number
- low-level name of file
- given inode number, can determine exactly where on file the inode is `start of inode table + inode number * sizeof(inode)`
#### Freespace tracking 
Bitmaps: each bit indicates if corresponding obect is in use (1 = in use, 0 = free)
we use one bitmap for tracking the data and one for the inodes
#### Superblock 
contains filesystem data 
- number of inodes
- number of data blocks
- start of inode tables, etc