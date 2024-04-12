## 2001-present: Windows 5-11
- End goal was to unify Win NT and Win 9x.
- accomplished with Windows XP (2001) right after OS X 10.1
- The new OS was based on the Win NT architecture, and thus no longer relied on MS-DOS.

- Introduced with Windows 10, enables Linux binaries to be run by translating linux syscalls to Win NT syscalls 
- Windows is legendary for backwards compatibility (much worse on Mac)
- Existing market dominance since 1990s.

### U.S. v. Microsoft Corp. (2001)
Landmark antitrust case. Part of the "browser wars."
- US accused Microsoft of illegal monopoly on web browser market by bundling Internet Explorer (IE) with Windows.
- This allegedly restricted the market for competing web browsers.  
- Microsoft argued that a web browser was an essential component of an OS.
- Ruling: Microsoft's dominance of x86 PC market OS's was a monopoly, and Microsoft had used anticompetitive acts to maintain it, including bundling of IE.
- Microsoft was to be broken up into two companies: one for OS and one for other software. 
- IE attains 96% of web browser usage share in 2001.
- A decade later, IE would fall in rankings to Firefox and then Chrome.

### Microsoft Corp. v. Commission (2003-07)
Antitrust case brought by European Commission (EU). 
- Alleged that Microsoft had abused its dominant position in the desktop PC OS market by integrating Windows Media Player into Windows. 
- Microsoft argued that it provided a benefit to consumers, that they did not need to use it and did not need to pay extra.
- Ruling: WMP bundling gave Microsoft an unfair advantage over competing media players. Microsoft ordered to offer a version of Windows without WMP. 
- So they made Windows XP N (Windows XP Reduced Media Edition). 
- In 2009: EC announced it would investigate IE bundling. 
- So Microsoft released "Windows 7 E" in Europe (no IE). 
- And had to provide a selection screen for a default browser, in random order.

### Goldstein v. Microsoft Corp. (2016)
- Terri Goldstein (California) alleges an unauthorized Win 10 upgrade caused crashes and slowdowns on her PC, leading to big losses at her travel booking business. 
- Ruling found in favour of Goldstein and ordered Microsoft to pay $10,000 fine.

### Bill Gates
- Wrote his first software program at age 13
- Founded Microsoft in November 1976 (21 years old). Was an active software developer in early years, then manager.
- Stepped down as CEO of Microsoft in 2000, replaced by Ballmer, and then Satya Nadella in 2014. Stepped down as chairman in 2014.

## Test-and-set
- If the lock is 0 and a thread calls TestAndSet, it will set the lock to 1 and return 0. Since it returns 0, the thread will know it has acquired the lock.
- If the lock is 1 and a thread calls TestAndSet, it will set the lock to 1 and return 1. Since it returns 1, the thread will know it has not acquired the lock and will spin.
- By making the testing and setting of the flag atomic, only one thread can acquire the lock. Thus, we have enforced ==mutual exclusion. ==
- Known as **spin locks**, since they use a while loop to spin and waste CPU cycles. 
  A thread may spin forever
  bad on single CPU, not so bag on multiple (only one cpu will be spinning)

### Pthread locks
The POSIX library uses the term **mutex** for lock 