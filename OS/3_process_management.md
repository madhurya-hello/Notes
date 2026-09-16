## Process Types
### A. CPU-Bound:
- spends most of its time calculating on the CPU
- **example:** video editing, bitcoin mining
### B. I/O-Bound:
- spends most of its time waiting for data
- **example:** browsing the web, typing in Word

## Context Switching & Process Control Block (PCB)
### A. PCB
- data structure storing process details
- PID, Process State, Program Counter, and CPU Registers
- **analogy**: a note attached to a dish: *"I was on step 5, heat at 200°C, 2 onions left"*
### B. Context Switching
- saving the PCB of the current process and loading the PCB of the next process
- context switching is considered an overhead because the CPU performs management work rather than something useful 
```text
┌─────────────────────────────────────────────────────────────────┐
│                   CONTEXT SWITCHING MECHANISM                   │
├─────────────────────────────────────────────────────────────────┤
│   [Process A Running]                                           │
│            │                                                    │
│            ▼ Save state to PCB A                                │
│       ┌─────────────────────────┐                               │
│       │  PCB A: PID, PC, Regs   │ ◄── Overhead (No useful work) │
│       └─────────────────────────┘                               │
│            │                                                    │
│            ▼ Load state from PCB B                              │
│   [Process B Running]                                           │
└─────────────────────────────────────────────────────────────────┘
```

## CPU Scheduling
### Core Metrics
- **Arrival Time (AT)**: Clock time when a process enters the Ready Queue
- **Burst Time (BT)**: CPU execution time required by the process
- **Completion Time (CT)**: Clock time when execution finishes
- **Turnaround Time (TAT)**: Total time the process spent in the system
- **Waiting Time (WT)**: Total time spent idle in the queue waiting for CPU
- **Response Time (RT)**: Time from submission until first response is produced
- Formula:
    - $TAT = CT - AT$
    - $WT = TAT - BT$
### Preemptive vs. Non-Preemptive
- **Preemptive:** OS can forcibly interrupt a running process
- **Non-Preemptive:** once a process gets the CPU, it runs until completion or until it voluntarily requests I/O

## Top 5 Scheduling Algorithms
### First Come, First Serve (FCFS)
- non-preemptive
- processes execute in exact order of arrival
- **convoy effect:** when a giant process runs first, all small processes wait behind it
### Shortest Job First (SJF)
- non-preemptive
- picks the process with the smallest Burst Time
- mathematically proven to give the **Minimum Average Waiting Time**
### Shortest Remaining Time First (SRTF)
- preemptive
- just the preemptive version of SJF
- swaps if a newly arrived process has a shorter remaining burst time than the running process
### Round Robin (RR)
- preemptive
- uses the **queue** data structure
- a process after completion moves to the back of the queue
- each process receives a fixed **Time Quantum** (e.g. 2ms)
- **observations:**
    - Time Quantum too large $\rightarrow$ behaves like FCFS
    - Time Quantum too small $\rightarrow$ high context switching overhead
### Priority Scheduling
- runs processes based on assigned priority numbers
- **starvation:** low-priority jobs may sit in queue indefinitely
- **aging:** gradually increases the priority of waiting processes over time