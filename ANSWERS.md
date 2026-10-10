# Assignment Questions

## Question 1: Thread vs Process

**Your Answer:**

A process is a program that is running and has its own memory space and resources. A thread is a smaller unit of execution within a process, and multiple threads can share the same memory and resources. Creating a new process usually requires more resources and overhead than creating a thread. In this assignment, we used Java threads by implementing the `Runnable` interface in the `Process` class and creating threads with `new Thread(process)`. Threads were suitable for this simulation because they allowed us to represent the execution of processes and simulate CPU scheduling using the Round-Robin algorithm. The `join()` method was also used to wait for each thread to finish its time slice before the scheduler continued to the next process.

---

## Question 2: Ready Queue Behavior

**Your Answer:**

In Round-Robin scheduling, if a process does not finish within its time quantum, it is placed at the end of the ready queue so that other processes can execute before it gets another turn. In my program, the time quantum is randomly selected as 2000, 3000, 4000, or 5000 milliseconds. The process runs for the smaller of its remaining execution time and the time quantum. If it still has remaining time and other processes are waiting in the queue, the program adds it to the end of the queue using the `addProcessToQueue()` method. This allows the scheduler to give other processes a chance to use the CPU before the unfinished process runs again. However, if the process is the last one remaining in the queue, my program runs it to completion instead of putting it back into the queue.

**Example from my output:**

```text
P1 completed quantum 3000ms
Remaining time: 2000ms
P1 yields CPU for context switch
P1 (Priority: 4) added to ready queue | Burst time: 5000ms
```

*Note: This is an illustrative example of the output format. The actual time values and priority depend on the program execution.*

**Explanation of example:**

In this example, P1 has a total burst time of 5000 milliseconds and receives a time quantum of 3000 milliseconds. After using its time slice, P1 still has 2000 milliseconds of execution time remaining. Since it has not finished and other processes are waiting in the ready queue, the scheduler places P1 at the end of the queue. The scheduler then selects the next process from the front of the queue. P1 will get another turn when the scheduler reaches it again.

---

## Question 3: Thread States

**Your Answer:**

The lifecycle of P1 can be understood by examining how the scheduler creates threads, executes a time quantum, and handles unfinished processes. In this simulation, a new Java thread is created for P1 when the `addProcessToQueue()` method is called. The thread starts when the scheduler calls `currentThread.start()`, and the scheduler uses `currentThread.join()` to wait for its execution to finish. If P1 has remaining execution time, a new thread is created for the next time slice. Therefore, the simulated process continues across multiple turns, although each individual Java thread handles only one time slice.

1. **New:** P1 is in the New state after the scheduler creates its thread using `new Thread(process)` but before calling `start()`.

2. **Runnable:** P1's thread becomes Runnable when the scheduler calls `currentThread.start()`. It is then eligible to receive CPU time.

3. **Running:** P1 executes its `run()` method and simulates its time quantum. During this execution, the thread calls `Thread.sleep()` to simulate the passage of time. While sleeping, the Java thread is technically in the Timed Waiting state rather than Running.

4. **Waiting:** If P1 has not finished, it is placed back in the ready queue to wait for another turn. In the Java implementation, the current thread finishes after its time slice, and the scheduler creates a new thread for P1's next turn. The `join()` method makes the main scheduler thread wait until the current thread finishes.

5. **Terminated:** The individual Java thread reaches the Terminated state when its `run()` method completes. If P1 still has remaining execution time, the simulated process can execute again through a newly created thread. The process is considered finished when its remaining execution time reaches zero.

---

## Question 4: Real-World Applications

### Example 1: Web Servers Handling Multiple Requests

**Description:**

A web server may receive requests from many users at the same time. Threads can be used to handle different requests, such as loading web pages, retrieving information from a database, or processing user input. Each request may need CPU time to complete its task. If one request requires more processing time, it can continue in another time slice while other requests receive attention.

**Why Round-Robin works well here:**

Round-Robin scheduling can provide fairness by giving each ready task a limited time quantum. This prevents one CPU-intensive task from continuously occupying the CPU while other tasks wait. It can improve responsiveness when multiple tasks compete for CPU time. However, real-world web servers often use more advanced scheduling and thread-pool mechanisms, so Round-Robin is one possible approach rather than the only solution.

### Example 2: Interactive Computer Applications

**Description:**

Interactive applications, such as desktop applications and some games, perform several tasks, including processing user input, updating application data, and handling background calculations. These tasks may be represented by separate threads. For example, a game may need to process game logic while also updating other parts of the application. If one task takes too long, other tasks may need an opportunity to execute.

**Why Round-Robin works well here:**

Round-Robin can help distribute CPU time fairly among tasks that are ready to execute. Its fixed time quantum allows each task to receive an opportunity to run instead of allowing one task to use the CPU indefinitely. This can help improve responsiveness when tasks have similar scheduling needs. However, highly interactive applications often require priority-based or real-time scheduling to ensure that important tasks respond quickly enough.

---

## Summary

**Key concepts I understood through these questions:**

1. A process and a thread are different: a process has its own memory space and resources, while threads within a process can share resources.
2. Round-Robin scheduling uses a FIFO ready queue and a fixed time quantum to distribute CPU time among processes.
3. Java thread states describe the lifecycle of a thread, and methods such as `start()`, `sleep()`, and `join()` affect its execution and state.

**Concepts I need to study more:**

1. Thread synchronization, race conditions, and safe access to shared resources.
2. The differences between Java thread states and the states of processes in a CPU scheduling simulation.
3. Advanced CPU scheduling algorithms and how they compare with Round-Robin in real-world systems. 
