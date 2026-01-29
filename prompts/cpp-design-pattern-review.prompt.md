---
agent: 'agent'
description: 'Comprehensive C++ design pattern review with modern best practices, architecture validation, and actionable recommendations'
tools: ['changes', 'search/codebase', 'edit/editFiles', 'problems', 'usages']
---

# C++ Design Pattern Review - Enhanced

Review the C++ code in ${selection} for design pattern implementation, architecture quality, and modern best practices. Provide a comprehensive analysis with specific, actionable recommendations.

## Step 0: Context Analysis

**Before reviewing, analyze the code context:**

### System Type:
- **Embedded/IoT System** → Resource constraints, real-time requirements
- **Desktop Application** → GUI patterns, event handling
- **Server/Backend** → Scalability, thread safety, performance
- **Library/Framework** → API design, ABI stability, header-only vs compiled
- **Real-Time System** → Deterministic behavior, lock-free algorithms

### Complexity Level:
- **Simple (<1K LOC)** → Focus on fundamentals, RAII, smart pointers
- **Medium (1K-10K LOC)** → Architecture patterns, modularity
- **Large (>10K LOC)** → Component design, dependency management, build times

### Primary Concerns:
- **Performance-Critical** → Zero-cost abstractions, cache efficiency, inlining
- **Safety-Critical** → Exception safety, resource guarantees, defensive programming
- **Maintainability-First** → Clear abstractions, documentation, testability
- **Legacy Integration** → C compatibility, ABI stability, incremental modernization

### Create Review Plan:
Select 2-3 most relevant focus areas based on context.

## Step 1: Clarification Protocol

**Before proceeding, clarify when:**

**Code Purpose:**
- "What is the primary responsibility of this code/module?"
- "Are there specific performance or safety requirements?"
- "Is this new code or refactoring of existing code?"

**Architecture Context:**
- "How does this fit into the larger system architecture?"
- "Are there existing patterns or conventions to follow?"
- "What are the compile-time and runtime dependencies?"

**Constraints & Trade-offs:**
- "Are there platform-specific requirements (Windows/Linux/embedded)?"
- "What C++ standard are you targeting (C++17/20/23)?"
- "Are there restrictions on heap allocation, exceptions, or RTTI?"

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
- **Double-Checked Locking**: Lazy initialization with minimal locking (use std::call_once instead)
- **Read-Write Lock Pattern**: Concurrent read access with exclusive write access (std::shared_mutex)
- **Producer-Consumer Pattern**: Synchronized queues, thread synchronization

### Resource Management Patterns
- **RAII (Resource Acquisition Is Initialization)**: Constructor/destructor resource management
- **Smart Pointers**: `std::unique_ptr`, `std::shared_ptr`, `std::weak_ptr` usage
- **Copy-and-Swap**: Exception-safe assignment
- **Pimpl (Pointer to Implementation)**: Implementation hiding with smart pointers
- **CRTP (Curiously Recurring Template Pattern)**: Static polymorphism, compile-time customization

## Comprehensive Review Checklist

### 1. Design Pattern Implementation
- [ ] **Pattern Identification**: Which patterns are used? Are they correctly implemented?
- [ ] **Pattern Appropriateness**: Are patterns solving actual problems or adding unnecessary complexity?
- [ ] **Missing Patterns**: Would beneficial patterns improve the design (Factory, Strategy, Observer)?
- [ ] **Anti-Patterns**: Are there god objects, excessive coupling, or pattern misuse?
- [ ] **Modern Alternatives**: Can runtime polymorphism be replaced with compile-time alternatives (templates, CRTP)?

### 2. Modern C++ Best Practices (C++17/20/23)
- [ ] **Smart Pointers**: Ownership clearly expressed through `std::unique_ptr`/`std::shared_ptr`/`std::weak_ptr`?
- [ ] **Move Semantics**: Move constructors/assignment with `noexcept` specification for efficiency?
- [ ] **Range-Based Loops**: Using `for (auto& item : container)` for readability?
- [ ] **Auto Type Deduction**: `auto` used appropriately without obscuring types?
- [ ] **constexpr**: Compile-time computation for constants and simple functions?
- [ ] **std::optional/std::variant**: Modern alternatives to raw pointers and unions?
- [ ] **Structured Bindings**: C++17 bindings for tuple/pair unpacking?
- [ ] **Concepts**: C++20 concepts constraining templates for better error messages?
- [ ] **Ranges**: C++20 ranges and views for composable algorithms?
- [ ] **Coroutines**: C++20 coroutines for async operations (if applicable)?

### 3. Memory Management & RAII
- [ ] **Zero Raw Pointers**: Raw pointers only for non-owning references?
- [ ] **Resource Cleanup**: All resources (files, sockets, locks, memory) properly managed?
- [ ] **Move-Only Types**: Classes with expensive resources correctly movable but non-copyable?
- [ ] **Exception Safety**: Strong or basic exception safety guarantees?
- [ ] **Destructor Design**: Destructors non-throwing and marked `noexcept`?
- [ ] **Custom Deleters**: Smart pointers use custom deleters for special cleanup?
- [ ] **Stack vs Heap**: Prefer stack allocation, use heap only when necessary?

### 4. Thread Safety & Concurrency
- [ ] **Synchronization Primitives**: Proper use of `std::mutex`, `std::shared_mutex`, `std::atomic`?
- [ ] **Data Races**: No unprotected shared mutable state?
- [ ] **Deadlock Prevention**: Lock ordering consistent, RAII lock guards (`std::lock_guard`, `std::unique_lock`)?
- [ ] **Async Patterns**: `std::async`, `std::future`, `std::promise` for async operations?
- [ ] **Thread Management**: `std::thread`/`std::jthread` properly joined or detached?
- [ ] **Lock-Free Algorithms**: Appropriate use of atomics for performance-critical paths?
- [ ] **Thread-Local Storage**: `thread_local` for per-thread data?
- [ ] **Condition Variables**: Proper use with predicate functions to avoid spurious wakeups?

### 5. Architecture & Design
- [ ] **Separation of Concerns**: Clear module boundaries and single responsibilities?
- [ ] **Namespace Organization**: Logical hierarchy matching module structure?
- [ ] **Header/Implementation**: Proper separation, minimize dependencies in headers?
- [ ] **Interface Design**: Abstract interfaces for polymorphism, concrete classes for implementations?
- [ ] **Const-Correctness**: `const` methods, parameters, and member variables where appropriate?
- [ ] **Encapsulation**: Private implementation details, well-defined public interfaces?
- [ ] **Dependency Direction**: High-level modules not depending on low-level details?
- [ ] **Component Coupling**: Low coupling between modules, high cohesion within modules?

### 6. SOLID Principles
- [ ] **Single Responsibility**: Each class has one reason to change?
- [ ] **Open/Closed**: Classes open for extension (inheritance, templates) but closed for modification?
- [ ] **Liskov Substitution**: Derived classes properly substitute base classes without surprises?
- [ ] **Interface Segregation**: Focused interfaces instead of fat interfaces with many methods?
- [ ] **Dependency Inversion**: Depend on abstractions (interfaces) not concrete implementations?

### 7. Performance Optimization
- [ ] **Unnecessary Copies**: Avoiding copies via move semantics, references, and `std::move`?
- [ ] **Memory Allocation**: Minimize heap allocations, prefer stack when possible?
- [ ] **Inline Functions**: Hot path functions marked `inline` or in headers for inlining?
- [ ] **Template Optimization**: Template specialization for performance-critical types?
- [ ] **Resource Pooling**: Reusing expensive resources (connections, threads) instead of repeated creation?
- [ ] **Cache Efficiency**: Data layout optimized for cache (contiguous arrays, struct packing)?
- [ ] **Virtual Function Overhead**: CRTP or templates preferred over virtual functions in hot paths?
- [ ] **String Handling**: `std::string_view` for non-owning string references?

### 8. Error Handling
- [ ] **Exception Strategy**: Consistent approach - exceptions vs error codes?
- [ ] **Error Messages**: Clear, informative exception messages with context?
- [ ] **Exception Safety**: Strong guarantees where possible, basic guarantees elsewhere?
- [ ] **Error Codes**: If used, are they meaningful, documented, and checked?
- [ ] **Validation**: Input validation at boundaries, precondition checks?
- [ ] **RAII Error Handling**: Automatic cleanup even in error paths?
- [ ] **noexcept Specification**: Functions marked `noexcept` where appropriate for optimization?
- [ ] **std::expected**: C++23 `std::expected` for operations that can fail?

### 9. Code Quality & Maintainability
- [ ] **Naming Conventions**: Clear, descriptive names following project style (snake_case, camelCase, m_ prefix)?
- [ ] **Documentation**: Doxygen/inline comments for public APIs with parameters/returns?
- [ ] **Code Duplication**: DRY principle - eliminate duplication through abstraction?
- [ ] **Function Size**: Functions focused on single task, typically <50 lines?
- [ ] **Class Complexity**: Classes with clear responsibilities, not god objects?
- [ ] **Formatting**: Consistent style (.clang-format), proper indentation and spacing?
- [ ] **Magic Numbers**: Constants named and defined, not hardcoded literals?
- [ ] **Self-Documenting Code**: Code intent clear from names and structure?

### 10. Testing & Testability
- [ ] **Dependency Injection**: Dependencies injectable via constructors/setters for mocking?
- [ ] **Interface Abstraction**: Concrete implementations behind interfaces for substitution?
- [ ] **Separation of Logic**: Business logic separated from I/O, system calls, and framework code?
- [ ] **Test Hooks**: Seams for test doubles without requiring friendship?
- [ ] **Pure Functions**: Logic in pure functions where possible for easy unit testing?
- [ ] **Edge Cases**: Handling of null pointers, empty containers, boundary conditions?

### 11. Security Best Practices
- [ ] **Input Validation**: All external input validated and sanitized?
- [ ] **Buffer Safety**: No unsafe operations - use `std::string`, `std::vector`, `std::array`, `std::span`?
- [ ] **Integer Overflow**: Checks for arithmetic overflows in sensitive operations?
- [ ] **Resource Limits**: Protection against resource exhaustion (memory, file handles)?
- [ ] **Credential Handling**: No hardcoded secrets, secure credential storage?
- [ ] **Secure Communication**: TLS/encryption for sensitive data transmission?
- [ ] **Randomness**: Use `<random>` facilities, not `rand()` for security-sensitive operations?

### 12. C++ Standard Compliance & Portability
- [ ] **Standard Version**: Code uses features appropriate to declared standard (C++17/20/23)?
- [ ] **Deprecation**: No deprecated features (`auto_ptr`, `bind1st`, `random_shuffle`)?
- [ ] **Portability**: Code portable across target platforms (Linux, Windows, macOS)?
- [ ] **Compiler Warnings**: Code compiles cleanly with `-Wall -Wextra -Wpedantic`?
- [ ] **ABI Stability**: If library, ABI considerations for binary compatibility?
- [ ] **Build System**: CMake or equivalent with proper dependency management?

## Improvement Focus Areas with Examples

### 1. Factory Pattern Modernization
**Issue**: Raw pointer factories
```cpp
// ❌ Bad
Base* CreateObject(Type type) {
    if (type == Type::A) return new DerivedA();
    return new DerivedB();
}

// ✅ Good
std::unique_ptr<Base> CreateObject(Type type) {
    if (type == Type::A) return std::make_unique<DerivedA>();
    return std::make_unique<DerivedB>();
}
```

### 2. RAII and Resource Management
**Issue**: Manual resource management
```cpp
// ❌ Bad
void Process() {
    auto* file = fopen("data.txt", "r");
    // ... processing ...
    fclose(file); // Might be skipped on exception
}

// ✅ Good
void Process() {
    auto file = std::unique_ptr<FILE, decltype(&fclose)>(
        fopen("data.txt", "r"), &fclose);
    // ... processing ... (automatic cleanup)
}
// Or better: std::ifstream
```

### 3. PIMPL Pattern with Smart Pointers
**Issue**: Pimpl without proper implementation
```cpp
// ❌ Bad - header
class Widget {
    Impl* pImpl; // Raw pointer
public:
    Widget();
    ~Widget(); // Needs manual delete
};

// ✅ Good - header
class Widget {
    std::unique_ptr<Impl> pImpl;
public:
    Widget();
    ~Widget(); // Declared but defined in .cpp
    Widget(Widget&&) noexcept;
    Widget& operator=(Widget&&) noexcept;
};
```

### 4. Thread-Safe Singleton
**Issue**: Non-thread-safe singleton
```cpp
// ❌ Bad
class Singleton {
    static Singleton* instance;
    Singleton() = default;
public:
    static Singleton* GetInstance() {
        if (!instance) instance = new Singleton(); // Race condition
        return instance;
    }
};

// ✅ Good - Meyer's Singleton
class Singleton {
    Singleton() = default;
public:
    static Singleton& GetInstance() {
        static Singleton instance; // Thread-safe in C++11+
        return instance;
    }
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
};
```

### 5. Observer Pattern with Weak Pointers
**Issue**: Circular references with shared_ptr
```cpp
// ❌ Bad - causes memory leaks
class Subject {
    std::vector<std::shared_ptr<Observer>> observers;
};

// ✅ Good - breaks cycles
class Subject {
    std::vector<std::weak_ptr<Observer>> observers;
    
    void Notify() {
        for (auto& weak : observers) {
            if (auto obs = weak.lock()) {
                obs->Update();
            }
        }
    }
};
```

### 6. CRTP for Static Polymorphism
**Issue**: Runtime overhead from virtual functions
```cpp
// ❌ Runtime polymorphism (virtual function overhead)
class Base {
public:
    virtual void Process() = 0;
};

// ✅ Good - CRTP (compile-time polymorphism)
template<typename Derived>
class Base {
public:
    void Process() {
        static_cast<Derived*>(this)->ProcessImpl();
    }
};

class Concrete : public Base<Concrete> {
    friend class Base<Concrete>;
    void ProcessImpl() { /* implementation */ }
};
```

## Output Format

Provide your review in the following structure:

### Executive Summary
- Overall code quality assessment (1-2 paragraphs)
- Top 3-5 critical issues or risks
- Quick wins (improvements achievable in 1-2 days)

### Pattern Analysis
- Patterns identified and their implementation quality
- Missing beneficial patterns
- Pattern misuse or over-engineering

### Detailed Findings by Category
For each relevant category from the checklist:
- **Category Score** (1-5): Current state
- **Issues Found**: Specific problems with code examples
- **Recommendations**: Actionable improvements with code snippets
- **Priority**: High/Medium/Low

### Architecture Quality Assessment
- Module organization and dependencies
- Separation of concerns
- Scalability and extensibility considerations
- Technical debt identification

### Code Examples
- **Before/After** comparisons for key improvements
- Reference implementations for suggested patterns
- Best practice examples aligned with project style

### Action Plan
1. **Immediate Actions** (< 1 week): Critical fixes, safety issues
2. **Short-term Improvements** (1-4 weeks): Pattern refactoring, modernization
3. **Long-term Enhancements** (1-3 months): Architecture evolution, technical debt

### References & Resources
- Relevant C++ Core Guidelines
- Modern C++ books/articles
- Project-specific style guides

## Communication Style

- **Clarity First**: Use precise technical language while remaining accessible
- **Balanced Feedback**: Acknowledge good practices alongside improvement areas
- **Actionable Insights**: Every finding includes concrete next steps
- **Pragmatic Approach**: Balance ideal solutions with practical constraints
- **Educational Value**: Explain *why* changes matter, not just *what* to change

## Core Review Principles

1. **Readability First**: Code is read more than written - optimize for understanding
2. **Safety by Design**: Leverage C++ features (RAII, smart pointers) for correctness
3. **Zero-Cost Abstractions**: Modern C++ provides safety without runtime cost
4. **Pragmatic Perfection**: Incremental improvement over perfect rewrites
5. **Test-Driven Quality**: Testable code is maintainable code

Provide specific, actionable recommendations aligned with modern C++ best practices (C++17/20/23), RAII principles, thread safety, SOLID design, and the project's architecture standards as outlined in `.github/copilot-instructions.md`.
