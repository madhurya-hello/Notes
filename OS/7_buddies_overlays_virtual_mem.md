## Buddy System
### A. Core Concept
- a memory allocation technique that divides memory into blocks
- sizes of the blocks are always powers of two ((e.g., 2, 4, 8, 16 KB)
- when a large block is split into two equal halves, those two halves are called **Buddies**
### B. Analogy
- imagine a 128-gram chocolate bar. 
- if someone asks for 20 grams, we cannot just snap off 20 grams
- we snap the 128g bar in half into two 64g buddies
- then we snap one 64g buddy into two 32g buddies
- then we give them one 32g piece
- if they return it later and the other 32g buddy is also free, we stick them back together into a 64g piece
### C. Diagram
```text
┌───────────────────────────────────────────────────────────────────────┐
│                         Total Memory: 128 KB                          │
├───────────────────────────────────┬───────────────────────────────────┤
│          Buddy 1 (64 KB)          │          Buddy 2 (64 KB)          │
├─────────────────┬─────────────────┼───────────────────────────────────┤
│ Buddy 1a (32KB) │ Buddy 1b (32KB) │          Buddy 2 (64 KB)          │
│ [ALLOCATED 20K] │  [FREE BUDDY]   │           [FREE BUDDY]            │
│  (12KB Waste)   │                 │                                   │
└─────────────────┴─────────────────┴───────────────────────────────────┘
```
### D. Types of Buddy Systems
- **Binary Buddy:** standard version using powers of two (e.g., 2, 4, 8, 16, ...)
- **Fibonacci Buddy:** blocks sized according to Fibonacci numbers (e.g., 1, 2, 3, 5, ...)
### E. Points to Remember
- two free blocks of the same size can **only** merge if they are **true buddies** created from the exact same split
- reduces **external fragmentation**, but suffers heavily from **internal fragmentation**

## Overlays
### A. Core Concept
- allow a program to run even if it is larger than physical RAM
- instead of the entire program, only the active module is loaded into RAM
### B Overlay Execution Tree
```text
  ┌─────────────────────────┐
  │       Root (2 KB)       │ ◄── Stays in RAM constantly
  └─────────────────────────┘
       /           │        \
      ▼            ▼         ▼
   A (4KB)      B (6KB)   C (8KB)
    /   \          │         │
   ▼     ▼         ▼         ▼
  D (6KB) E (8KB) F (2KB)  G (4KB)
```
```text
┌────────────────────────────────────────────────────────────────────┐
│             Root / Common Module (Always in RAM: 2 KB)             │
├────────────────────────────────────────────────────────────────────┤
│                    Shared Overlay Memory Region                    │
│  [Path 1: Module A (4KB) + Module D (6KB) ──► 10 KB]               │
│  [Path 2: Module A (4KB) + Module E (8KB) ──► 12 KB]  *MAX PATH    │
│  [Path 3: Module B (6KB) + Module F (2KB) ──►  8 KB]               │
│  [Path 4: Module C (8KB) + Module G (4KB) ──► 12 KB]               │
├────────────────────────────────────────────────────────────────────┤
│        Minimum Physical RAM Required = 2 KB + 12 KB = 14 KB        │
└────────────────────────────────────────────────────────────────────┘
```
### C. Why We Don't Use Overlays Today
- immense manual tracking burden on developers
- hence it is now replaced with **Virtual Memory**

## Virtual Memory
### A. Core Concept
- gives programmers the illusion of a massive, continuous block of memory
- active parts of a program are kept in physical RAM
- inactive parts stay in a **Page File** on the SSD
### B. Virtual Memory to Physical Memory Mapping
```text
┌───────────────────────────┐        MMU Page Table        ┌──────────────────────┐
│ Virtual Address Space     │ ───────────────────────────► │ Physical RAM         │
│ (Program View)            │     [Page 0] ──► Frame 2     │ [Frame 0]            │
│  - Page 0 (Active)        │     [Page 1] ──► Disk        │ [Frame 1]            │
│  - Page 1 (Inactive)      │     [Page 2] ──► Frame 0     │ [Frame 2] Page 0     │
│  - Page 2 (Active)        │                              └──────────┬───────────┘
└─────────────┬─────────────┘                                         │
              │                                              Page Fault (Page 1)
              │                                                       │
              └──────────────────────────────────────────────────────►┤
                                                                      ▼
                                                           ┌──────────────────────┐
                                                           │ Swap File (Disk)     │
                                                           │ [Page 1 Stored]      │
                                                           └──────────────────────┘
```
### C. Key Terms
- **Paging:** 
  - divides a program into fixed-size virtual blocks called **Pages**
  - and physical RAM into matching blocks called **Frames**
- **Page Fault:** 
  - occurs when the CPU requests a page that is not currently loaded in physical RAM
  - forces the OS to fetch it from disk
- **Demand Paging:** 
  - loading pages into RAM **only when requested** by the CPU
- **Thrashing:** 
  - a state where the Page Fault rate is very high 
  - the OS spends all its time swapping data back and forth to the disk