## Python developer interview questions

### Python Fundamentals

1. **What are the key features of Python?**
    - **Response**: Python is a high-level, general-purpose programming language known for readable syntax, dynamic typing, automatic memory management, and a large standard library. It supports multiple programming paradigms, including procedural, object-oriented, and functional programming. Python is widely used in web development, automation, data engineering, machine learning, scripting, and testing.

2. **Is Python compiled or interpreted? Explain how Python code is executed.**
    - **Response**: Python is commonly described as an interpreted language, but the standard CPython implementation first compiles source code into bytecode. The Python virtual machine then executes that bytecode. Bytecode may be cached in `__pycache__` files as `.pyc` files. Other implementations, such as PyPy, use different execution strategies and may apply just-in-time compilation.

3. **What is the difference between dynamically typed and strongly typed languages?**
    - **Response**: Python is dynamically typed because variable types are determined at runtime and a name can be rebound to objects of different types. It is also strongly typed because it generally does not silently combine incompatible types. For example, `"5" + 5` raises a `TypeError`; explicit conversion such as `int("5") + 5` is required.

4. **What are Python's built-in data types?**
    - **Response**: Common built-in types include numeric types (`int`, `float`, `complex`), boolean (`bool`), text (`str`), binary types (`bytes`, `bytearray`, `memoryview`), sequence types (`list`, `tuple`, `range`), mapping (`dict`), set types (`set`, `frozenset`), and the `NoneType` represented by `None`. Choosing the right type affects correctness, readability, and performance.

5. **What is the difference between mutable and immutable objects?**
    - **Response**: Mutable objects can be changed after creation; examples include lists, dictionaries, and sets. Immutable objects cannot be changed after creation; examples include integers, floats, strings, and tuples (although a tuple may contain mutable objects). This distinction matters for aliasing, function arguments, hashing, and dictionary keys.

    ```python
    values = [1, 2]
    alias = values
    alias.append(3)
    print(values)  # [1, 2, 3]

    name = "Python"
    name = name + " 3"
    print(name)    # Python 3
    ```

6. **What is the difference between a list and a tuple?**
    - **Response**: A list is mutable and is normally used for collections that change. A tuple is immutable and is useful for fixed records or values that should not be reassigned. Lists use square brackets and tuples usually use parentheses. Tuples can be hashable when all their elements are hashable, which can make them suitable dictionary keys.

7. **What is the difference between a set and a dictionary?**
    - **Response**: A set stores unique hashable values and is useful for membership checks and set operations. A dictionary stores key-value pairs and requires unique, hashable keys. Both are implemented using hash tables in CPython and generally offer average-case constant-time membership or key lookup, subject to good hash distribution.

8. **What is the difference between `is` and `==` in Python?**
    - **Response**: `==` checks value equality by calling an object's equality behavior, while `is` checks object identity, meaning whether two references point to the same object. Use `is None` to test for `None`; do not use identity to compare ordinary strings or numbers.

    ```python
    first = [1, 2]
    second = [1, 2]
    third = first

    print(first == second)  # True
    print(first is second)  # False
    print(first is third)   # True
    ```

9. **What are Python's naming conventions and coding standards?**
    - **Response**: PEP 8 is the widely used style guide. It recommends `snake_case` for functions and variables, `PascalCase` for classes, and uppercase names for module-level constants. Use meaningful names, consistent indentation, sensible line lengths, and clear imports. Type hints and documentation strings improve maintainability in larger codebases.

10. **What is the difference between shallow copy and deep copy?**
    - **Response**: A shallow copy creates a new outer object but retains references to nested objects. A deep copy recursively copies nested objects, subject to the behavior of the objects involved. Use the `copy` module's `copy()` and `deepcopy()` functions when those semantics are needed.

    ```python
    import copy

    original = [[1, 2], [3, 4]]
    shallow = copy.copy(original)
    deep = copy.deepcopy(original)

    original[0].append(99)
    print(shallow)  # [[1, 2, 99], [3, 4]]
    print(deep)     # [[1, 2], [3, 4]]
    ```

### Data Structures, Functions, and Language Features

11. **How do lists work internally in Python, and what are their time complexities?**
    - **Response**: In CPython, a list is a dynamic array of object references. Indexing is O(1), appending is amortized O(1), and inserting or deleting near the beginning or middle is O(n) because elements need to shift. Searching by value is O(n). These are typical complexity characteristics, not guarantees for every Python implementation.

12. **How do dictionaries work internally?**
    - **Response**: A dictionary is a hash-table-based mapping. Python calculates a hash for a key and uses it to locate an entry, checking equality when needed to resolve collisions. Average-case lookup, insertion, and deletion are O(1), while pathological collision cases can be slower. Keys must be hashable and should have stable hash and equality behavior while stored.

13. **What is a list comprehension, and how is it different from a regular loop?**
    - **Response**: A list comprehension creates a list from an iterable using a compact expression with optional filtering. It is often clearer than building a list with repeated `append()` calls, but deeply nested comprehensions can reduce readability.

    ```python
    numbers = [1, 2, 3, 4, 5]
    squares = [n * n for n in numbers if n % 2 == 0]
    print(squares)  # [4, 16]
    ```

14. **What are generator expressions, and how do they differ from list comprehensions?**
    - **Response**: A list comprehension eagerly creates and stores all results in memory. A generator expression produces values lazily, one at a time, when iterated. Generators are useful for large data streams or pipelines when results do not need to be retained.

    ```python
    squares_list = [n * n for n in range(1_000_000)]
    squares_gen = (n * n for n in range(1_000_000))

    print(next(squares_gen))  # 0
    ```

15. **What are `*args` and `**kwargs` in Python?**
    - **Response**: `*args` collects additional positional arguments into a tuple, while `**kwargs` collects additional keyword arguments into a dictionary. They are useful for flexible APIs, decorators, and forwarding arguments. Required and commonly used parameters should still be explicit where possible.

    ```python
    def display_user(user_id, *roles, **details):
        print(user_id, roles, details)

    display_user(10, "admin", "editor", active=True, city="Kanpur")
    ```

16. **What is the difference between positional, keyword-only, and default arguments?**
    - **Response**: Positional arguments are matched by their position; keyword arguments are matched by parameter name. Parameters with default values can be omitted by callers. A bare `*` in a function signature makes following parameters keyword-only, which can make APIs clearer and safer.

    ```python
    def create_user(name, active=True, *, role="member"):
        return {"name": name, "active": active, "role": role}

    create_user("Anand", role="admin")
    ```

17. **Why should mutable default arguments be avoided?**
    - **Response**: Default argument expressions are evaluated once when the function is defined, not each time it is called. A mutable default such as a list is therefore shared between calls, which can cause unexpected state retention. Use `None` as the default and create a new object inside the function.

    ```python
    def add_item(item, items=None):
        if items is None:
            items = []
        items.append(item)
        return items
    ```

18. **What is a lambda function?**
    - **Response**: A lambda is a small anonymous function containing a single expression. It is commonly used for short callbacks or as a sorting key. For complex logic, a named `def` function is more readable and easier to test.

    ```python
    employees = [{"name": "A", "salary": 50000},
                 {"name": "B", "salary": 70000}]
    ordered = sorted(employees, key=lambda employee: employee["salary"])
    ```

19. **What is the difference between `map()`, `filter()`, and `reduce()`?**
    - **Response**: `map()` applies a function to each item and returns an iterator; `filter()` yields items that satisfy a predicate; `reduce()` combines items into a single accumulated result and is available in `functools`. Comprehensions and built-ins are often clearer for straightforward transformations.

    ```python
    from functools import reduce

    numbers = [1, 2, 3, 4]
    doubled = list(map(lambda n: n * 2, numbers))
    evens = list(filter(lambda n: n % 2 == 0, numbers))
    total = reduce(lambda a, b: a + b, numbers, 0)
    ```

20. **What are iterators and iterables?**
    - **Response**: An iterable is an object that can return an iterator, typically through `iter()`. An iterator implements `__iter__()` and `__next__()` and produces values one at a time, raising `StopIteration` when exhausted. A list is iterable; an iterator created from it tracks iteration state.

    ```python
    values = [10, 20, 30]
    iterator = iter(values)

    print(next(iterator))  # 10
    print(next(iterator))  # 20
    ```

### Object-Oriented Programming

21. **What are the main principles of object-oriented programming in Python?**
    - **Response**: The four commonly cited principles are encapsulation, abstraction, inheritance, and polymorphism. Encapsulation groups state and behavior; abstraction exposes essential operations while hiding implementation details; inheritance supports specialization and reuse; polymorphism allows different objects to respond to a common interface.

22. **What is the difference between a class and an object?**
    - **Response**: A class defines a type's attributes and behavior, while an object is an instance of that class with its own state. A class can create many independent objects. In Python, classes are objects too: they are instances of the metaclass `type` by default.

    ```python
    class Employee:
        def __init__(self, name):
            self.name = name

    employee = Employee("Anand")
    print(employee.name)
    ```

23. **What is the purpose of `self` in Python?**
    - **Response**: `self` is the conventional name for the first parameter of an instance method. It refers to the instance on which the method was called. It is not a reserved keyword; Python passes the instance automatically when an instance method is accessed through an object.

24. **What is the difference between instance methods, class methods, and static methods?**
    - **Response**: Instance methods receive the instance as `self` and operate on instance state. Class methods use `@classmethod` and receive the class as `cls`, often for alternative constructors or class-level behavior. Static methods use `@staticmethod` and receive no implicit instance or class argument.

    ```python
    class Employee:
        company = "Example Inc."

        def __init__(self, name):
            self.name = name

        def introduce(self):
            return self.name

        @classmethod
        def company_name(cls):
            return cls.company

        @staticmethod
        def is_valid_name(name):
            return bool(name.strip())
    ```

25. **What is inheritance, and what types of inheritance does Python support?**
    - **Response**: Inheritance lets a class derive behavior and attributes from one or more base classes. Python supports single, multiple, multilevel, and hierarchical inheritance. Inheritance should model a genuine substitutable relationship; composition is often preferable when the relationship is simply "has a".

26. **What is method overriding?**
    - **Response**: Method overriding occurs when a subclass defines a method with the same name as an inherited method, providing specialized behavior. Calls on a subclass instance normally resolve to the subclass implementation. `super()` can be used to call the next implementation in the method resolution order.

    ```python
    class Animal:
        def speak(self):
            return "sound"

    class Dog(Animal):
        def speak(self):
            return "bark"

    print(Dog().speak())  # bark
    ```

27. **Does Python support method overloading?**
    - **Response**: Python does not support traditional compile-time method overloading by defining several methods with the same name and different signatures; a later definition replaces an earlier one in the class namespace. Similar behavior can be designed with default arguments, `*args`, keyword arguments, or dispatch tools such as `functools.singledispatch`.

28. **What is multiple inheritance, and how does Python resolve method calls?**
    - **Response**: Multiple inheritance allows a class to inherit from more than one base class. Python uses the Method Resolution Order (MRO), calculated using C3 linearization, to determine the order in which base classes are searched. Inspect it with `ClassName.mro()` or `ClassName.__mro__`.

    ```python
    class A:
        def show(self):
            print("A")

    class B(A):
        def show(self):
            print("B")
            super().show()

    class C(A):
        def show(self):
            print("C")
            super().show()

    class D(B, C):
        pass

    print(D.mro())
    D().show()
    ```

29. **What is encapsulation in Python, given that it does not enforce private fields like some languages?**
    - **Response**: Python relies largely on conventions and name mangling rather than strict access control. A single leading underscore indicates a non-public implementation detail. A double leading underscore triggers name mangling to reduce accidental name conflicts in subclasses. Properties can provide controlled attribute access, validation, or computed values.

30. **What are abstract classes and abstract methods in Python?**
    - **Response**: The `abc` module provides `ABC` and `@abstractmethod` for defining abstract interfaces. A class with unimplemented abstract methods cannot be instantiated. Abstract base classes are useful when related implementations must provide a common contract.

    ```python
    from abc import ABC, abstractmethod

    class Payment(ABC):
        @abstractmethod
        def pay(self, amount):
            pass

    class CardPayment(Payment):
        def pay(self, amount):
            return f"Paid {amount} by card"
    ```

### Advanced Python Features

31. **What are decorators in Python?**
    - **Response**: A decorator is a callable that takes another callable (or, in broader cases, a class) and returns a replacement or wrapper. Decorators are used for logging, authorization, caching, metrics, and registration. Use `functools.wraps` to preserve important metadata of wrapped functions.

    ```python
    from functools import wraps

    def log_call(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            print(f"Calling {func.__name__}")
            return func(*args, **kwargs)
        return wrapper

    @log_call
    def greet(name):
        return f"Hello, {name}"
    ```

32. **What are generators, and how does the `yield` keyword work?**
    - **Response**: A generator function contains `yield`. Calling it returns a generator iterator without running the function body immediately. Each `next()` resumes execution until the next `yield`, preserving local state between yields. Generators enable lazy processing and can reduce memory use for large sequences.

    ```python
    def read_ids(limit):
        for item_id in range(limit):
            yield item_id

    for item_id in read_ids(3):
        print(item_id)
    ```

33. **What is the difference between `yield` and `return`?**
    - **Response**: `return` terminates a function and provides its final result. In a generator, `return` ends iteration and may provide a value through the `StopIteration` exception's `value` attribute. `yield` emits a value while pausing the generator so it can continue later.

34. **What are context managers, and how does the `with` statement work?**
    - **Response**: A context manager manages setup and cleanup around a block of code. The `with` statement invokes `__enter__()` and later `__exit__()`, including when an exception occurs. Files, locks, and database transactions commonly use context managers. The `contextlib` module makes custom context managers easier to write.

    ```python
    with open("data.txt", "w", encoding="utf-8") as file:
        file.write("Hello Python")
    # The file is closed when the block exits.
    ```

35. **What are Python's magic or dunder methods?**
    - **Response**: Dunder methods are special methods with double underscores, such as `__init__`, `__str__`, `__repr__`, `__len__`, `__iter__`, and `__eq__`. They let custom objects integrate with language operations and built-ins. Implement only the methods whose semantics are appropriate for the class.

36. **What is the difference between `__str__` and `__repr__`?**
    - **Response**: `__str__` provides a readable, user-facing representation. `__repr__` provides a developer-facing representation intended to be unambiguous and, where practical, useful for reconstructing the object. `str()` prefers `__str__`; `repr()` uses `__repr__`.

37. **What are properties in Python, and why are they used?**
    - **Response**: The `property` decorator exposes methods through attribute access. It allows a class to add validation or computed behavior without changing the public attribute syntax used by callers. Properties are useful for maintaining invariants, but should not hide expensive or surprising operations.

    ```python
    class Product:
        def __init__(self, price):
            self.price = price

        @property
        def price(self):
            return self._price

        @price.setter
        def price(self, value):
            if value < 0:
                raise ValueError("Price cannot be negative")
            self._price = value
    ```

38. **What is a closure in Python?**
    - **Response**: A closure is a function that retains access to variables from its enclosing lexical scope even after that outer function has finished executing. Closures are often used to build configurable functions, callbacks, and decorators. Use `nonlocal` when a nested function needs to rebind a variable in an enclosing, non-global scope.

    ```python
    def make_multiplier(factor):
        def multiply(value):
            return value * factor
        return multiply

    double = make_multiplier(2)
    print(double(5))  # 10
    ```

39. **What are type hints, and does Python enforce them at runtime?**
    - **Response**: Type hints document expected types and allow static analysis tools and IDEs to detect likely mistakes. Python generally does not enforce annotations at runtime. Tools such as mypy and Pyright can check code without executing it. The `typing` module provides constructs such as `Optional`, `Union`, `Protocol`, and generic types.

    ```python
    def total(prices: list[float]) -> float:
        return sum(prices)
    ```

40. **What are dataclasses, and when would you use them?**
    - **Response**: The `dataclasses` module generates common methods such as `__init__`, `__repr__`, and `__eq__` based on annotated fields. Dataclasses reduce boilerplate for classes primarily used to store structured data. They support defaults, frozen instances, ordering, and post-initialization hooks.

    ```python
    from dataclasses import dataclass

    @dataclass
    class Employee:
        name: str
        department: str
        active: bool = True
    ```

### Memory Management and Performance

41. **How does memory management work in Python?**
    - **Response**: In CPython, objects are managed through reference counting, supplemented by a cyclic garbage collector for reference cycles. When an object's reference count reaches zero, it can usually be deallocated immediately. Other Python implementations may use different memory-management strategies. Developers should manage external resources such as files and sockets explicitly with context managers.

42. **What is garbage collection in Python?**
    - **Response**: Garbage collection reclaims objects that are no longer reachable or useful. CPython primarily uses reference counting and also has a cyclic garbage collector that can detect unreachable reference cycles. The `gc` module exposes controls and diagnostics, but manual collection is rarely needed in ordinary application code.

43. **What is the Global Interpreter Lock (GIL)?**
    - **Response**: In traditional GIL-enabled CPython builds, the Global Interpreter Lock allows only one thread at a time to execute Python bytecode in a given interpreter, which limits CPU-bound parallel execution with threads. Threads can still be useful for I/O-bound work because the interpreter can switch while operations wait. Python also has optional free-threaded CPython builds in newer releases, so the exact behavior depends on the build and version.

44. **How can you improve the performance of a Python application?**
    - **Response**: Start by measuring with a profiler rather than guessing. Improve algorithmic complexity, reduce unnecessary I/O, choose appropriate data structures, batch operations, use caching where safe, and avoid repeated work. For CPU-heavy workloads, consider optimized native libraries, multiprocessing, or compiled extensions. Benchmark realistic workloads and validate memory and latency trade-offs.

45. **What is the difference between shallow memory usage and deep memory usage of an object?**
    - **Response**: A shallow size measures the memory directly attributed to an object, while deep size also accounts for objects it references, subject to how shared references are counted. `sys.getsizeof()` reports shallow size for many built-in objects. Tools such as `tracemalloc` can help investigate allocations; deep-size measurements require more careful tooling and interpretation.

46. **What is memoization, and how can you implement it in Python?**
    - **Response**: Memoization caches the results of a function for previously seen inputs, which can speed up repeated calculations. `functools.lru_cache` provides a bounded least-recently-used cache. Arguments must be hashable, and caching is appropriate only when results remain valid for the same inputs and hidden state does not invalidate them.

    ```python
    from functools import lru_cache

    @lru_cache(maxsize=128)
    def fibonacci(n):
        if n < 2:
            return n
        return fibonacci(n - 1) + fibonacci(n - 2)
    ```

47. **What is the difference between time complexity and space complexity?**
    - **Response**: Time complexity describes how an algorithm's running work grows with input size; space complexity describes how its memory requirements grow. Big-O notation expresses an upper-bound growth class, ignoring constant factors. Interview answers should identify the dominant operations and any auxiliary memory, not just give a complexity label.

48. **How do you profile a Python application?**
    - **Response**: Use `cProfile` to identify time-consuming functions and `timeit` for small, controlled benchmarks. Use `tracemalloc` or memory profilers to investigate allocation growth. For production services, combine application-level metrics, tracing, and representative load tests. Profile under realistic inputs and avoid optimizing noise.

49. **What is the difference between `deepcopy()` and serialization-based copying?**
    - **Response**: `copy.deepcopy()` recursively copies supported Python objects while tracking already-copied references to handle cycles. Serialization-based copying converts data into a serialized form and reconstructs it, imposing format and type constraints and sometimes losing object identity or custom state. Never use unsafe deserialization on untrusted input; for data exchange, prefer explicit, well-defined formats.

50. **What is the purpose of slots (`__slots__`) in Python classes?**
    - **Response**: `__slots__` declares a fixed set of instance attributes and can avoid a per-instance `__dict__`, potentially reducing memory use for many small objects. It also restricts adding undeclared attributes unless a dictionary is included. It has inheritance and tooling implications, so measure before adopting it as an optimization.

### Exception Handling and Modules

51. **How does exception handling work in Python?**
    - **Response**: Python uses `try`, `except`, `else`, and `finally` blocks. The `except` clause handles matching exceptions, `else` runs when no exception occurs in the `try` block, and `finally` runs during cleanup regardless of success or failure. Catch specific exceptions and avoid silently swallowing unexpected errors.

    ```python
    try:
        result = 10 / 2
    except ZeroDivisionError:
        result = None
    else:
        print("Calculation succeeded")
    finally:
        print("Finished")
    ```

52. **What is the difference between an error and an exception in Python?**
    - **Response**: Exceptions are runtime events represented by exception objects and can often be handled by application code. The term "error" is commonly used informally for a problem, and many built-in exception names end in `Error`, such as `TypeError` and `ValueError`. Some serious exceptions derive directly or indirectly from `BaseException` and are generally not caught by broad application handlers.

53. **How do you create a custom exception?**
    - **Response**: Define a class that inherits from `Exception` or a more specific built-in exception. Custom exceptions communicate domain-specific failure conditions and allow callers to handle them separately. Include useful context in the message or attributes.

    ```python
    class InsufficientBalanceError(Exception):
        pass

    def withdraw(balance, amount):
        if amount > balance:
            raise InsufficientBalanceError("Insufficient funds")
        return balance - amount
    ```

54. **What is the difference between `raise` and `assert`?**
    - **Response**: `raise` explicitly signals an exception and is appropriate for enforcing runtime behavior. `assert` checks a debugging assumption and raises `AssertionError` if it fails; assertions can be disabled with optimization settings. Do not use assertions for input validation, authorization, or business rules that must always execute.

55. **What is the purpose of the `finally` block?**
    - **Response**: A `finally` block is used for cleanup code that should run whether the `try` block succeeds or raises an exception. It is commonly used to release resources, though context managers are usually clearer for files and locks. A return or exception inside `finally` can override an earlier result or exception and should generally be avoided.

56. **What are modules and packages in Python?**
    - **Response**: A module is a Python file that defines code such as functions, classes, and variables. A package organizes modules under a common namespace, commonly using directories and optionally `__init__.py`. Packages may also be namespace packages. This structure helps organize large applications and control imports.

57. **What is the difference between `import module` and `from module import name`?**
    - **Response**: `import module` binds the module name, so its members are accessed with qualified names such as `module.name`. `from module import name` binds the selected name directly in the current namespace. Qualified imports can improve clarity and reduce accidental name collisions; imports should generally be kept at module scope unless there is a good reason otherwise.

58. **What is the purpose of `if __name__ == "__main__":`?**
    - **Response**: When a Python file is run as the top-level program, its `__name__` is set to `"__main__"`. When imported as a module, `__name__` is usually the module's qualified name. The guard keeps command-line or demonstration code from running automatically on import.

    ```python
    def main():
        print("Application started")

    if __name__ == "__main__":
        main()
    ```

59. **What is a virtual environment, and why is it important?**
    - **Response**: A virtual environment isolates a project's Python interpreter context and installed packages from other projects. It helps prevent dependency conflicts and makes setup more reproducible. Common tools include the built-in `venv`, virtualenv, and environment managers such as Conda or uv.

60. **What is the difference between pip, requirements.txt, and pyproject.toml?**
    - **Response**: `pip` is a package installer. A `requirements.txt` file commonly lists dependencies for installation, often with pinned versions. `pyproject.toml` is a standardized project configuration file used by build systems and tools to declare project metadata, dependencies, and tool settings. A lock file may be used by a chosen tool to record a reproducible dependency resolution.

### Multithreading, Multiprocessing, and Asynchronous Programming

61. **What is the difference between a process and a thread in Python?**
    - **Response**: A process has its own memory space and operating-system resources. Threads run within a process and share its memory, which makes communication easier but introduces synchronization concerns. In traditional GIL-enabled CPython, threads are generally suited to I/O-bound concurrency, while processes can use multiple CPU cores for Python CPU-bound work.

62. **What is multithreading, and when should you use it in Python?**
    - **Response**: Multithreading runs multiple threads within one process. It is useful for overlapping I/O operations such as network requests, file access, or waiting on external services. Shared mutable state requires synchronization. For CPU-bound Python code on a traditional GIL-enabled CPython build, threads often do not provide parallel bytecode execution.

    ```python
    from concurrent.futures import ThreadPoolExecutor

    def fetch_item(item_id):
        # Replace with an I/O operation.
        return f"item-{item_id}"

    with ThreadPoolExecutor(max_workers=5) as executor:
        results = list(executor.map(fetch_item, range(10)))
    ```

63. **What is multiprocessing, and how is it different from multithreading?**
    - **Response**: Multiprocessing uses multiple processes, each with its own interpreter and memory space. It can execute CPU-bound Python work in parallel across cores, including on traditional GIL-enabled CPython. Processes have higher startup and communication costs, and data must be transferred or shared through mechanisms such as queues, pipes, shared memory, or managers.

    ```python
    from concurrent.futures import ProcessPoolExecutor

    def square(number):
        return number * number

    if __name__ == "__main__":
        with ProcessPoolExecutor() as executor:
            print(list(executor.map(square, range(5))))
    ```

64. **What is the Global Interpreter Lock's impact on multithreaded applications?**
    - **Response**: On traditional GIL-enabled CPython, the GIL protects interpreter state and means only one thread executes Python bytecode at a time per interpreter. I/O operations can release the GIL, so threads can still improve throughput for I/O-bound work. CPU-intensive workloads may benefit more from processes, native code that releases the GIL, or a free-threaded build where supported and appropriate.

65. **What is a race condition, and how can you prevent it?**
    - **Response**: A race condition occurs when program behavior depends on the timing or interleaving of concurrent operations. Shared read-modify-write operations can produce incorrect results without coordination. Use locks, queues, immutable data, atomic operations provided by suitable libraries, or designs that avoid shared mutable state.

    ```python
    from threading import Lock

    counter = 0
    lock = Lock()

    def increment():
        global counter
        with lock:
            counter += 1
    ```

66. **What is a deadlock, and how can it be avoided?**
    - **Response**: A deadlock occurs when concurrent tasks wait indefinitely for resources held by one another. Avoid it by acquiring locks in a consistent global order, minimizing lock scope, avoiding unnecessary nested locks, using timeouts where suitable, and preferring higher-level concurrency primitives. Diagnose deadlocks with thread dumps or stack inspection.

67. **What are asynchronous programming and `asyncio` in Python?**
    - **Response**: Asynchronous programming allows a single thread to manage many tasks that spend time waiting, such as network or disk I/O. `asyncio` provides an event loop, tasks, and awaitable operations. An `async def` function creates a coroutine, and `await` suspends it while the awaited operation makes progress. It is not automatic CPU parallelism.

    ```python
    import asyncio

    async def fetch_data():
        await asyncio.sleep(1)
        return "data received"

    async def main():
        result = await fetch_data()
        print(result)

    asyncio.run(main())
    ```

68. **What is the difference between `async`, `await`, and a coroutine?**
    - **Response**: `async def` defines a coroutine function. Calling it creates a coroutine object, which represents work that can be scheduled or awaited. `await` suspends the current coroutine until an awaitable completes, allowing the event loop to run other tasks. A coroutine must be awaited or scheduled to execute.

69. **What is the difference between concurrency and parallelism?**
    - **Response**: Concurrency means multiple tasks make progress during overlapping periods; their execution may be interleaved. Parallelism means tasks execute at the same instant, typically on multiple CPU cores. Async I/O and threads often provide concurrency; multiprocessing and suitable native code can provide parallel execution.

70. **How do you handle exceptions in asynchronous Python code?**
    - **Response**: Use `try/except/finally` around awaited operations, just as with synchronous code. When using `asyncio.gather()`, understand how exceptions are propagated and consider `return_exceptions=True` only when treating exceptions as results is intentional. In modern Python, `asyncio.TaskGroup` provides structured concurrency and coordinated cancellation for related tasks.

### File Handling, APIs, and Databases

71. **How do you read and write files safely in Python?**
    - **Response**: Use `open()` with a context manager so files are closed reliably. Specify an encoding such as UTF-8 for text files, choose an appropriate mode, and handle expected I/O exceptions. For large files, iterate line by line rather than reading the entire file into memory.

    ```python
    with open("input.txt", "r", encoding="utf-8") as source:
        for line in source:
            print(line.rstrip())

    with open("output.txt", "w", encoding="utf-8") as target:
        target.write("Hello\n")
    ```

72. **What is the difference between text mode and binary mode when handling files?**
    - **Response**: Text mode decodes bytes into strings using an encoding and may translate newline conventions. Binary mode reads and writes bytes without text decoding or newline translation. Use text mode for documents and structured text; use binary mode for images, archives, executables, and other byte-oriented data.

73. **How do you work with JSON in Python?**
    - **Response**: The standard `json` module provides `loads()` and `dumps()` for strings, and `load()` and `dump()` for file objects. JSON maps naturally to dictionaries, lists, strings, numbers, booleans, and `None`. Validate external data and handle `JSONDecodeError`; do not assume parsed input has the expected schema.

    ```python
    import json

    payload = {"name": "Anand", "active": True}
    serialized = json.dumps(payload)
    restored = json.loads(serialized)
    ```

74. **How do you call a REST API from Python?**
    - **Response**: A common approach is the third-party `requests` library or an asynchronous client such as `httpx`. Set timeouts, handle non-success status codes, validate response content, and consider retries for transient failures with backoff. Keep credentials out of source code and avoid logging secrets or sensitive response data.

    ```python
    import requests

    response = requests.get(
        "https://api.example.com/users",
        timeout=10
    )
    response.raise_for_status()
    users = response.json()
    ```

75. **What is the difference between synchronous and asynchronous HTTP requests?**
    - **Response**: A synchronous request blocks the calling thread until the response or error is available. An asynchronous request yields control while waiting, allowing an event loop to run other tasks. Async clients such as `httpx.AsyncClient` or `aiohttp` can improve throughput for many concurrent I/O operations, but require async-compatible application flow.

76. **How do you connect Python to a relational database?**
    - **Response**: Use a database driver or an abstraction such as SQLAlchemy. Create connections through a pool where appropriate, use parameterized queries to prevent SQL injection, manage transactions explicitly, and close resources reliably. Database-specific drivers include psycopg for PostgreSQL, mysql-connector-python for MySQL, and the built-in `sqlite3` module for SQLite.

    ```python
    import sqlite3

    with sqlite3.connect("app.db") as connection:
        connection.execute(
            "CREATE TABLE IF NOT EXISTS users "
            "(id INTEGER PRIMARY KEY, name TEXT)"
        )
        connection.execute(
            "INSERT INTO users (name) VALUES (?)",
            ("Anand",)
        )
    ```

77. **What is an ORM, and what are the advantages and disadvantages of using one?**
    - **Response**: An Object-Relational Mapper maps database tables and rows to application objects. It can reduce repetitive SQL, provide model relationships, and integrate with transactions and migrations. It can also hide expensive queries, encourage N+1 query problems, and make complex SQL harder to express. Developers should understand the generated SQL and use direct queries when appropriate.

78. **What is SQLAlchemy, and what is the difference between Core and ORM?**
    - **Response**: SQLAlchemy is a Python SQL toolkit and ORM. SQLAlchemy Core provides SQL expression constructs and connection/transaction APIs, while the ORM maps database rows to Python classes and manages object persistence. Both can be used together. Choose based on whether the code benefits from domain objects or needs direct SQL-oriented control.

79. **What is connection pooling, and why is it important?**
    - **Response**: Connection pooling maintains a reusable set of database connections. Opening a new connection for every operation is expensive, while pooling reduces connection setup overhead and controls concurrent connection usage. Configure pool size, timeouts, health checks, and transaction cleanup based on application load and database limits.

80. **How do you prevent SQL injection in Python applications?**
    - **Response**: Use parameterized queries or prepared statements so user input is treated as data, not executable SQL. Do not build SQL by concatenating untrusted strings. For dynamic table or column names that cannot be bound as parameters, use strict allowlists and safe identifier APIs provided by the database library.

    ```python
    cursor.execute(
        "SELECT id, name FROM users WHERE email = ?",
        (email,)
    )
    ```

### Web Frameworks, Testing, and Quality

81. **What are the popular Python web frameworks, and how do they differ?**
    - **Response**: Django is a batteries-included framework with an ORM, admin site, authentication, and conventions. Flask is a lightweight WSGI framework with a flexible extension ecosystem. FastAPI is an ASGI framework designed for typed API development, validation, and asynchronous endpoints. The right choice depends on project needs, team familiarity, ecosystem, and deployment requirements.

82. **What is the difference between WSGI and ASGI?**
    - **Response**: WSGI is the traditional synchronous Python web-server interface. ASGI supports asynchronous applications and protocols such as HTTP and WebSockets. Frameworks such as Flask and traditional Django deployments commonly use WSGI, while FastAPI and modern Django deployments can use ASGI. ASGI can support long-lived and highly concurrent connections when the application and dependencies are async-compatible.

83. **What is middleware in a Python web application?**
    - **Response**: Middleware is a layer that processes requests and responses around application handlers. It is commonly used for logging, authentication, CORS, security headers, tracing, and request IDs. Middleware order matters because it affects how requests enter the stack and responses leave it. Keep middleware focused and avoid duplicating business logic.

84. **How do you implement authentication and authorization in a Python API?**
    - **Response**: Authentication establishes who the caller is; authorization determines what that caller is allowed to do. Common approaches include secure server-side sessions and token-based methods such as OAuth 2.0 or OpenID Connect. Validate tokens, enforce permissions on the server, use HTTPS, store passwords with a modern password-hashing algorithm, and apply least privilege. A valid login must not imply unrestricted access.

85. **What is unit testing, and which testing frameworks are commonly used in Python?**
    - **Response**: Unit testing verifies small pieces of behavior in isolation. Python's standard library includes `unittest`; `pytest` is a widely used third-party framework with fixtures, parameterization, and a rich plugin ecosystem. Tests should be deterministic, focused, and independent of production services unless an integration test explicitly requires them.

    ```python
    def add(a, b):
        return a + b

    def test_add():
        assert add(2, 3) == 5
    ```

86. **What are fixtures in pytest?**
    - **Response**: Fixtures provide reusable setup and cleanup for tests. They can create test data, configure clients, open temporary resources, or prepare database state. Fixtures can have different scopes such as function, class, module, or session; choose the narrowest practical scope to avoid state leakage.

    ```python
    import pytest

    @pytest.fixture
    def user():
        return {"name": "Anand", "active": True}

    def test_user_is_active(user):
        assert user["active"] is True
    ```

87. **What is mocking, and when should you use it in unit tests?**
    - **Response**: Mocking replaces a dependency with a controlled test double so a unit can be tested in isolation. The `unittest.mock` module provides `Mock`, `MagicMock`, and `patch`; pytest also supports monkeypatching. Mock external boundaries such as HTTP clients or clocks when useful, but avoid mocking every internal call because tests can become tightly coupled to implementation details.

88. **What is the difference between unit testing, integration testing, and end-to-end testing?**
    - **Response**: Unit tests verify small components in isolation. Integration tests verify that components work together, such as application code with a real database. End-to-end tests exercise complete user workflows across the system. A balanced test suite uses many fast unit tests, targeted integration tests, and a smaller number of high-value end-to-end tests.

89. **How do you manage dependencies and make Python deployments reproducible?**
    - **Response**: Declare project dependencies and compatible version ranges, then use the chosen package tool's lock file or pinned requirements to reproduce an environment. Build and deploy from a clean environment, test the installation process, and separate runtime from development dependencies. Rebuild dependencies deliberately rather than relying on a developer's local machine.

90. **How do you debug a Python application in production?**
    - **Response**: Use structured logs, correlation or request IDs, metrics, distributed tracing, and error monitoring to identify failing requests and slow operations. Capture enough context to reproduce an issue while redacting credentials and personal data. Use a debugger or profiler in a controlled environment; avoid exposing interactive debug endpoints or sensitive tracebacks publicly.

### Advanced Concepts, Security, and System Design

91. **What is monkey patching in Python, and what are its risks?**
    - **Response**: Monkey patching changes a module, class, or object at runtime, often by replacing an attribute or method. It can be useful in tests or compatibility layers, but it creates hidden global behavior, order-dependent bugs, and upgrade risks. Prefer dependency injection, adapters, or explicit extension points for production design.

92. **What is the descriptor protocol in Python?**
    - **Response**: A descriptor is an object that defines one or more of `__get__`, `__set__`, or `__delete__`. Descriptors customize attribute access and power features such as properties, methods, and many ORM fields. Data descriptors define `__set__` or `__delete__`; non-data descriptors define only `__get__`. Understanding descriptors helps explain Python's attribute lookup rules.

93. **What is the Method Resolution Order (MRO), and why is it important?**
    - **Response**: The MRO defines the order Python uses to look up attributes and methods across a class hierarchy. Python uses C3 linearization to create a consistent order for multiple inheritance. Cooperative methods should use `super()` so each class in the MRO can participate, rather than directly calling a named parent and potentially skipping other implementations.

94. **What is the difference between a thread-safe and a process-safe operation?**
    - **Response**: Thread safety means an operation behaves correctly when accessed by multiple threads in the same process. Process safety means it remains correct across separate processes, which generally do not share ordinary memory. A thread lock does not coordinate independent processes. Use process-aware locks, database constraints, file locks, or external coordination services when required.

95. **How do you secure a Python application?**
    - **Response**: Validate inputs at trust boundaries, use parameterized database queries, apply least-privilege permissions, keep dependencies patched, protect secrets with a secret manager or environment-specific secure configuration, and enforce HTTPS. Avoid unsafe deserialization and shell-command construction with untrusted input. Add authentication, authorization, rate limiting, security headers, audit logging, and dependency scanning as appropriate to the threat model.

96. **How would you design a scalable Python REST API?**
    - **Response**: Separate routing, validation, business logic, and data-access layers. Use stateless application instances behind a load balancer, a connection-pooled database, caching for suitable read paths, and background workers for long-running jobs. Add pagination, timeouts, rate limits, retries with backoff, observability, and health checks. Design idempotency and transaction boundaries carefully, and load-test realistic traffic before scaling infrastructure.

97. **How do you handle background jobs in Python applications?**
    - **Response**: Use a task queue such as Celery or RQ with a broker such as Redis or RabbitMQ when work must run outside the request-response cycle. Background tasks should be idempotent where possible, have bounded retries and backoff, expose status and failure information, and avoid assuming that a job executes exactly once. For simple in-process concurrency, standard-library executors may be sufficient.

98. **What is caching, and what caching strategies can be used in Python applications?**
    - **Response**: Caching stores reusable results to reduce latency or repeated computation. Common strategies include in-memory caching, `functools.lru_cache`, application caches, and distributed caches such as Redis. Consider TTLs, invalidation, cache keys, data freshness, stampedes, and whether cached data is safe to share between users. A cache should improve performance without becoming the source of truth.

99. **How do you deploy and monitor a Python application using Docker?**
    - **Response**: Package the application and its dependencies in a Docker image built from a reproducible, minimal base image. Use a non-root user, keep secrets outside the image, define health checks where appropriate, and handle signals and graceful shutdown. Run containers with resource limits and externalized configuration. Monitor logs, metrics, traces, error rates, latency, and resource utilization after deployment.

100. **What are the major differences between Python 2 and Python 3, and what should developers consider in modern Python projects?**
    - **Response**: Python 2 reached end of life in 2020 and should not be used for new development. Python 3 standardized text strings as Unicode, changed division behavior, introduced clearer iterator and exception semantics, and continues to evolve with new language and standard-library features. Modern projects should use a currently supported Python 3 release, declare version compatibility, test dependencies, use type hints where helpful, and plan upgrades with automated tests.

