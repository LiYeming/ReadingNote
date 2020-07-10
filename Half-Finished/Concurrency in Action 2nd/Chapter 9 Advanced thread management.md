# Advanced thread management



## Thread pools

Tasks that can be executed concurrently are submitted to the pool, which puts then on a queue of pending work.

Each task is then taken from the queue by one of the worker threads, which executes the task before looping back to take another from the queue.

### The simplest possible thread pool

- A fixed number of *worker threads* that process work

### Waiting for tasks submitted to a thread pool







## Interrupting threads

