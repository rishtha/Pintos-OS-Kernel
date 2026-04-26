# Pintos Operating System Kernel

Implementation of core OS components in the Pintos educational operating system,
including thread scheduling, synchronization, and user program support.  
**Course:** CSE 421 — Operating Systems, University at Buffalo

## Overview
This project covers three major OS subsystems built from scratch in C:

- **Project 1 — Threads:** Alarm clock, priority scheduling, priority donation,
  and MLFQ (Multi-Level Feedback Queue) scheduler
- **Project 2 — User Programs:** Argument passing, system calls, process
  management, and file descriptor isolation

## Design Documents
- [PA1 — Threads Design Document](./PA1_DesignDoc.pdf)
- [PA2 — User Programs Design Document](./PA2_DesignDoc.pdf)

## Tech Stack
- **Language:** C
- **Platform:** Pintos (x86 educational OS)
- **Tools:** GDB, QEMU, Make

## Project 1 — Threads
### Alarm Clock
- Replaced busy-waiting with thread blocking using a sorted `sleep_list`
- Threads unblocked in `thread_tick()` to minimize interrupt handler overhead

### Priority Scheduling
- Implemented priority donation to prevent priority inversion
- Supported nested donation chains across multiple locks
- Tracked per-thread `hold_lock_list` and `block_lock` for donation propagation

### MLFQ Scheduler
- Implemented `nice`, `recent_cpu`, and `load_avg` using fixed-point arithmetic
- Priority recalculated every 4 ticks per thread, load average every second
- Abstracted fixed-point math via `fixed_point_t` type and `fix_add/mul/div`

## Project 2 — User Programs
### Argument Passing
- Used `strtok_r` to tokenize command-line arguments
- Arguments pushed onto user stack in reverse order with proper word alignment

### System Calls
- Implemented: `exec`, `wait`, `exit`, `create`, `open`, `read`, `write`,
  `close`, `remove`, `filesize`
- Per-process `fd_table[128]` for isolated file descriptor management
- Pointer validation via `validate_user_ptr()` and `validate_buffer()`
  before any kernel memory access

### Process Synchronization
- Parent-child sync via `child_process` struct with semaphore
- `wait()` blocks on semaphore until child signals via `sema_up` on exit

## Test Results
| Area | Test Cases |
|------|-----------|
| Alarm Clock | alarm-single, alarm-multiple, alarm-simultaneous, alarm-priority |
| Priority Scheduling | priority-donate-one, donate-multiple, donate-nest, donate-chain |
| MLFQS | mlfqs-load-1, mlfqs-fair-2, mlfqs-nice-2, mlfqs-block |

## Author
Rishitha Saravanan Priya  
[LinkedIn](https://linkedin.com/in/rishithasp) | [Portfolio](https://rishitha.dev)
