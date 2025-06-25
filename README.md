# OperatingSystems
semester 6
## Unified IPC & Synchronization Suite

This repository contains two Operating Systems Lab projects demonstrating advanced UNIX Inter-Process Communication (IPC) and synchronization:

1. **Foodoku Interactive Puzzle** — a multi-process Sudoku variant.
2. **BarFooBar Restaurant Simulation** — a timed restaurant service simulation.

---

### Prerequisites

* GCC (C compiler)
* UNIX/Linux environment
* `xterm` for launching block processes
* Make utility

---

### Build

Run the following command to compile all modules:

```bash
make all
```

This will generate executables:

* `foodoku` (coordinator for Sudoku blocks)
* `cook`, `waiter`, `customer`, `resource`, `coordinator` (Restaurant simulation)

---

### Foodoku Interactive Puzzle

**Files:** `coordinator.c`, `block.c`, `boardgen.c` (included)

**How it works:**

* Spawns **9 block processes** in separate `xterm` windows.
* Uses UNIX pipes and `dup2()` redirection to chain blocks for row/column/block consistency checks.
* Supports interactive commands:

  * `h` — Show hint for current block.
  * `n` — Generate new puzzle (via `boardgen`).
  * `p b r c d` — Place/remove digit `d` at block `b`, row `r`, column `c`.
  * `s` — Solve and reveal entire puzzle.
  * `q` — Quit and cleanup.

**Launch:**

```bash
./foodoku
```

---

### BarFooBar Restaurant Simulation

**Files:** `cook.c`, `waiter.c`, `customer.c`, `resource.c`, `coordinator.c`

**How it works:**

* Simulates a lunch session (11 AM–3 PM) with:

  * **2 cooks**, **5 waiters**, **multiple customers**
* Allocates System V shared memory (\~2000 ints) for global time, queues, and table status.
* Protects shared data with System V semaphores (mutex, cook, waiter, per-customer).
* Advances time via `usleep()` delays:

  * 1 minute/order (100ms)
  * 5 minutes×party size for cooking
  * 30 minutes for dining
* Implements circular queues for order dispatch and completion signaling.

**Launch:**

```bash
./coordinator   # sets up IPC and spawns cook, waiter, customer processes
```

---

### Cleanup

To remove executables and object files:

```bash
make clean
```

---

### Notes

* Ensure no existing IPC resources conflict (use `ipcs`/`ipcrm` if needed).
* Requires interactive terminal support for `xterm` windows.

---

