# Assignment 3 - Complete Documentation

**Student Name**: [Noura Khalid Alhilali]  
**Student ID**: [445052045]  
**Date Submitted**: [2026/5/7]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

Part 1: Development Log (1 mark)
Document your development process with minimum 3 entries showing progression:

Entry 1 - [April 25, 2026 - 4:30 PM]
What I implemented:
Added ReentrantLock objects to protect shared counter variables such as contextSwitchCount, completedProcessCount, and totalWaitingTime.

Challenges encountered:
Multiple threads were updating the counters at the same time which caused inconsistent values between runs.

How I solved it:
I used lock.lock() before modifying the counters and unlock() inside finally blocks to ensure proper synchronization.

Testing approach:
Ran the scheduler simulation several times and compared final counter values across executions.

Time spent:
1 hour 15 minutes

Entry 2 - [April 26, 2026 - 7:10 PM]
What I implemented:
Added synchronization for the execution log ArrayList using ReentrantLock.

Challenges encountered:
The program sometimes threw ConcurrentModificationException when multiple threads accessed the execution log simultaneously.

How I solved it:
Protected all add and read operations on the execution log with a dedicated lock.

Testing approach:
Executed the program repeatedly with multiple threads and monitored the log output for exceptions.

Time spent:
1 hour

Entry 3 - [April 27, 2026 - 6:40 PM]
What I implemented:
Implemented Semaphore to control CPU access and allow only one process to use the CPU at a time.

Challenges encountered:
Processes were entering the critical section simultaneously which caused scheduling inconsistencies.

How I solved it:
Used a binary semaphore with one permit to ensure mutual exclusion for CPU access.

Testing approach:
Verified that only one thread executed CPU-related code at a time using printed execution logs.

Time spent:
1 hour 20 minutes

Entry 4 - [April 29, 2026 - 5:15 PM]
What I implemented:
Reviewed synchronization mechanisms and added try-finally blocks to all lock sections.

Challenges encountered:
Forgetting to release locks could potentially cause deadlocks.

How I solved it:
Placed every unlock() call inside finally blocks to guarantee lock release even if exceptions occur.

Testing approach:
Forced multiple executions and checked that the program never froze or stopped responding.

Time spent:
50 minutes

Entry 5 - [April 30, 2026 - 8:00 PM]
What I implemented:
Completed documentation, verified final outputs, and reviewed commit history.

Challenges encountered:
Ensuring all synchronization concepts were clearly explained in the documentation.

How I solved it:
Compared the implementation with assignment requirements and added explanations for each synchronization mechanism.

Testing approach:
Ran the program five times and confirmed stable and correct results in every run.

Time spent:
1 hour


---

Part 2: Technical Questions (1 mark)

Question 1: Race Conditions
Q: Identify and explain TWO race conditions in the original code. For each:

What shared resource is affected?
Why is concurrent access a problem?
What incorrect behavior could occur?

Your Answer:

The first race condition existed in the shared counter variables such as contextSwitchCount and completedProcessCount. Multiple threads could increment these variables simultaneously, causing lost updates because increment operations are not atomic. This could produce incorrect final statistics and inconsistent scheduler results between executions. For example, two threads might both read the same counter value before updating it, resulting in only one increment being stored.

The second race condition occurred in the executionLog ArrayList. Multiple threads accessed and modified the list concurrently without synchronization. Since ArrayList is not thread-safe, concurrent modifications could cause ConcurrentModificationException or corrupted log entries. This would make the execution history incomplete or incorrectly ordered.

Question 2: Locks vs Semaphores
Q: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

Your Answer:

ReentrantLock is used to provide mutual exclusion when accessing shared resources. It allows only one thread at a time to execute critical sections and gives more control than synchronized blocks. In my code, I used ReentrantLock to protect shared counters and the execution log because these resources needed safe updates from multiple threads.

Semaphore controls access to a limited number of shared resources using permits. Unlike locks, semaphores can allow multiple threads depending on the number of permits available. I used a binary semaphore with one permit to simulate CPU access, ensuring that only one process could execute on the CPU at any moment. This matched the scheduling behavior required in the assignment.

Question 3: Deadlock Prevention
Q: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

Your Answer:

Deadlock happens when two or more threads wait forever for resources locked by each other. This causes the program to stop progressing because no thread can continue execution.

The first prevention technique I used was try-finally blocks. Every lock acquisition was followed by unlock() inside a finally block to guarantee that locks are always released even if exceptions occur.

The second technique was maintaining consistent lock usage and avoiding unnecessary nested locks. By keeping critical sections small and organized, I reduced the chance of circular waiting between threads. These approaches helped prevent deadlocks and improved program stability.

Question 4: Lock Granularity Design Decision
Q: For Task 1 (protecting the three counters), explain your lock design choice:

Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
Explain WHY you made this choice
What are the trade-offs between the two approaches?
Given that the three counters are independent, which approach provides better concurrency and why?

Your Answer:

I used separate locks for each counter, which is considered fine-grained locking. I made this choice because the three counters are independent and do not rely on each other during updates. Using separate locks allows multiple threads to update different counters simultaneously without unnecessary blocking.

The advantage of coarse-grained locking is simpler implementation because only one lock is managed. However, it reduces concurrency because all threads must wait even when accessing different counters. Fine-grained locking improves performance and concurrency but increases implementation complexity since more locks must be managed carefully.

Since the counters are independent, fine-grained locking provides better concurrency because threads can work on separate resources at the same time. This reduces waiting time and improves efficiency in multithreaded execution.


---

Part 3: Synchronization Analysis (1 mark)

Critical Section #1: Counter Variables
Which variables:
contextSwitchCount, completedProcessCount, totalWaitingTime

Why they need protection:
These variables are shared between multiple threads and can be updated simultaneously, causing race conditions and incorrect values.

Synchronization mechanism used:
ReentrantLock

Code snippet:

counterLock.lock();
try {
    contextSwitchCount++;
} finally {
    counterLock.unlock();
}

Justification:
The lock ensures that only one thread updates the counters at a time, preventing lost updates and inconsistent results.

Critical Section #2: Execution Log
What resource:
executionLog ArrayList

Why it needs protection:
Multiple threads add log entries concurrently, which can corrupt the list or cause ConcurrentModificationException.

Synchronization mechanism used:
ReentrantLock

Code snippet:

logLock.lock();
try {
    executionLog.add(logEntry);
} finally {
    logLock.unlock();
}

Justification:
The lock guarantees thread-safe access to the execution log and preserves correct execution history.

Critical Section #3: CPU Semaphore
Purpose of semaphore:
To ensure that only one process accesses the CPU at a time.

Number of permits and why:
1 permit because the CPU can only execute one process at a time in this simulation.

Where implemented:
Inside the CPU execution section of the scheduler.

Code snippet:

cpuSemaphore.acquire();
try {
    executeProcess(process);
} finally {
    cpuSemaphore.release();
}

Effect on program behavior:
The semaphore prevents simultaneous CPU access and maintains correct scheduling behavior.


---

Part 4: Testing and Verification (2 marks)

Test 1: Consistency Check
What I tested: Running program multiple times to verify consistent results

Testing procedure:

java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync

Results:
All executions produced consistent final values for counters, waiting time, and context switches. No random differences appeared between runs.

Why synchronization is necessary:
Without synchronization, multiple threads could update shared counters simultaneously and overwrite each other’s changes. The execution log could also become corrupted because ArrayList is not thread-safe. Synchronization protects shared resources and guarantees correct program behavior.

Conclusion:
Synchronization successfully removed inconsistent behavior and ensured reliable results across executions.

Test 2: Exception Testing
What I tested: Checking for ConcurrentModificationException

Testing procedure:
Ran the program repeatedly while multiple threads accessed the execution log.

Results:
No ConcurrentModificationException occurred after adding synchronization to the ArrayList.

What this proves:
The execution log is now thread-safe and properly synchronized.

Test 3: Correctness Verification
What I tested: Verifying correct final values (total burst time, context switches, etc.)

Expected values:
Correct totals based on the process scheduling calculations.

Actual values:
The actual output matched the expected calculations in all test runs.

Analysis:
The synchronization mechanisms protected shared resources correctly and preserved accurate scheduler statistics.

Test 4: Different Scenarios
Scenario tested: Different time quantum values

Purpose:
To verify that synchronization still works correctly under different scheduling conditions.

Results:
The program remained stable and produced correct outputs even when the time quantum changed.

What I learned:
Synchronization mechanisms work independently from scheduling parameters and remain necessary in all execution scenarios.


---

Part 5: Reflection and Learning

What I learned about synchronization:
I learned that synchronization is necessary whenever multiple threads share resources. Race conditions can easily occur if shared variables are modified without protection. ReentrantLock provides flexible control for protecting critical sections, while semaphores are useful for controlling access to limited resources. I also learned the importance of try-finally blocks to avoid deadlocks and ensure locks are always released. Testing multithreaded programs multiple times is important because synchronization bugs may not appear in every run. This assignment helped me understand how operating systems coordinate concurrent execution safely. I also improved my understanding of thread safety and resource management in Java.

Real-world applications:
Give TWO examples where synchronization is critical:

Example 1:
Banking systems where multiple users access and update account balances simultaneously.

Example 2:
Airline reservation systems where many users book seats at the same time.

How I would explain synchronization to others:
Synchronization is like managing access to a single bathroom key shared by many people. Without rules, everyone may try to enter at the same time and create confusion. Locks and semaphores act like organized systems that allow only the correct number of people to access shared resources safely. In programming, synchronization prevents threads from interfering with each other and keeps the program results correct and predictable.


---

Part 6: GitHub Repository Information

Repository URL:
OS-Assignment3-Norah-Khalid Repository

Number of commits:
4

Commit messages:

1. Set my student ID and initial setup


2. Added locks for shared counters


3. Implemented semaphore and execution log synchronization


4. Completed testing and documentation



Summary
Total time spent on assignment:
6-7 hours

Key takeaways:

1. Synchronization prevents race conditions in multithreaded programs.


2. Locks and semaphores have different purposes and use cases.


3. Proper testing is essential for verifying thread safety.



Most challenging aspect:
Debugging synchronization issues and ensuring all shared resources were properly protected.

What I'm most proud of:
Successfully implementing synchronization mechanisms that produced stable and consistent results across all test runs.

End of Documentation
