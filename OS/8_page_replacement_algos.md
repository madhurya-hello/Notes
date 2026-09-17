## Why Page Replacement?
- when the RAM becomes completely full and a Page Fault occur, the OS must kick out an existing page to make room for the new one
- **Dirty Bit :**
    - `Dirty Bit = 1` 
        - the page was edited
        - the OS must write it back to the disk before deleting it from RAM
    - `Dirty Bit = 0`
        - the page was not changed
        - the OS can instantly overwrite it

## The 4 Page Replacement Algorithms
### A. First-In, First-Out (FIFO)
- the oldest page currently in RAM is the first one kicked out
- might kick out a heavily used page simply because it was the first one to arrive
- **Belady’s Anomaly:** Counterintuitively, giving the system more RAM frames can sometimes result in more page faults
### B. Least Recently Used (LRU)
- replaces the page that hasn't been touched for the longest time
- most widely used algorithm in real OSs
- **does not suffer** from Belady's Anomaly
### C. Most Recently Used (MRU)
- replaces the page that was accessed most recently
- used in database scanning scenarios where recently read data won't be re-read right away
- **can suffer** from Belady's Anomaly
### D. Optimal Page Replacement (OPT)
- replaces the page that will not be used for the longest period of time in the future
- usually used as a benchmark to calculate how optimized other algorithms are
