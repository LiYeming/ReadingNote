# Designing concurrent code

- Techniques for dividing data between threads
- Factors that affect the performance of concurrent code
- How performance factors affect the design of data structures
- Exception safety in multi-threaded code
- Scalability
- Example implementations of several parallel algorithms

---
## Techniques for dividing work between threads

Questions:
 - How many threads to use? What tasks they should be doning?
 - Generalist thread v.s. Specialist thread
 - Driving reason for using concurrency and effects of performance and clarity of code

### Dividing data between threads before processing begins

`std::for_each`

Idea from MPI or OpenMP framework
- A task is split into a set of parallel tasks
- worker threads run these tasks independently
- combined in a final reduction step

### Dividing data recursively

`Quicksort` algorithm
- Spawning a new thread for each recursion would result in a large number of threads. Resulting worse performance or exhausion of memory.
- `std::async()` can handle this situation by deciding automatically.
- `std::thread::hardware_concurrency()` to choose the number of threads. Use a producer-consumer concurrency model to solve problem.
- Use a `thread pool` to implement

### Dividing work by task type

- Dividing work by task type to separate concerns
- 	Separating the wrong concerns, too much communication between two thread may be a good reason to join them into a single one.

### Dividing a sequence of tasks between threads

Use *pipeline* to exploit the avaiable concurrency of your system.
- A separate thread for each stage in the pipeline.


## Factors affecting the performance of concurrent code

### How many processors?

Behavior and performance characteristics of a concurrent program can vary considerably under different processors
- multiple single-core?
- single multi-core?
- multi multi-core?

Too many threads would result in over-subscription, waste processor time switching between the threads.
- `std::thread::hardware_concurrency()`
- `std::async()`
- Thread Pools
- Other applications running at the same time

### Data contention and cache ping-pong

One thread modifies data, need synchronization among CPU caches, which take time
- Depending on 
  - 	Nature of the operations on the two threads 
  - 	Memory ordering used for operations
- Modification may cause the second processor to stop in its tracks and wait for the change to propagate through the memory hardware
  - Phenomenally slow operation equivalent to many hundreds of individual instructions
  - `high contention`, processors are waiting for each other for a significant amount of time
  - `cache ping-pong` data passed back and forth between the caches many times
  - the contention on the `mutex`
  - Solution: Reduce the potential for two threads competing for the same memory location.

### False sharing

Processor caches deal with *cache lines*.

- When data items in a cache line are unrelated and need to be accessed by different threads, this can be a major cause of performance problems.
- Cache ping-pong happens when different threads processing adjacent locations in a single cache line.
- Solution: 
  - Data items to be accessed by the same thread are close together in memory.
  - Data items to be accessed by different thread are far apart in memory.
  - `std::hardware_destructive_interference_size` specifies the maximum number of consecutive bytes that may be subject to false sharing for the current compilation target.

### How close is your data?

If there are more threads than cores in the system, each core is going to be  running multiple threads. This increases the pressure on the cache.

- When processor switches threads, it is more likely to reload cache. If each thread uses data spread across multiple cache lines.
- `std::hardware_constructive_interference_size` shows the maximum number of consecutive bytes guaranteed to be on the same cache line if suitably aligned.

### Over-subscription and excessive task switching

It's typical to have more threads than processors, unless you're running on a massively parallel hardware.

## Designing data structures for multi-threaded performance

How can you use information above when designing data structures for multi-threaded performance?

Key points:

- contention
- false sharing
- data proximity

### Dividing array elements for complex operations

- Try to adjust the data distribution between threads so that data that's close together is worked on by the same thread
- Try to minimize the data required by any given thread
- Try to ensure that data accessed by separate threads is sufficiently far apart to avoid false sharing using std::hardware_destructive_interference_size as a guide.

## Additional considerations when designing for concurrency

- Exception safety
- Scalability, performance increases as more processing cores are added in the system, ideally linear.

### Exception safety in parallel algorithms

In single thread program, only need to tidy up and avoid resource leaks or broken invariants.

In a parallel algorithm, many operations will be running on separate threads, the exception can't be allowed to propagate because it's on the wrong call stack.

Use `std::packaged_task` and `std::future`

- exception thrown in a worker thread are re-thrown in the main thread.
- leaking threads if an exception is thrown between when you spawn the first thread and when you've joined with them all.
  - catch any exception, join threads and rethrow the exception.
  - Use RAII to do the process, more efficiently.

`std::async()`

- The future's destructor will be blocked until thread is complete.
- take advantage of the hardware concurrency, but also trivially exception-safe.

### Scalability and Amdahl's law

Amdahl's law:
$$
P = \frac{1}{f_s + \frac{1 - f_s}{N}}
$$
$f_s$ is the `serial` section as a fraction of the program. $N$ is the number of processors. $P$ is the performance gain.

Implications:

- Minimize `serial` sections.

Scalability is about reducing the time it takes to perform an action or increasing the amount of data that can be processed in a given time as more processors are added.

### Hiding latency with multiple threads

Threads may not always have useful work to do.

- I/O
- mutex
- barrier
- conditional variable

Wasting of time if thread\_number = processor\_number if threads are waiting.



Important to measure performance before and after any change in the number of threads. Optimal thread number will depend on

- nature of the work.
- percentage of time the thread spends waiting.

## Designing concurrent code in practice

### std::for_each

### std::find

- An example how data access patterns can affect the design of your code.

### std::partial_sum

- First algorithm, separate the list into chunks, and do partial_sum on each chunk. After that, add the last element of the previous chunk to the current chunk, serially from begin to end.
  - O(N) in complexity.
- Second algorithm, add n-1, add n-2, add n-4, add n-8.....
  - O(NlogN) in complexity, slower than the first algorithm.
- However, when in a parallelized system, for each and every processor, only O(logN) is calculated, thus faster than the first algorithm.
  - More easily to refactor to a parallelized system.



















