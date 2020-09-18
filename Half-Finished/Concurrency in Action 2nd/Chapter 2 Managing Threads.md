## Chapter 2 Managing Threads

- Launching a thread:

  ```c++
  void do_some_work();
  std::thread my_thread(do_some_work, args...);
  ```

  std::thread works with any callable type, the supplied function object is copied into the storage belonging to the newly created thread of execution and invoked from there.

- Terminate a thread:

  - joined or detached, decide before std::thread is destroyed.
  - If detached, ensure the named state of the callable object is valid.
    - Detached threads are no longer joinable.
  - If joined, the new thread would wait the main thread, or the other way. It is not efficient in this way.
    - Also the thread needs to be joinable before join().
    - Use RAII to manage.

- Passing arguments, beware of dangling pointer problem.

  - Use manager to own the argument you pass.
    - pointer may be a dangling pointer, get ownership!
    - reference may use a wrapper instead, like std::ref or std::cref!
  - When using function pointer to class member, the first element must be X&.
  - If the passed object cannot be copied, beware of a moved object.

- Transfer ownership of a thread.

  - You can’t just drop a thread by assigning a new value to the std::thread object that manages it.
  - Can be used to build a management class for threads.

- Use std::thread::hardware_concurrency() to show the number of threads the hardware support, return 0 if this information is not available.

  - Don't use too much threads, overload the context switching.

- We can identify thread's id by using std::thread::get_id(), supported by std::cout.

