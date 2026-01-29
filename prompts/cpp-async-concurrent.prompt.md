---
agent: 'agent'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems']
description: 'Get best practices for C++ asynchronous and concurrent programming'
applyTo: '**/*.cpp, **/*.h, **/*.hpp, **/*.cc, **/*.cxx'
---

# C++ Asynchronous and Concurrent Programming Best Practices

Your goal is to help me follow best practices for asynchronous, concurrent, and parallel programming in C++ using modern C++17/20/23 features.

## Async Primitives and Return Types

### Future/Promise Pattern
- Use `std::future<T>` to retrieve results from asynchronous operations
- Use `std::promise<T>` to set values or exceptions in separate threads
- Use `std::async()` for simple fire-and-forget async operations
- Consider `std::packaged_task` to wrap callable objects with future/promise
- Check future validity with `.valid()` before calling `.get()`

### Thread Management
- Use `std::jthread` (C++20) for automatic joining and stop token support
- Use `std::thread` for C++17 or when fine-grained control is needed
- Always join or detach threads explicitly (unless using `std::jthread`)
- Pass thread arguments by value or use `std::ref()` for references
- Use `std::this_thread::sleep_for()` and `std::this_thread::yield()` appropriately

### Coroutines (C++20)
- Use `co_await`, `co_return`, `co_yield` for coroutine-based async operations
- Define coroutine return types (`std::coroutine_handle`, custom promise types)
- Leverage coroutines for state machine implementations and async I/O
- Document coroutine suspension points and lifetime management

## Thread Safety and Synchronization

### Mutual Exclusion
- Use `std::mutex` for exclusive access to shared resources
- Always use RAII guards: `std::lock_guard` for simple locking, `std::unique_lock` for advanced scenarios
- Use `std::scoped_lock` (C++17) for locking multiple mutexes (deadlock-free)
- Use `std::shared_mutex` (C++17) for read-write locks (multiple readers, single writer)
- Never manually lock/unlock mutexes without RAII guards

### Lock-Free Programming
- Use `std::atomic<T>` for lock-free operations on primitive types and trivially copyable types
- Use memory ordering: `memory_order_relaxed`, `memory_order_acquire`, `memory_order_release`, `memory_order_seq_cst`
- Prefer `memory_order_seq_cst` (default) unless you understand weaker orderings
- Use `std::atomic_flag` for simple spinlock implementations
- Test with ThreadSanitizer to detect data races

### Condition Variables
- Use `std::condition_variable` for thread signaling and waiting
- Always use with a predicate to avoid spurious wakeups: `cv.wait(lock, []{ return condition; })`
- Use `std::condition_variable_any` when working with custom lock types
- Notify under lock only if necessary; prefer notifying after unlock

## Naming Conventions

- Use descriptive names for thread functions: `workerThread`, `processingLoop`
- Prefix atomic variables: `atomicCounter`, `atomicFlag` (or use `m_atomic*` for members)
- Suffix async functions if helpful for clarity: `processDataAsync`, `loadAsync`
- Name mutexes after the resource they protect: `dataMutex`, `queueMutex`
- Use clear names for locks: `dataLock`, `queueGuard`

## Exception Safety in Concurrent Code

- Ensure destructors are `noexcept` (especially for RAII lock guards and thread wrappers)
- Handle exceptions in thread functions; uncaught exceptions call `std::terminate()`
- Use `std::exception_ptr` with `std::current_exception()` to transfer exceptions across threads
- Store exceptions in promises: `promise.set_exception(std::current_exception())`
- Design for exception safety: basic, strong, or nothrow guarantees
- Never throw from destructors or move operations

## Performance and Optimization

### Parallel Algorithms (C++17)
- Use execution policies: `std::execution::seq`, `std::execution::par`, `std::execution::par_unseq`
- Apply to STL algorithms: `std::sort`, `std::transform`, `std::for_each`, etc.
- Example: `std::sort(std::execution::par, vec.begin(), vec.end())`
- Profile to ensure parallel execution provides benefit

### Task-Based Parallelism
- Prefer task-based parallelism (`std::async`) over thread-based for simple cases
- Use thread pools for managing worker threads and task queues
- Consider work-stealing algorithms for load balancing
- Minimize thread creation/destruction overhead through pooling

### Avoiding Performance Pitfalls
- Minimize lock contention: keep critical sections small
- Avoid false sharing: align cache lines with `alignas(std::hardware_destructive_interference_size)`
- Use `std::atomic::load()`/`store()` instead of direct assignment for clarity
- Prefer lock-free algorithms in hot paths when safe and measurable
- Profile with tools: perf, vtune, ThreadSanitizer

## Common Concurrency Pitfalls

### Data Races
- **Never** access shared mutable state without synchronization
- Protect all shared data with appropriate synchronization primitives
- Use ThreadSanitizer (`-fsanitize=thread`) to detect races
- Document thread-safety guarantees for public APIs

### Deadlocks
- Establish and follow consistent lock ordering across all code
- Use `std::scoped_lock` to lock multiple mutexes atomically
- Avoid nested locking when possible
- Use lock-free algorithms where appropriate
- Implement timeouts with `try_lock_for()` or `try_lock_until()`

### Resource Leaks
- Always join or detach threads before thread objects are destroyed
- Use `std::jthread` (C++20) for automatic joining
- Wrap threads in RAII classes for automatic lifetime management
- Ensure futures are retrieved (`.get()` or `.wait()`) to propagate exceptions

### Blocking Operations
- Never call blocking operations while holding locks
- Avoid busy-waiting; use condition variables for signaling
- Don't use `.wait()` on futures with short timeouts in tight loops
- Consider async I/O instead of blocking I/O with many threads

## Concurrency Design Patterns

### Active Object Pattern
- Asynchronous method invocation with private scheduler thread
- Return `std::future<T>` from public methods
- Queue method requests and execute on background thread
- Serialize access to object state through message queue

### Monitor Object Pattern
- Encapsulate mutex and condition variable with protected data
- Provide thread-safe methods using RAII lock guards
- All public methods acquire lock before accessing shared state

### Thread Pool Pattern
- Fixed number of worker threads processing task queue
- Submit tasks as `std::function<void()>` or similar
- Use `std::packaged_task` to get futures for task results
- Implement graceful shutdown with poison pill or flag

### Producer-Consumer Pattern
- Use thread-safe queue (mutex + condition variable)
- Producers push work items, consumers pop and process
- Handle queue shutdown gracefully (notify all consumers)
- Consider lock-free queues for high-performance scenarios

### Read-Write Lock Pattern
- Use `std::shared_mutex` for concurrent reads, exclusive writes
- Readers acquire shared lock: `std::shared_lock`
- Writers acquire exclusive lock: `std::unique_lock`
- Prefer reader-writer locks when reads vastly outnumber writes

## Modern C++ Features for Concurrency

### C++17 Features
- Parallel STL algorithms with execution policies
- `std::shared_mutex` for efficient reader-writer locking
- `std::scoped_lock` for deadlock-free multi-mutex locking
- Structured bindings for unpacking futures/tuples

### C++20 Features
- `std::jthread` with automatic joining and stop tokens
- `std::stop_token`, `std::stop_source`, `std::stop_callback` for cancellation
- Coroutines (`co_await`, `co_return`, `co_yield`)
- `std::latch` and `std::barrier` for thread synchronization
- `std::counting_semaphore` and `std::binary_semaphore`

### C++23 Features (if available)
- `std::move_only_function` for async callbacks
- Enhanced atomic operations and memory model improvements

## RAII and Resource Management

### Thread RAII Wrappers
```cpp
// ✅ Good: RAII thread wrapper
class ThreadRAII {
    std::thread thread;
public:
    template<typename Func>
    explicit ThreadRAII(Func&& func) : thread(std::forward<Func>(func)) {}
    
    ~ThreadRAII() {
        if (thread.joinable()) thread.join();
    }
    
    ThreadRAII(ThreadRAII&&) noexcept = default;
    ThreadRAII& operator=(ThreadRAII&&) noexcept = default;
    ThreadRAII(const ThreadRAII&) = delete;
    ThreadRAII& operator=(const ThreadRAII&) = delete;
};
```

### Lock Guards
```cpp
// ✅ Always use RAII lock guards
void ProcessData() {
    std::lock_guard<std::mutex> lock(dataMutex);
    // Critical section
} // Automatic unlock

// ✅ Multiple mutex locking
void Transfer() {
    std::scoped_lock lock(mutex1, mutex2); // C++17, deadlock-free
    // Critical section
}

// ✅ Conditional locking
void TryProcess() {
    std::unique_lock lock(dataMutex, std::try_to_lock);
    if (lock.owns_lock()) {
        // Got the lock
    }
}
```

## Testing and Debugging

- Write stress tests that run many iterations with multiple threads
- Use ThreadSanitizer (`-fsanitize=thread`) to detect data races
- Use AddressSanitizer to detect memory errors
- Test with different thread counts and timing scenarios
- Add logging with thread IDs for debugging: `std::this_thread::get_id()`
- Use tools: Valgrind's Helgrind, Intel Inspector, gdb/lldb

## Documentation Requirements

- Document thread-safety guarantees: thread-safe, not thread-safe, conditionally thread-safe
- Document which methods can be called concurrently
- Document synchronization mechanisms used
- Specify whether functions block or are non-blocking
- Document exception behavior in concurrent contexts
- Specify ownership and lifetime of shared resources

When reviewing my C++ code, identify these issues and suggest improvements that follow these best practices for safe, efficient, and maintainable concurrent code.
