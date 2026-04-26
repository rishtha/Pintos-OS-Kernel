# Pintos OS Kernel — Project 1: Threads

Implementation of core thread scheduling and synchronization in the
Pintos educational OS kernel, written in C.  
**Course:** CSE 421 — Operating Systems, University at Buffalo

## Design Document
[PA1 — Threads Design Document](./PA1_DesignDoc.pdf)

## What Was Built

### Alarm Clock
- Replaced busy-waiting `timer_sleep()` with thread blocking
- Maintained a sorted `sleep_list` ordered by wake-up tick
- Threads unblocked efficiently in `thread_tick()` to minimize
  interrupt handler overhead

### Priority Scheduling
- Implemented priority-based thread selection across ready queue,
  semaphore waiters, and condition variable waiters
- Implemented priority donation to resolve priority inversion
- Supported nested donation chains across multiple locks
- Per-thread `hold_lock_list` tracks held locks for donation propagation
- `block_lock` tracks what lock a thread is currently waiting on

### MLFQ Scheduler (Advanced Scheduler)
- Implemented `nice`, `recent_cpu`, and `load_avg` per Pintos BSD spec
- Priority recalculated every 4 ticks, load average updated every second
- Abstracted fixed-point arithmetic via `fixed_point_t` and
  `fix_add`, `fix_sub`, `fix_mul`, `fix_div` functions

## Key Data Structures
```c
struct thread {
    int64_t wkup_ticks;         // Wake-up tick for sleeping threads
    int priority;               // Current effective priority
    int initial_priority;       // Base priority before donation
    struct list hold_lock_list; // Locks currently held
    struct lock *block_lock;    // Lock this thread is waiting on
    int nice;                   // MLFQS niceness value
    fixed_point_t recent_cpu;   // Recent CPU usage (fixed-point)
};

struct lock {
    int lock_priority;          // Max priority among waiters
    struct list_elem elem_lock; // For thread's lock list
};

static struct list sleep_list;  // Sorted list of sleeping threads
fixed_point_t LoadAvg;          // System-wide load average
```

## Test Coverage
| Category | Test Cases Owned |
|----------|-----------------|
| Priority Donation | priority-donate-one, donate-multiple, donate-multiple2, donate-nest, donate-sema, donate-lower, donate-chain |
| Priority Scheduling | priority-sema, priority-condvar |
| MLFQS | mlfqs-load-1, mlfqs-load-60, mlfqs-fair-2, mlfqs-nice-2, mlfqs-block |

## Tech Stack
- **Language:** C
- **Platform:** Pintos (x86 educational OS)
- **Tools:** GDB, QEMU, Make

## Author
Rishitha Saravanan Priya  
[LinkedIn](https://linkedin.com/in/rishithasp) | [Portfolio](https://rishitha.dev)
