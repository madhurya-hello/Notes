## Operating System & Its Types
### Operating System: 
- acts as an interface between hardware and applications by managing system resources
```text
┌─────────────────────────────────────────────────────────────┐   
│                           USER                              │ ├─────────────────────────────────────────────────────────────┤ 
│                       APPLICATIONS                          │ ├─────────────────────────────────────────────────────────────┤ 
│          OPERATING SYSTEM (Kernel & Interface)              │ ├─────────────────────────────────────────────────────────────┤ 
│                         HARDWARE                            │ └─────────────────────────────────────────────────────────────┘
```
### Types of Operating Systems:
- **Windows** (by Microsoft)
- **macOS** (by Apple for its Mac computers)
- **Linux** (open source and customizable)
- **Unix** (foundation for Linux & macOS)
- **Android & iOS** (for smartphones)
- **Real-Time OS** (for real-time IoT devices)

## Program vs. Process vs. Thread
### A. Program:
- passive file stored on disk
### B. Process:
- an active program in execution 
- loaded into RAM 
- own isolated memory space, resources, and execution context)
### C. Thread:
- a lightweight unit of execution inside a process 
- threads within the same process share memory-space and resources

### Example: Google Chrome
```text
┌────────────────────────────────────────────────────────────────────────┐
│                                                                        │
│                      PROCESS (e.g., Chrome)                            │
│                                                                        │
│   ┌──────────────────────────────────────────────────────────┐         │
│   │  Shared Resources: Code Segment, Heap, File Handles      │         │
│   └──────────────────────────────────────────────────────────┘         │
│                                                                        │
│   ┌─────────────────┐  ┌──────────────────────┐  ┌─────────────────┐   │
│   │    Thread 1     │  │       Thread 2       │  │    Thread 3     │   │
│   │  (UI Handler)   │  │ (Network Download)   │  │   (Rendering)   │   │
│   └─────────────────┘  └──────────────────────┘  └─────────────────┘   │
│                                                                        │
└────────────────────────────────────────────────────────────────────────┘
```

## The 4 Execution Models
### 1. Multiprocessing
- multiple CPU cores working on separate jobs at the same clock cycle
- if `Process A` crashes, `Process B` keeps running unaffected
- **example:** 
    - each chrome tab runs in its own dedicated process
    - if a heavy script crashes one tab, the main browser window stays alive
```text
┌───────────────────────────────────────────────────────────────────┐
│                          MULTIPROCESSING                          │
├──────────────────────────────────┬────────────────────────────────┤
│              Core 1              │             Core 2             │
│            Process A             │           Process B            │
│        (Isolated Memory)         │       (Isolated Memory)        │
└──────────────────────────────────┴────────────────────────────────┘
```
### 2. Multithreading
- one process splitting its main task into multiple mini-tasks
- threads share memory-space and resources, but keep independent execution contexts
- **example:**
    - inside one video-game process: 
        - Thread 1 calculates physics
        - Thread 2 plays 3D spatial audio
        - Thread 3 manages multiplayer network calls
        - Thread 4 renders frames 
    - all four threads read from the shared memory
```text
┌─────────────────────────────────────────────────────────┐
│                     MULTITHREADING                      │
├─────────────────────────────────────────────────────────┤
│                        CPU Core                         │
│  Process (Shared Memory Space)                          │
│  ├── Thread 1 (Physics Calculation)                     │
│  ├── Thread 2 (Audio Processing)                        │
│  └── Thread 3 (Frame Rendering)                         │
└─────────────────────────────────────────────────────────┘
```
### 3. Multiprogramming
- keeping multiple programs loaded in RAM
- CPU immediately switches to another program whenever the current one is waiting
- designed to eliminate CPU idle waste
- **example:** 
    - the payroll waits for data from disk storage
    - the CPU instantly calculates inventory algorithms
```text
┌───────────────────────────────────────────────────────────────┐
│                        MULTIPROGRAMMING                       │
├───────────────────────────────────────────────────────────────┤
│  RAM: [Job A (Waiting for I/O)] ──► CPU Switches ──► [Job B]  │
│  Goal: Minimize CPU idle time during slow I/O operations.     │
└───────────────────────────────────────────────────────────────┘
```
### 4 Multitasking
- CPU rapidly switches between multiple active programs, creating the illusion that everything runs at once
- OS allocates tiny time-slices per task
- **example:** 
    - we can stream music on Spotify, download a movie, and code in an IDE simultaneously
```text
┌───────────────────────────────────────────────────────────────────────┐
│                             MULTITASKING                              │
├───────────────────────────────────────────────────────────────────────┤
│  CPU Time Slices: [10ms Spotify] ──► [10ms IDE] ──► [10ms Download]   │
│  Goal: Maximize user responsiveness and interactivity.                │
└───────────────────────────────────────────────────────────────────────┘
```
