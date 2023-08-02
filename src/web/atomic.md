# Atomic

## Concept

In programming, "atomic" refers to an operation or a set of operations that are executed as a single, indivisible unit. The term is commonly used in the context of concurrent programming, where multiple threads or processes may access shared resources or data simultaneously.

## Goal

The primary goal of atomic operations is to ensure that certain critical sections of code are executed without interruption or interference from other threads or processes. This prevents potential race conditions, data corruption, or other undesired behavior that could arise from concurrent access to shared resources.

For example, consider a scenario where two threads need to increment a shared variable simultaneously. If the increment operation is not atomic, it is possible that both threads could read the current value of the variable, increment it separately, and then write back their updated values. As a result, one of the increments would be lost, and the final value of the variable would be incorrect.

By making the increment operation atomic, both threads are guaranteed to read the current value, perform the increment, and write back the updated value as a single, uninterrupted operation. This ensures that the variable's value is correctly incremented, regardless of how many threads access it simultaneously.

## Methods

Atomicity can be achieved using various mechanisms provided by programming languages and platforms, such as atomic instructions, locks, mutexes, semaphores, or compare-and-swap operations.
