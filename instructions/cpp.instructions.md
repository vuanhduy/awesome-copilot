---
description: 'Guidelines for building C++ applications and libraries'
applyTo: '**/*.cpp, **/*.h, **/*.hpp, **/*.cc, **/*.cxx'
---

# C++ Development

## C++ Standards and Language Features

- Always use modern C++ standards: C++11, C++14, C++17, and C++20.
- Prefer C++17 or C++20 features when available unless project compatibility requirements dictate otherwise.
- Use auto keyword to deduce types where it improves readability.
- Leverage move semantics and perfect forwarding for optimal performance.
- Prefer constexpr for compile-time evaluation when applicable.
- Use std::optional and std::variant instead of raw pointers or sentinel values for optional types.
- Employ structured bindings (C++17) for tuple and array unpacking.
- Use concepts (C++20) to constrain template parameters for better error messages and code clarity.

## General Instructions

- Make only high confidence suggestions when reviewing code changes.
- Write code with good maintainability practices, including comments on why certain design decisions were made.
- Handle edge cases and write clear exception handling.
- For libraries or external dependencies, mention their usage and purpose in comments.
- Follow RAII (Resource Acquisition Is Initialization) principles strictly.
- Use smart pointers (std::unique_ptr, std::shared_ptr) instead of raw pointers.
- Avoid manual memory management whenever possible.

## Naming Conventions

- Use snake_case for variable and function names (e.g., `calculate_total`, `user_count`).
- Use PascalCase for class and struct names (e.g., `UserService`, `DataProcessor`).
- Use UPPER_SNAKE_CASE for constants and macros (e.g., `MAX_BUFFER_SIZE`, `DEFAULT_TIMEOUT`).
- Prefix member variables with `m_` for clarity (e.g., `m_data`, `m_callback`).
- Use `is_`, `has_`, `can_` prefixes for boolean variables and functions (e.g., `is_valid`, `has_error`).
- Avoid cryptic abbreviations; prefer clarity over brevity.
- Use descriptive names for template parameters (e.g., `Container`, `Allocator` instead of `T`, `U`).

## Formatting and Code Style

- Follow the `.clang-format` configuration if present in the project.
- Use 4-space indentation (or configure as per project standards).
- Maintain consistent brace style: place opening braces on the same line (K&R style) or opening brace on new line (Allman style) depending on project convention.
- Line length: aim for 80-120 characters; break long lines logically.
- Use one declaration per line (avoid `int a, b, c;`).
- Prefer initializer lists over assignment in constructors.
- Use nullptr instead of NULL or 0 for null pointers.
- Add spaces around binary operators and after control flow keywords (if, for, while).
- Use const and constexpr liberally to communicate intent and enable compiler optimizations.
- Declare variables close to where they are used, minimizing scope.

## Header Files and Organization

- Use `#pragma once` or traditional include guards to prevent multiple inclusion.
- Separate declarations (headers) from implementations (source files).
- Header files should contain class declarations, function prototypes, and inline implementations.
- Keep header files clean: avoid unnecessary includes; use forward declarations when possible.
- Group related declarations together logically.
- Use namespace to organize code and avoid global name pollution.
- Provide consistent header file extensions: `.h` for C-compatible headers, `.hpp` for C++-only headers.
- Include a brief comment at the top of each header explaining its purpose.

## Modern C++ Features and Best Practices

### Smart Pointers
- Use `std::unique_ptr` for exclusive ownership.
- Use `std::shared_ptr` for shared ownership (sparingly).
- Avoid raw pointers for memory ownership; use smart pointers instead.
- Use `std::make_unique()` and `std::make_shared()` for construction.

### STL Containers and Algorithms
- Prefer STL containers (vector, deque, map, unordered_map, set) over raw arrays.
- Use range-based for loops: `for (auto& item : container)`.
- Leverage STL algorithms (std::find, std::sort, std::transform) instead of manual loops.
- Use std::string and std::string_view instead of C-style char arrays.
- Prefer std::array for fixed-size arrays over C-style arrays.

### Error Handling
- Use exceptions for error handling; design exception hierarchies.
- Avoid exception specifications and noexcept unless you know the guarantee you can provide.
- Use std::optional for operations that may fail without throwing.
- Use std::expected (or equivalent) for operations returning error codes with results.
- Catch exceptions by const reference: `catch (const std::exception& e)`.
- Never throw in destructors; use noexcept on destructors.

### Concurrency
- Use std::thread for thread creation and management.
- Employ std::mutex and std::lock_guard (or std::unique_lock) for synchronization.
- Use std::atomic for thread-safe atomic operations without explicit locking.
- Leverage std::future and std::promise for asynchronous operations.
- Use std::condition_variable for signaling between threads.
- Implement proper synchronization to avoid race conditions and deadlocks.
- Consider std::jthread (C++20) for automatic thread cleanup.

### Templates and Generic Programming
- Write generic code using templates; avoid code duplication.
- Use template specialization when behavior needs to differ for specific types.
- Employ SFINAE or concepts (C++20) to control template instantiation.
- Document template requirements clearly (expected member functions, operators, etc.).
- Use variadic templates for flexible, type-safe variable argument functions.

## Project Setup and Structure

- Organize projects with clear directory structure: `src/`, `include/`, `test/`, `cmake/`, `docs/`.
- Use CMake as the build system; provide clear CMakeLists.txt files.
- Separate library code from application code.
- Create namespaces matching directory structure for logical organization.
- Provide clear README documentation explaining build instructions and dependencies.
- Use git for version control with meaningful commit messages.

## Build System (CMake)

- Provide a modern CMakeLists.txt with clear targets and dependencies.
- Specify C++ standard requirement: `set(CMAKE_CXX_STANDARD 17)` or higher.
- Use target-based commands (target_include_directories, target_link_libraries) instead of directory-based.
- Define clear public, private, and interface requirements for libraries.
- Use find_package() for external dependencies (Boost, etc.).
- Implement proper compiler flags for warnings and optimizations.
- Support both Debug and Release build configurations.
- Document build options and their purposes in comments or README.

## Package Management

- For Boost dependencies: document which specific Boost libraries are used.
- Use vcpkg, Conan, or similar package managers for dependency management.
- Specify version constraints for dependencies to ensure compatibility.
- Document installation instructions for all external dependencies.
- Keep dependency tree minimal to reduce complexity and build time.
- Consider the licensing implications of added dependencies.

## Memory Management and RAII

- Always follow RAII: resources acquired in constructor, released in destructor.
- Use smart pointers exclusively; raw pointers only for non-owning references.
- Implement move constructors and move assignment operators for large objects.
- Use std::move() to transfer ownership of expensive resources.
- Avoid manual new/delete; use make_unique and make_shared.
- Ensure destructors are exception-safe and non-throwing (mark with noexcept).
- Use placement new only when necessary and document it clearly.

## Concurrency and Threading

- Design for thread safety from the start; don't retrofit it later.
- Minimize critical sections locked by mutexes.
- Use higher-level abstractions (thread pools, task queues) over raw threads when appropriate.
- Avoid global state; pass data through function parameters.
- Use thread-local storage (thread_local) judiciously; document its use.
- Implement proper synchronization mechanisms to prevent race conditions.
- Test concurrent code extensively; use tools like ThreadSanitizer.
- Document thread safety guarantees clearly (thread-safe, not thread-safe, etc.).

## Exception Handling and Error Handling

- Define a clear exception hierarchy with a base exception class.
- Throw exceptions for exceptional conditions; use return codes only when semantically appropriate.
- Provide informative error messages in exceptions.
- Use RAII and exception safety guarantees (strong, basic, nothrow).
- Avoid catching and ignoring exceptions; handle them meaningfully.
- Document which functions throw exceptions and under what conditions.
- Use std::optional or std::expected for operations that may fail gracefully.
- Never throw in destructors, move operations, or functions marked noexcept.

## Validation and Input Handling

- Validate all inputs at entry points; assume nothing.
- Check for null pointers and invalid ranges.
- Use assertions (assert macro) for internal invariants that should never fail in correct code.
- Use exceptions or return codes for runtime errors caused by external input.
- Implement bounds checking for arrays and container access.
- Sanitize and validate data received from external sources.
- Use std::string and std::string_view to safely handle text data.

## Testing

- Write unit tests using a testing framework (Google Test, Catch2, etc.).
- Aim for high code coverage on critical paths and error-handling code.
- Test both success and failure scenarios.
- Use mocking frameworks (Google Mock) to isolate units under test.
- Test concurrent code with stress tests and multiple iterations.
- Include integration tests for interactions between components.
- Write tests that are independent and can run in any order.
- Document test purposes and expected behaviors in test names.

## Performance Optimization

- Profile code before optimizing; identify actual bottlenecks.
- Use move semantics to avoid unnecessary copies.
- Leverage compile-time computation (constexpr) where possible.
- Use inline functions and template specialization for performance-critical code.
- Prefer stack allocation over heap allocation when sizes are known.
- Use std::vector instead of std::list unless you need frequent insertions/deletions.
- Implement caching and memoization for expensive computations.
- Consider SIMD operations for data-parallel workloads.
- Monitor memory usage and avoid memory leaks (use tools like Valgrind or AddressSanitizer).
- Use link-time optimization (LTO) and profile-guided optimization (PGO) when available.

## Documentation and Comments

- Write clear comments explaining the "why", not the "what" (code shows what it does).
- Document public APIs with clear descriptions of parameters, return values, and exceptions.
- Include code examples in documentation for complex functionality.
- Document thread safety guarantees for public APIs.
- Explain non-obvious design decisions or workarounds.
- Use doxygen-style comments for auto-generated documentation.
- Keep comments up-to-date with code changes.
- Avoid over-commenting obvious code; let clear naming speak for itself.

## Debugging and Diagnostics

- Implement comprehensive logging for debugging and troubleshooting.
- Use conditional compilation (ifdef DEBUG) for debug-only code and logging.
- Leverage debugging tools: gdb, lldb, Valgrind, AddressSanitizer, ThreadSanitizer.
- Implement assertions extensively; use assert() for internal invariants.
- Provide clear error messages and stack traces for debugging.
- Consider implementing a logging framework (spdlog, boost::log) for production code.
- Document known issues and workarounds.

## Security Best Practices

- Validate and sanitize all external input.
- Avoid buffer overflows: use std::string, std::vector, bounds checking.
- Use secure functions: prefer std::string over strcpy, strncpy.
- Avoid hardcoding secrets in code; use environment variables or secure storage.
- Implement proper access control and authentication when applicable.
- Keep dependencies updated to include security patches.
- Use static analysis tools (clang-analyzer, cppcheck) to find security issues.
- Implement secure error handling that doesn't leak sensitive information.

## Continuous Integration and Deployment

- Provide clear build instructions and CMake configuration.
- Ensure code builds cleanly without warnings on multiple compilers (GCC, Clang, MSVC).
- Run automated tests on every commit.
- Use continuous integration (CI) pipelines to catch issues early.
- Implement static analysis checks in CI pipeline.
- Maintain multiple build configurations: Debug, Release, with/without sanitizers.
- Document deployment procedures for libraries and applications.
- Keep changelog and version information up-to-date.

## Multi-Platform Development

- Use platform-agnostic libraries and standard library features.
- Abstract platform-specific code into separate files or modules.
- Test on multiple platforms: Linux, Windows, macOS.
- Use cross-compilation when targeting different architectures.
- Document platform-specific requirements and limitations.
- Use conditional compilation judiciously; prefer runtime detection when possible.
- Ensure line endings and file paths are handled correctly across platforms.

## Boost Library Usage

- Document which Boost libraries are used and their purposes.
- Use Boost.Smart_ptr (std::unique_ptr, std::shared_ptr now in std).
- Leverage Boost.Asio for networking and asynchronous I/O.
- Use Boost.Thread (std::thread now in std) or std::thread for threading.
- Utilize Boost.Test for testing frameworks.
- Use Boost.System for error codes and system errors.
- Document any version-specific Boost usage.
- Keep Boost dependency versions consistent across team.

## Model Context Protocol (MCP) Server Development

### Protocol Implementation
- Follow the MCP specification for message format and protocol compliance.
- Use JSON-RPC 2.0 for message transport with proper request/response pairing.
- Implement stdio-based transport with proper message framing.
- Handle concurrent requests with appropriate synchronization.
- Implement proper error responses with meaningful error codes and messages.

### Tool Definition
- Define tools with clear, descriptive names and purposes.
- Provide comprehensive descriptions for LLM understanding.
- Document input parameters with types, constraints, and examples.
- Use JSON Schema for input validation.
- Implement robust input validation before tool execution.
- Provide meaningful error messages for tool failures.

### Resource Management
- Define resource URIs with clear naming conventions (e.g., `file://`, `db://`).
- Implement resource listing with proper pagination if needed.
- Support streaming for large resources to manage memory efficiently.
- Implement proper resource caching to improve performance.
- Document resource lifecycle and versioning.

### Prompt Templates
- Design reusable prompt templates with clear parameters.
- Provide default parameter values where applicable.
- Make prompts LLM-friendly and easy to consume.
- Document template purpose and usage.
- Support dynamic prompt generation based on context.

### Async & Concurrency
- Design for async operation from the start using callbacks, futures, or coroutines.
- Implement proper cancellation handling for long-running operations.
- Use thread-safe message queue patterns for request distribution.
- Test thoroughly with concurrent access patterns.
- Use ThreadSanitizer to detect and prevent race conditions.

### Logging & Diagnostics
- Implement structured logging to stderr for MCP servers.
- Use appropriate log levels (trace, debug, info, warn, error).
- Log all incoming requests and outgoing responses.
- Include request IDs for tracing distributed calls.
- Support configurable log levels for debugging.

### Security Best Practices
- Validate all input parameters against expected schema.
- Implement access control for sensitive resources and tools.
- Use environment variables or secure vaults for credentials (never hardcode).
- Implement rate limiting for resource-intensive operations.
- Log security-relevant events for audit purposes.
- Consider process isolation if tools execute untrusted code.

## Advanced C++ Patterns

### Type Traits and SFINAE
- Use type traits (std::enable_if, std::is_*) to select implementations at compile time.
- Leverage SFINAE for template specialization based on type properties.
- Prefer concepts (C++20) over SFINAE for clearer template constraints.
- Document template constraints and requirements clearly.

### Custom Allocators
- Implement custom allocators only when necessary (e.g., memory pooling).
- Use polymorphic allocators (std::pmr) for flexibility.
- Document allocation patterns and memory management strategy.
- Profile to ensure custom allocators improve performance.

### Expression Templates
- Use expression templates for domain-specific optimization (e.g., matrix operations).
- Document the purpose and limitations of expression template implementations.
- Ensure expression templates don't introduce memory leaks or dangling references.
- Consider alternatives before implementing expression templates (may impact compile time).

### Policy-Based Design
- Use template policies to provide flexible, compile-time configuration.
- Document policy requirements and interfaces clearly.
- Prefer simpler approaches (inheritance, composition) if policies make code harder to understand.
- Test policies thoroughly with various implementations.

## API Design Best Practices

### Header-Only Libraries
- Use header-only libraries judiciously (may impact compile time).
- Provide inline implementations for simple, performance-critical code.
- Use #pragma once or traditional include guards consistently.
- Document header-only library limitations and requirements.
- Consider separating interface from implementation for large libraries.

### Public API Stability
- Keep public APIs stable; plan for versioning from the start.
- Use opaque pointers or PIMPL to hide implementation details.
- Avoid exposing implementation-dependent types in public interfaces.
- Document API contracts and stability guarantees.
- Provide deprecation warnings for deprecated APIs.

### Namespace Organization
- Organize code hierarchically using nested namespaces.
- Use short, meaningful namespace names (e.g., `mylib::core`, `mylib::io`).
- Avoid namespace pollution; hide implementation details in anonymous namespaces or detail namespaces.
- Provide `using` directives in implementation only, not in headers.
- Document namespace organization and module relationships.

### Error Code Management
- Define error codes as enums or error code categories.
- Use std::error_code and std::error_category for integration with std::exception.
- Provide clear error messages for each error code.
- Document error handling strategy and expected error conditions.
- Consider using std::expected or similar for function return values.

## Code Review Checklist

### Architecture & Design
- [ ] Design is modular with clear separation of concerns
- [ ] API is intuitive and well-documented
- [ ] Classes follow single responsibility principle
- [ ] Dependencies are minimal and well-managed
- [ ] Design patterns are applied appropriately

### Modern C++ & Standards
- [ ] Uses C++17/20 features appropriately
- [ ] Leverages std::optional, std::variant, std::expected
- [ ] Uses range-based for loops and STL algorithms
- [ ] Implements move semantics for efficiency
- [ ] Proper use of const and constexpr

### Memory & Resource Management
- [ ] Smart pointers used exclusively (no raw pointer ownership)
- [ ] RAII principles followed for all resources
- [ ] No memory leaks or dangling pointers
- [ ] Move constructors and move assignment operators implemented
- [ ] Destructors are exception-safe (noexcept)

### Thread Safety & Concurrency
- [ ] Thread safety guarantees documented
- [ ] Proper synchronization mechanisms used
- [ ] No data races or deadlocks
- [ ] Tested with ThreadSanitizer
- [ ] Cancellation handling implemented

### Error Handling & Exceptions
- [ ] Comprehensive error handling strategy
- [ ] Exception safety guarantees specified (strong/basic/nothrow)
- [ ] Clear error messages and logging
- [ ] Edge cases handled gracefully
- [ ] No silent failures or undefined behavior

### Testing & Quality
- [ ] Unit tests cover critical paths
- [ ] Edge cases and error paths tested
- [ ] Performance benchmarks for critical code
- [ ] Concurrency tests for multi-threaded code
- [ ] Code follows style guidelines and passes linters

### Documentation
- [ ] Public APIs documented with Doxygen
- [ ] Parameters, return values, and exceptions documented
- [ ] Usage examples provided
- [ ] Complex algorithms explained
- [ ] Performance characteristics noted
