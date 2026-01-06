---
mode: 'agent'
description: 'Review C++ code for design pattern implementation and suggest improvements based on modern C++ best practices.'
tools: ['search/codebase', 'edit/editFiles', 'problems']
---

# C++ Design Pattern Review

Review the C++ code in ${selection} for design pattern implementation and suggest improvements for the solution/project. Do not make any changes to the code, just provide a comprehensive review.

## Required Modern C++ Design Patterns

### Creational Patterns
- **Factory Pattern**: Smart pointer factories, abstract factories, template factories with `std::unique_ptr`/`std::shared_ptr`
- **Builder Pattern**: Fluent interfaces, parameter object patterns, construction without constructor overloading
- **Singleton Pattern**: Meyer's Singleton (thread-safe), modern implementation without raw pointers
- **Object Pool Pattern**: Resource reuse for expensive object creation, connection/thread pooling

### Structural Patterns
- **Adapter Pattern**: Interface adaptation, inheritance vs composition
- **Bridge Pattern**: Implementation abstraction, PIMPL (Pointer to Implementation)
- **Composite Pattern**: Tree hierarchies, recursive composition with smart pointers
- **Decorator Pattern**: Compile-time vs runtime decoration, template-based composition
- **Facade Pattern**: Simplified interfaces to complex subsystems
- **Proxy Pattern**: Lazy loading, virtual proxies, smart proxy wrappers

### Behavioral Patterns
- **Chain of Responsibility**: Command chains, request handlers, middleware patterns
- **Command Pattern**: Operation encapsulation, undo/redo implementations, command queues
- **Iterator Pattern**: STL iterator design, custom iterators for containers
- **Mediator Pattern**: Centralized communication, event dispatchers
- **Memento Pattern**: State snapshots, serialization, undo mechanisms
- **Observer Pattern**: Event systems, pub/sub patterns, signal/slot mechanisms
- **State Pattern**: State machines, hierarchical states, context switching
- **Strategy Pattern**: Algorithm selection at runtime, policy-based design
- **Template Method Pattern**: Base class algorithms, virtual function hooks
- **Visitor Pattern**: Double dispatch, type-safe operations on heterogeneous collections

### Concurrency Patterns
- **Active Object Pattern**: Asynchronous method execution with futures/promises
- **Monitor Object Pattern**: Thread-safe object access with mutex guards
- **Thread Pool Pattern**: Worker threads, task queues, async execution
- **Double-Checked Locking**: Lazy initialization with minimal locking
- **Read-Write Lock Pattern**: Concurrent read access with exclusive write access
- **Producer-Consumer Pattern**: Synchronized queues, thread synchronization

### Resource Management Patterns
- **RAII (Resource Acquisition Is Initialization)**: Constructor/destructor resource management
- **Smart Pointers**: `std::unique_ptr`, `std::shared_ptr`, `std::weak_ptr` usage
- **Copy-and-Swap**: Exception-safe assignment
- **Pimpl (Pointer to Implementation)**: Implementation hiding with smart pointers
- **CRTP (Curiously Recurring Template Pattern)**: Static polymorphism, compile-time customization

## Code Review Checklist

### Design Pattern Implementation
- [ ] **Creational Patterns**: Are Factory, Builder, Singleton patterns correctly implemented?
- [ ] **Structural Patterns**: Are Adapter, Bridge, Composite patterns used appropriately?
- [ ] **Behavioral Patterns**: Are Command, Observer, State, Strategy patterns well-designed?
- [ ] **Concurrency Patterns**: Are thread-safe patterns (Monitor Object, Thread Pool) properly implemented?
- [ ] **Resource Patterns**: Is RAII strictly followed? Smart pointers used for ownership?

### Modern C++ Best Practices (C++17/20)
- [ ] **Smart Pointers**: `std::unique_ptr` for exclusive ownership, `std::shared_ptr` for shared ownership?
- [ ] **Move Semantics**: Move constructors/assignment operators implemented for efficiency?
- [ ] **Range-Based Loops**: Using `for (auto& item : container)` instead of iterators?
- [ ] **Auto Type Deduction**: `auto` used to reduce verbosity while maintaining readability?
- [ ] **constexpr**: Compile-time computation used where applicable?
- [ ] **std::optional/std::variant**: Used instead of raw pointers or sentinel values?
- [ ] **Structured Bindings**: C++17 bindings for tuple/pair unpacking?
- [ ] **Concepts**: C++20 concepts used to constrain templates?

### Memory Management & RAII
- [ ] **No Raw Pointers**: Raw pointers only used for non-owning references?
- [ ] **Resource Cleanup**: All resources (files, locks, memory) properly managed?
- [ ] **Move-Only Types**: Classes with movable but non-copyable semantics properly defined?
- [ ] **Exception Safety**: Strong exception safety guarantees where possible?
- [ ] **Destructor Design**: Destructors are non-throwing and properly mark with `noexcept`?

### Thread Safety & Concurrency
- [ ] **Synchronization**: Proper use of mutex, condition_variable, atomic types?
- [ ] **Data Races**: No unprotected shared data access?
- [ ] **Deadlock Prevention**: Lock ordering consistent, RAII lock guards used?
- [ ] **Async Patterns**: std::future, std::promise used for async operations?
- [ ] **Thread Management**: std::thread/std::jthread properly managed and joined?

### Architecture & Design
- [ ] **Separation of Concerns**: Clear module boundaries and responsibilities?
- [ ] **Namespace Organization**: Logical namespace hierarchy matching module structure?
- [ ] **Header/Implementation**: Proper separation of interface from implementation?
- [ ] **Const-Correctness**: const methods, const parameters, const data members?
- [ ] **Encapsulation**: Private implementation details, public interfaces well-defined?

### SOLID Principles
- [ ] **Single Responsibility**: Each class has one reason to change?
- [ ] **Open/Closed**: Classes open for extension, closed for modification?
- [ ] **Liskov Substitution**: Derived classes properly substitute base classes?
- [ ] **Interface Segregation**: Specific interfaces instead of fat interfaces?
- [ ] **Dependency Inversion**: Depend on abstractions, not concrete implementations?

### Performance Optimization
- [ ] **Unnecessary Copies**: Avoiding copies via move semantics and references?
- [ ] **Memory Allocation**: Stack allocation preferred over heap where possible?
- [ ] **Inline Functions**: Hot path functions properly inlined?
- [ ] **Template Optimization**: Template specialization for performance-critical code?
- [ ] **Resource Pooling**: Reusing expensive resources instead of repeated creation?

### Error Handling
- [ ] **Exception Strategy**: Consistent exception handling approach throughout?
- [ ] **Error Messages**: Clear, informative exception messages with context?
- [ ] **Error Codes**: When using error codes, are they meaningful and documented?
- [ ] **Validation**: Input validation at boundaries, precondition checks?
- [ ] **RAII Error Handling**: Using RAII for cleanup even in error paths?

### Code Quality & Maintainability
- [ ] **Naming Conventions**: Clear, descriptive names following project conventions (snake_case, m_ prefixes)?
- [ ] **Documentation**: Doxygen comments for public APIs with parameter/return documentation?
- [ ] **Code Duplication**: Unnecessary duplication eliminated through abstraction?
- [ ] **Complexity**: Methods/functions appropriately sized, single responsibility?
- [ ] **Formatting**: Consistent indentation, brace placement, whitespace (follow .clang-format)?

### Testing & Testability
- [ ] **Dependency Injection**: Dependencies injected or mockable?
- [ ] **Separation of Logic**: Business logic separated from I/O and system calls?
- [ ] **Test Compatibility**: Code designed to be easily tested?
- [ ] **Edge Cases**: Handling of boundary conditions and error cases?

### Security Best Practices
- [ ] **Input Validation**: All external input validated and sanitized?
- [ ] **Buffer Overflows**: No unsafe string/array operations (use std::string, std::vector)?
- [ ] **Resource Limits**: Protection against resource exhaustion attacks?
- [ ] **Credential Handling**: Secrets never hardcoded, secure handling in place?
- [ ] **Secure Communication**: Encryption/secure protocols for sensitive data?

### C++ Standard Compliance
- [ ] **C++ Version**: Code uses features appropriate to declared standard (C++17/20)?
- [ ] **Deprecation**: No use of deprecated features or functions?
- [ ] **Portability**: Code portable across target platforms (Linux, Windows, macOS)?
- [ ] **Compiler Warnings**: Code compiles cleanly with -Wall -Wextra flags?

## Improvement Focus Areas

### Factory Patterns
- Smart pointer factory methods returning `std::unique_ptr` or `std::shared_ptr`
- Abstract factory interfaces with proper virtual destructors
- Avoiding unnecessary dynamic allocation for simple construction

### Builder Patterns
- Fluent interface design for readable configuration
- Default parameter handling without constructor overloading
- Move-optimized builders returning configured objects by value

### PIMPL Pattern
- Implementation hiding with smart pointers (`std::unique_ptr`)
- Reducing compilation dependencies via forward declarations
- Proper move semantics for Pimpl-wrapped classes

### Observer/Event Patterns
- Signal/slot mechanisms for decoupled communication
- Weak pointer usage to prevent circular references
- Safe observer lifetime management

### Concurrency Patterns
- Future/promise usage for async operations
- Thread pool abstractions for worker thread management
- Atomic operations and lock-free algorithms where appropriate
- Thread-safe singleton patterns without raw pointers

### CRTP (Curiously Recurring Template Pattern)
- Static polymorphism to avoid virtual function overhead
- Type-safe customization through template specialization
- Performance-critical code optimization

Provide specific, actionable recommendations for improvements aligned with modern C++ best practices (C++17/20), RAII principles, thread safety, and the project's architecture standards outlined in `cpp.instructions.md`.
