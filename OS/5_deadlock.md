## Deadlock Fundamentals
### A. Definition
a state where a set of processes are blocked permanently because each holds a resource while waiting for another resource
```text
┌───────────────────────────────────────────────────────────────┐
│                                                               │
│                  RESOURCE CYCLE IN DEADLOCK                   │
│                                                               │
│   ┌──────────────┐       Assigned To       ┌──────────────┐   │
│   │  Resource 1  │ ──────────────────────► │  Process 1   │   │
│   └──────────────┘                         └──────────────┘   │
│          ▲                                        │           │
│          │ Waiting For                Waiting For │           │
│          │                                        ▼           │
│   ┌──────────────┐       Assigned To       ┌──────────────┐   │
│   │  Process 2   │ ◄────────────────────── │  Resource 2  │   │
│   └──────────────┘                         └──────────────┘   │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```
### B. Deadlock vs. Starvation
- **Deadlock:** multiple processes are stuck waiting on each other
- **Starvation:** a single low-priority process waits indefinitely because higher-priority processes keep cutting the line

## The 4 Necessary Conditions ("The Four Horsemen")
```text
┌─────────────────────────────────────────────────────────────────────────┐
│                                                                         │
│                       THE 4 NECESSARY CONDITIONS                        │
│                                                                         │
│  ┌─────────────────────────────┐     ┌─────────────────────────────┐    │
│  │  1. Mutual Exclusion        │     │  2. Hold and Wait           │    │
│  │  Resource is held by        │     │  Holding 1 resource while   │    │
│  │  only 1 process at time     │     │  waiting for another.       │    │
│  └─────────────────────────────┘     └─────────────────────────────┘    │
│                                                                         │
│  ┌─────────────────────────────┐     ┌─────────────────────────────┐    │
│  │  3. No Preemption           │     │  4. Circular Wait           │    │
│  │  Resources cannot be        │     │  Closed loop of processes   │    │
│  │  forcibly taken away.       │     │  waiting on each other.     │    │
│  └─────────────────────────────┘     └─────────────────────────────┘    │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## Strategies for Handling Deadlocks
### 1. Prevention
- **Break Mutual Exclusion:** make resources sharable
- **Break Hold and Wait:** force processes to request all required resources upfront
- **Break No Preemption:** if a process holding Resource A requests an unavailable Resource B, the OS forcibly revokes Resource A
### 2. Avoidance
- **Safe State:** a state where the OS guarantees a sequential order exists for every process to finish execution without deadlocking
- **Safe Path:** a specific order of executing processes that guarantees every process can finish without causing a deadlock
- **Banker's Algorithm:** 
    - requires processes to declare maximum potential resource needs in advance 
    - before granting a request, the OS verifies whether allocating resources leaves a **safe path** for the remaining processes
- **Resource Allocation Graph (RAG):** 
    - used for single-instance resources
    - denies requests if adding a request edge forms a cycle
### 3. Deadlock Detection & Recovery
- **Process Termination:** aborting one or all deadlocked processes
- **Resource Preemption:** forcibly reclaiming resources from a process until the cycle breaks
### 4. Deadlock Ignorance
- **Ostrich Algorithm:**
    - deadlocks occur rarely in standard workloads
    - continuous prevention or avoidance checks create heavy runtime overhead
    - OS simply pretends deadlocks do not exist
    - if a deadlock occurs, the computer freezes, and the user fixes it by rebooting

