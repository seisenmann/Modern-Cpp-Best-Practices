# Modern C++ Best Practices

Welcome to **Modern C++ Best Practices**, a curated guide to writing clean, efficient, and maintainable C++ code. This document provides practical advice and examples for using C++ effectively, focusing on features introduced in C++11 and later.

---

## Table of Contents
1. [Introduction](#introduction)
2. [General Guidelines](#general-guidelines)
3. [Code Clarity and Safety](#code-clarity-and-safety)
4. [Iterators and Containers](#iterators-and-containers)
5. [Functions and Semantics](#functions-and-semantics)
6. [Concurrency and Thread Safety](#concurrency-and-thread-safety)
7. [Memory Management](#memory-management)
8. [Lambdas and Functional Programming](#lambdas-and-functional-programming)
9. [Task-Based Concurrency](#task-based-concurrency)
10. [Additional Resources](#additional-resources)

---

## Introduction

Modern C++ (C++11 and beyond) offers powerful features and tools to help developers write cleaner, safer, and more performant code. This guide compiles best practices to maximize the benefits of modern C++ while avoiding common pitfalls.

---

## General Guidelines

- **Use `struct` for simple data aggregates**: If a class only contains public data members and no behavior, use `struct` for clarity.
- **Prefer type aliases (`using`) over `typedef`**: Improves readability and consistency.
  ```cpp
  using StringList = std::vector<std::string>;
  ```
- **Mark as `const` whenever possible**: Emphasize immutability for better safety and optimization.
- **Use `constexpr`**: Leverage compile-time evaluation to improve runtime performance.

---

## Code Clarity and Safety

- **Avoid implicit conversions**: Ensure clear and explicit type usage.
- **Use `auto`**: Simplifies type declarations while maintaining readability.
- **Prefer `nullptr` over `0` or `NULL`**: Indicates pointer semantics explicitly.
- **Scoped enums (`enum class`) over unscoped enums**: Prevents naming collisions and improves type safety.
- **Avoid raw `assert` in production**: Use structured error handling or logging for better reliability.
- **Use `std::transform` or similar functions over loops**: Makes code more expressive and reduces boilerplate.
  ```cpp
  std::vector<int> input = {1, 2, 3};
  std::vector<int> output(input.size());
  std::transform(input.begin(), input.end(), output.begin(), [](int x) { return x * 2; });
  ```
- **Use `std::ranges` for cleaner algorithms**: Ranges eliminate the need for manual iterators and allow algorithm composition through a pipeline syntax.

---

## Iterators and Containers

- **Use `cbegin()` and `cend()`**: Signals read-only intent when iterating.
- **Prefer `emplace_back()` over `push_back()`**: Avoids unnecessary copies by constructing elements in place.
- **Prefer `const_iterator` over mutable iterators**: Use `const` wherever mutation is not required.

---

## Functions and Semantics

- **Mark overriding functions with `override`**: Prevents accidental hiding of base class functions.
- **Use `noexcept` for functions that won’t throw exceptions**: Enables compiler optimizations.
- **Pass by value for cheap-to-move parameters**: Avoids unnecessary indirection and improves clarity.
- **Avoid passing by value for types that are expensive to copy**: Such as `std::vector` or `std::string`.
- **Use move semantics and perfect forwarding**: 
  - **`std::move`** casts its argument to an rvalue reference, enabling move operations.
  - **`std::forward`** preserves the value category (lvalue or rvalue) of its argument.
  - **Universal references**: If a function parameter is declared as `T&&` or a variable as `auto&&`, it’s a universal reference. Use `std::forward` for efficient forwarding.
    ```cpp
    template <typename T>
    void process(T&& arg) {
        doWork(std::forward<T>(arg));
    }
    ```

---

## Concurrency and Thread Safety

- **Thread safety**:
  - Use `std::mutex` for multi-variable synchronization.
  - Use `std::atomic` for lightweight synchronization.
- **Task-based concurrency**:
  - Prefer `std::async` over manual thread creation.
  - Always join or detach threads to prevent undefined behavior.
- **Use `std::atomic` over `volatile`**: `std::atomic` is designed for concurrency, while `volatile` is for hardware-specific scenarios.

---

## Memory Management

- **Prefer smart pointers over raw pointers**:
  - Use `std::unique_ptr` for exclusive ownership.
  - Use `std::shared_ptr` for shared ownership.
  - Use `std::weak_ptr` to prevent circular references.
- **Prefer `std::make_unique` and `std::make_shared`**: Avoids manual `new` and ensures exception safety.
- **Follow the Rule of Five**: If your class manages resources, explicitly define or delete:
  1. Destructor
  2. Copy constructor
  3. Copy assignment operator
  4. Move constructor
  5. Move assignment operator

---

## Lambdas and Functional Programming

- **Prefer lambdas over `std::bind`**: More readable and flexible.
- **Embrace lambdas**: Use lambdas for short, inline functions.

---

## Task-Based Concurrency

- **Use `std::async` for simple parallelism**: Prefer `std::launch::async` for guaranteed asynchrony.
  ```cpp
  auto future = std::async(std::launch::async, [] { return doWork(); });
  ```
- **Handle `std::thread` carefully**: Always ensure threads are joined or detached to avoid undefined behavior.

---

## Additional Resources

- [cppreference.com](https://en.cppreference.com)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
- [Effective Modern C++](https://www.amazon.com/Effective-Modern-Specific-Ways-Improve/dp/1491903996) by Scott Meyers

---

## Contributing

Contributions are welcome! If you have suggestions for additional practices or improvements, feel free to submit a pull request or open an issue.
