# Assignment 4: Threads & Synchronization

# Soufiane Yagoubi

## Project Description
This assignment explores the challenges of multi-threaded programming, with a focus on shared memory access, identifying data corruption through race conditions, and implementing a mutex/lock mechanism to guarantee thread safety.

---

## Technical Report

### 1. What is a Race Condition? (Observations from Part 1)
A race condition occurs when multiple concurrent threads attempt to modify a shared global variable simultaneously without any coordination.

The core issue comes from the fact that the high-level operation `counter += 1` is not atomic. At the CPU and OS instruction level, it is executed as three distinct steps:
1. Read: Load the current value of the counter from main memory into a local thread register.
2. Modify: Increment the register value by 1.
3. Write: Copy the updated value back from the register to the shared memory location.

When multiple threads execute this cycle concurrently, the OS scheduler can interrupt a thread mid-cycle (for example, after the Read step but before the Write step). A second thread then reads the old value, increments it, and writes it back. When the first thread resumes, it overwrites the progress made by the second thread, effectively causing lost updates. This explains why the actual final count in Part 1 falls significantly short of the expected 4,000,000.

### 2. How the Mutex/Lock Solved the Problem (Part 2)
To solve this concurrency issue, a mutex (mutual exclusion lock) via `threading.Lock()` was introduced in Part 2.

The lock acts as a gatekeeper that enforces mutual exclusion over the critical section (the code blocks reading and writing to `counter`).

- Before a thread is allowed to access the counter, it must call `acquire()`.
- If another thread already holds the lock, the requesting thread is safely put into a waiting/blocked state by the OS scheduler.
- Only when the owning thread completes its read-modify-write sequence and calls `release()` can the next thread take the lock.

This serialization guarantees that only one thread can execute the counter increment at any given moment, eliminating interleaving errors and ensuring a correct final result of 4,000,000.

---

## Result
result.odt

## How to Run the Program

Execute the script inside your terminal or cloud environment:
```bash
python3 thread.py
```
