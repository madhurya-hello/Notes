## The Critical Section
```text
┌────────────────────────────────────────────────────────────────────────────┐
│                      CRITICAL SECTION CODE STRUCTURE                       │
│                                                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │ 1. ENTRY SECTION      ──► wait(semaphore);                           │  │
│  ├──────────────────────────────────────────────────────────────────────┤  │
│  │ 2. CRITICAL SECTION   ──► balance = balance - withdrawal;            │  │
│  ├──────────────────────────────────────────────────────────────────────┤  │
│  │ 3. EXIT SECTION       ──► signal(semaphore);                         │  │
│  ├──────────────────────────────────────────────────────────────────────┤  │
│  │ 4. REMAINDER SECTION  ──► // Unshared local operations               │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                                                            │
└────────────────────────────────────────────────────────────────────────────┘
```
### A. The Race Condition
- multiple processes access and modify shared data concurrently, causing the final output to depend on the exact order of execution
- **Example**
    - Process A reads the balance ($500) and calculates $500 - $100 = $400
    - Process B reads the balance ($500) before A writes, and calculates $500 - $100 = $400
    - Process A writes $400 to memory
    - Process B writes $400 to memory
    - **Result**: The final balance is $400, even though $200 total was withdrawn

### B. The 3 Golden Rules 
to solve the Critical Section problem, any solution must satisfy these 3 requirements
- **1. Mutual Exclusion**: 
    - if `Process A` is executing in its Critical Section, no other process can enter its Critical Section
- **2. Progress**: 
    - if no process is in its Critical Section, and `Process A` and `Process B` wants to enter, the decision of who enters next cannot be postponed indefinitely
- **3. Bounded Waiting**: 
    - if `Process A` requests access to Critical Section but `Process B` and `Process C` repeatedly take turns entering the Critical Section, then `Process A` will be ignored indefinitely and suffer from starvation

### C. Basic Synchronization Tools
- **Mutex (Mutual Exclusion)**: 
    - a process grabs the lock to enter the room and must release it when leaving
    - only the lock owner can unlock it
- **Binary Semaphore**:
    - similar to a mutex 
    - value is 0 or 1
- **Counting Semaphore**: 
    - manages finite resource instances 
    - e.g. a room with 5 chairs 
    - it counts down as processes enter and up as they leave
- **Peterson's Solution**: 
    - a classic software solution using `flag` and `turn` variables
    - its works only for two processes


## Solutions to Synchronization Problems
```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│                    EVOLUTION OF SYNCHRONIZATION METHODS                     │
│                                                                             │
│  ┌───────────────────┐     ┌───────────────────┐     ┌───────────────────┐  │
│  │ Level 1: Hardware │ ──► │ Level 2: Software │ ──► │    Level 3: OS    │  │
│  │ Interrupt Disable │     │    & Spinlocks    │     │ Mutex/Semaphores  │  │
│  └───────────────────┘     └───────────────────┘     └───────────────────┘  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```
### Level 1: Interrupt Disabling
- the process turns off hardware interrupts before entering the Critical Section (prevents context switching)
- risks hanging the entire system
- fails on multi-core processors because disabling interrupts on Core 1 does not stop Core 2
### Level 2: Locks 
- **Software Locks:** 
    - Peterson's Algorithm (2 processes) 
    - Bakery Algorithm (N processes using a ticket-number system where the lowest ticket is served first)
- **Hardware Locks:**
    - modern CPUs use **atomic instructions**
    - atomic instructions locks the memory bus for a specific memory address, and no other process or CPU core can touch that variable until the atomic instruction has completely executed
- **Note:** 
    - both software and hardware locks use `while(lock == busy)` and hence uses 100% of the CPU 
    - this is called **busy waiting**

### Level 3: OS-Based Solutions
- **Mutex:**
    - avoids busy waiting
    - if a lock is locked, the OS puts the process to **sleep**
    - once the lock gets unlocked, the OS **wakes** the process up
- **Semaphores:**
    - `counter`
    - `wait()` 
        - subtracts 1 from the `counter` 
        - if `counter == 0`, new process goes to sleep
    - `signal()` 
        - adds 1 to the `counter` 
        - if `counter == 0`, a sleeping process wakes up


    

