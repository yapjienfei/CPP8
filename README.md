# C++ Module 08 — 42KL

> Templated containers, iterators, and STL algorithms: applying the C++ Standard Template Library to build practical data structure utilities.

## Overview

The C++ Standard Template Library (STL) provides a rich set of containers (`vector`, `list`, `map`, `stack`, `deque`, …), iterators for traversing them uniformly, and algorithms (`find`, `sort`, `count`, …) that work with any container through iterators. Module 08 moves from writing templates to *using* templates — specifically the STL's composable building blocks.

Each exercise solves a real problem using STL containers and algorithms, which teaches idiomatic C++ patterns: using iterators instead of raw indices, composing containers with algorithms, and extending existing containers by inheriting from them.

## The Challenge

- `ex00`: write a function template `easyfind` that locates a value in any STL container using `std::find`
- `ex01`: build a `Span` class that stores up to N integers and can report the shortest and longest span (range between any two elements)
- `ex02`: build `MutantStack` — a `std::stack` extended with full iterator support, enabling it to be used in range-based loops and with STL algorithms

## Concepts Introduced

- **STL iterators**: `begin()`, `end()`, iterator arithmetic, the iterator as the universal interface between containers and algorithms
- **`std::find`**: linear search algorithm that works on any iterator range
- **`std::sort`**: sorting an iterator range in place
- **`std::min_element` / `std::max_element`**: finding extremes in an iterator range
- **Container adapters**: `std::stack` is a wrapper around `std::deque` — inheriting from it exposes the underlying container's iterators
- **`std::distance`**: computing the number of steps between two iterators (works for both random-access and forward iterators)
- **Exception handling**: throwing and catching `std::exception` derived classes for out-of-range and empty-container errors

## Learning Outcomes

After completing this module you will have:
- Used STL algorithms with multiple container types through the iterator interface
- Written a template function that works correctly with `std::list`, `std::vector`, `std::deque`, and any other iterable
- Implemented a container class that exposes iterators (enabling for-each, STL algorithms)
- Understood the container adapter pattern and how `std::stack`, `std::queue`, and `std::priority_queue` are built on top of other containers
- Gained fluency reading and writing idiomatic C++ that leverages the STL

## Exercises

### ex00 — easyfind
`easyfind(container, value)` returns an iterator to the first occurrence of `value` in `container`, or throws an exception if not found.

```cpp
std::vector<int> v = {1, 2, 3, 4, 5};
std::vector<int>::iterator it = ::easyfind(v, 3);
std::cout << *it << "\n";  // 3

// Works with list, deque, or any container with begin()/end()
std::list<int> lst = {10, 20, 30};
::easyfind(lst, 99);  // throws
```

### ex01 — Span
`Span(N)` stores up to N integers (added one at a time with `addNumber`, or in bulk with a range overload). Provides:
- `shortestSpan()` — minimum difference between any two stored values
- `longestSpan()` — maximum difference

```cpp
Span sp(5);
sp.addNumber(5); sp.addNumber(3); sp.addNumber(17); sp.addNumber(9); sp.addNumber(11);

std::cout << sp.shortestSpan() << "\n";  // 2
std::cout << sp.longestSpan()  << "\n";  // 14

// Bulk add from a range
std::vector<int> src = {6, 7, 8};
sp.addNumber(src.begin(), src.end());
```

### ex02 — MutantStack
`std::stack` does not expose iterators. `MutantStack<T>` inherits from it and exposes `begin()` and `end()` by calling the protected `c` member (the underlying container), enabling iteration.

```cpp
MutantStack<int> mstack;
mstack.push(5); mstack.push(17); mstack.push(3);

// Can now iterate like a normal container:
for (MutantStack<int>::iterator it = mstack.begin(); it != mstack.end(); ++it)
    std::cout << *it << "\n";

// All std::stack operations still work:
std::cout << mstack.top() << "\n";  // 3
mstack.pop();
```

## How to Build

```bash
cd ex00 && make && ./easyfind
cd ex01 && make && ./span
cd ex02 && make && ./mutantstack
```
