# 0-Preface
The following story provides a backdrop for gaining this understanding. The central
focus of Asyncio is on how best to best perform multiple tasks at the same time—and
not just any tasks, but specifically tasks that involve waiting periods. The key insight
required with this style of programming is that while you wait for this task to com‐
plete, work on other tasks can be performed.

(Story about retorant and ThreadBots)

## What Problem Is Asyncio Trying to Solve?
For I/O-bound workloads, there are exactly (only!) two reasons to use async-based
concurrency over thread-based concurrency:
• Asyncio offers a safer alternative to preemptive multitasking (i.e., using threads),
thereby avoiding the bugs, race conditions, and other nondeterministic dangers
that frequently occur in nontrivial threaded applications.
• Asyncio offers a simple way to support many thousands of simultaneous socket
connections, including being able to handle many long-lived connections for
newer technologies like WebSockets, or MQTT for Internet of Things (IoT)
applications.

That’s it.

