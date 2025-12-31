---
title: "From Full Stack to AI: Generators and Decorators in Python"
seoTitle: "From Full Stack to AI: Generators and Decorators in Python"
seoDescription: "Learn About Python Generators and Decorators for AI Development"
datePublished: Wed Dec 31 2025 14:23:19 GMT+0000 (Coordinated Universal Time)
cuid: cmju3w6ay003202l8b2yc1jia
slug: from-full-stack-to-ai-generators-and-decorators-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767190870159/537339bc-f010-4057-b693-6b618693f975.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1767190888278/9558833d-7df5-4461-8f32-28973c3250c3.png
tags: ai, technology, python, engineering, community, developer, full-stack, hashnode, articles, technical-writing-1, build-in-public, buildinpublic, payalkumari11, fullstackai, payallearnsai

---

## Introduction

When you move from full stack development into AI or data focused work, you start dealing with large data, long running processes, and reusable logic. Writing everything in a simple list or function often wastes memory or repeats code. This is where generators and decorators quietly become very useful. I am writing this as a learning note, the way I understood these concepts while revisiting Python fundamentals.

---

## What is a Generator

A generator is a special kind of function that gives values one at a time instead of all at once. It uses the keyword `yield` instead of `return`.

The main idea is simple.  
You do not want all results immediately.  
You want values only when needed.

This helps save memory and keeps programs efficient.

---

## 1\. Generators with yield and next

### What it is

A generator pauses its execution when it hits `yield`. When you ask for the next value, it continues from where it stopped.

### Code example

```python
def serve_chai():
    yield "Cup 1: Hot Chocolate"
    yield "Cup 2: Masala Chai"
    yield "Cup 3: Elaichi Chai"

stall = serve_chai()

print(next(stall))
print(next(stall))
print(next(stall))
```

### Output

```plaintext
Cup 1: Hot Chocolate
Cup 2: Masala Chai
Cup 3: Elaichi Chai
```

### Explanation

The function does not run fully at once. Each `next()` call asks the generator for the next cup of chai. Once all values are used, calling `next()` again will raise a `StopIteration` error.

This is different from returning a list where everything is stored in memory at the same time.

## Generator vs Normal Function

### Normal function

```python
def get_chai_list():
    return ["Cup 1", "Cup 2", "Cup 3"]
```

This creates the full list immediately.

### Generator function

```python
def get_chai_gen():
    yield "Cup 1"
    yield "Cup 2"
    yield "Cup 3"

chai = get_chai_gen()
print(next(chai))
print(next(chai))
print(next(chai))
```

### Explanation

The generator creates values only when requested. This is called lazy evaluation and it is very helpful when data is large or infinite.

## 2\. Infinite Generators

### What it is

An infinite generator keeps producing values forever. It is controlled by the code that consumes it.

### Code example

```python
def infinite_chai():
    count = 1
    while True:
        yield f"Refill #{count}"
        count += 1

refill = infinite_chai()
user2 = infinite_chai()

for _ in range(10):
    print(next(refill))

for _ in range(6):
    print(next(user2))
```

### Output

```plaintext
Refill #1
Refill #2
Refill #3
...
Refill #10
Refill #1
Refill #2
...
Refill #6
```

### Explanation

The generator never stops on its own. Each user gets their own independent generator state. This pattern is common in streaming data or continuous tasks.

## 3\. Sending Values to a Generator

### What it is

Generators can also receive data using the `send()` method. This makes them interactive.

### Code example

```python
def food_customer():
    print("Welcome! What food would you like?")
    order = yield
    while True:
        print(f"Preparing: {order}")
        order = yield

stall = food_customer()
next(stall)

stall.send("Special Thali")
stall.send("Dinner Thali")
```

### Output

```plaintext
Welcome! What food would you like?
Preparing: Special Thali
Preparing: Dinner Thali
```

### Explanation

The first `next()` starts the generator. After that, `send()` passes values back into the generator. This is useful in pipelines and task processors.

## 4\. yield from and closing generators

### What it is

`yield from` allows one generator to reuse another generator. Closing a generator helps clean up resources.

### Code example

```python
def local_chai():
    yield "Special Chai"
    yield "Cardamom Chai"

def imported_chai():
    yield "Matcha"
    yield "Oolong"

def full_menu():
    yield from local_chai()
    yield from imported_chai()

for chai in full_menu():
    print(chai)
```

### Output

```plaintext
Special Chai
Cardamom Chai
Matcha
Oolong
```

### Closing a generator

```python
def chai_stall():
    try:
        while True:
            order = yield "Waiting for chai order"
    except:
        print("Stall closed, no more chai")

stall = chai_stall()
print(next(stall))
stall.close()
```

### Explanation

`yield from` keeps code clean. `close()` safely stops the generator and runs cleanup logic.

## 5\. Decorators in Python

### What it is

A decorator is a function that modifies another function without changing its code. It runs logic before and after the function call.

### Code example

```python
from functools import wraps

def my_decorator(func):
    @wraps(func)
    def wrapper():
        print("Before function runs")
        func()
        print("After function runs")
    return wrapper

@my_decorator
def greet():
    print("Hello from decoration class from chai stall")

greet()
print(greet.__name__)
```

### Output

```plaintext
Before function runs
Hello from decoration class from chai stall
After function runs
greet
```

### Explanation

The decorator adds behavior around the function. `wraps` keeps the original function name and metadata.

## 6\. Build a Logger using a Decorator

### What it is

Logging is a common real world use of decorators.

### Code example

```python
from functools import wraps

def log_activity(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling: {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Finished: {func.__name__}")
        return result
    return wrapper

@log_activity
def brew_chai(type, milk="no"):
    print(f"Brewing {type} chai and milk status {milk}")

brew_chai("Masala")
```

### Explanation

The decorator logs when a function starts and ends without touching the function logic itself.

## 7\. Authorization Decorator

### What it is

Decorators are also useful for access control.

### Code example

```python
from functools import wraps

def require_admin(func):
    @wraps(func)
    def wrapper(user_role):
        if user_role != "admin":
            print("Access denied: Admin only")
            return None
        else:
            return func(user_role)
    return wrapper

@require_admin
def access_tea_inventory(role):
    print("Access granted to tea inventory")

access_tea_inventory("user")
access_tea_inventory("admin")
```

### Output

```plaintext
Access denied: Admin only
Access granted to tea inventory
```

### Explanation

The decorator checks permissions before allowing the function to run. This pattern is common in backend systems.

---

## Closing Thoughts

Generators and decorators look confusing at first, but they solve very practical problems. Generators help manage memory and flow. Decorators help keep code clean and reusable. Learning them step by step builds a strong foundation, especially when moving toward AI and data heavy applications.

## Documenting my Full Stack → AI journey, step by step.

## By [Payal Kumari](https://www.linkedin.com/in/payalkumari10/)