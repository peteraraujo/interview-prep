## Race Conditions

A race condition is a flaw in a system or software where the output depends on the unpredictable sequence or timing of uncontrollable events. It occurs when two or more threads (or processes) access and modify the same shared data at the same time.

Because the operating system’s scheduler can switch between threads at any moment, the threads “race” each other. If they complete in the wrong order, the shared data becomes corrupted.

### The Classic Example: Bank Account
Consider a shared bank account with a balance of $100. Two transactions occur simultaneously:

- Thread A deposits $50.
- Thread B withdraws $20.

The expected final balance is $130 ($100 + $50 − $20).

Updating the balance is not atomic. It requires three steps:

1. Read the current balance.
2. Calculate the new balance.
3. Write the new balance back to the database.

**How the race condition corrupts the data:**

- Thread A reads $100.
- (Context switch: scheduler pauses Thread A and runs Thread B.)
- Thread B reads $100.
- Thread A calculates $100 + $50 = $150 and writes $150.
- Thread B calculates $100 − $20 = $80 and writes $80.

**Final result:** The account balance is $80. Thread B overwrote Thread A’s update using stale data, causing the $50 deposit to disappear.

### How to Prevent Race Conditions
To eliminate race conditions, shared code must be made **atomic** (indivisible and uninterruptible).

The most common solution is a **Mutex** (Mutual Exclusion) lock:

- When Thread A needs to update the balance, it acquires (“locks”) the mutex.
- Thread B is forced to wait in a queue until Thread A completes its read-calculate-write sequence and releases (“unlocks”) the mutex.
- Only one thread can execute the critical section at a time, guaranteeing correct results regardless of scheduling order.

