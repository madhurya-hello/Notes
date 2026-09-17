## RAM vs. ROM

### A. RAM (Random Access Memory)
- data disappears as soon as power turns off
- **SRAM (Static RAM):** 
    - uses flip-flops 
    - requires no refreshing
    - super fast and expensive
    - used in CPU Cache (L1, L2, L3)
- **DRAM (Dynamic RAM):**
    - uses capacitors that lose charge over time 
    - requires periodic refreshing
    - slower and cheaper
    - used as Main Memory

### B. ROM (Read-Only Memory)
- retains data without power
- holds the bootstrap code to start the computer
- **Evolution:**
    - **PROM:** programmable once
    - **EPROM:** erasable using UV light
    - **EEPROM:** erasable electronically (modern SSDs)

## Memory Allocation Techniques
### A. Single Contiguous Allocation:
- RAM is split into two regions: 
    - one for the OS 
    - one for a single user process
### B. Fixed Partitioning:
- RAM is pre-divided into fixed-size blocks
- **Internal Fragmentation:** 
    - wasted space inside an allocated block 
    - happens when process size < block size
### C. Variable Partitioning:
- RAM partitions are created on the fly to match the exact size of an incoming process
- **External Fragmentation:** 
    - over time, free memory gets broken up into small holes
    - although total free memory is enough, no single continuous gap is big enough to fit a new program
```text
┌──────────────────────────────────────────────────────────────────────────┐
│                   INTERNAL vs. EXTERNAL FRAGMENTATION                    │
│                                                                          │
│  INTERNAL FRAGMENTATION:                                                 │
│  [ Process (5 MB) | WASTED SPACE (10 MB) ] ──► (In a 15 MB Block)        │
│                                                                          │
│  EXTERNAL FRAGMENTATION:                                                 │
│  [ Free 2MB ] [ Process ] [ Free 3MB ] [ Process ] [ Free 2MB ]          │
│  Total Free = 7MB, but a 6MB program CANNOT fit contiguously!            │
└──────────────────────────────────────────────────────────────────────────┘
```

## Placement Algorithms
### 1. First Fit 
- picks the very first hole that is big enough
### 2. Best Fit 
- searches all memory and picks the smallest hole that fits
### 3. Worst Fit
- picks the largest available hole
### 4. Next Fit
- works like First Fit, but starts searching from the location of the last allocation

## Logical vs. Physical Address
### A. The Core Concept
```text
┌────────────────────────────────────────────────────────────────────────┐
│                        THE MOVIE TICKET ANALOGY                        │
│                                                                        │
│  Logical Address   ──► Ticket Seat Number (e.g., Row 5, Seat 10)       │
│  Physical Address  ──► Exact GPS Coordinates of that seat              │
│  MMU (Usher)       ──► Translates ticket seat to physical position     │
└────────────────────────────────────────────────────────────────────────┘
```
### B. Address Translation Handshake
```text
┌─────────────────────────────────────────────────────────────────────────┐
│                        ADDRESS TRANSLATION FLOW                         │
│                                                                         │
│  [ CPU ] ───(Logical Addr 100)────► [ MMU ]                             │
│                                        │                                │
│                              (Looks up Page Table)                      │
│                                        │                                │
│                                        ▼                                │
│  [ RAM ] ◄──(Physical Addr 5000)───────┘                                │
│     │                                                                   │
│     └───────────(Returns Data to CPU)────────────► [ CPU ]              │
└─────────────────────────────────────────────────────────────────────────┘
```