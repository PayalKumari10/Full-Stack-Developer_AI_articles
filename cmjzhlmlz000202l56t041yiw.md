---
title: "From Full Stack to AI: File and Exception Handling in Python"
seoTitle: "From Full Stack to AI: File and Exception Handling in Python"
seoDescription: "Learn about handling errors and files in Python, essential skills for transitioning from full stack development to AI and data work"
datePublished: Sun Jan 04 2026 08:45:53 GMT+0000 (Coordinated Universal Time)
cuid: cmjzhlmlz000202l56t041yiw
slug: from-full-stack-to-ai-file-and-exception-handling-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767515706690/06dd12f2-ac55-4078-9518-daa75b91821e.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1767516210771/edc506ec-67cf-48a0-b74b-21c607a4e072.png
tags: ai, technology, python, engineering, community, developer, full-stack, hashnode, articles, technical-writing-1, build-in-public, buildinpublic, payalkumari11, fullstackai, payallearnsai

---

When you move from full stack development into AI or data work, Python becomes your daily tool. In real projects, things go wrong often. A missing file, a wrong input type, or an unexpected value can break your code. Exception and file handling help you write code that fails gracefully instead of crashing suddenly. This is a basic but very important habit to build early.

This article is written like a personal learning note. Simple examples, clear outputs, and chai based stories to make things relatable.

---

## What is error handling in Python

Error handling is how we deal with runtime problems in our code. These are issues that Python only discovers while running the program.

For example, trying to access data that does not exist.

### Example

```python
orders = ["masala", "ginger"]

print(orders[2])
```

### Output

```plaintext
IndexError: list index out of range
```

### Explanation

The list has only two items. Index 2 does not exist. Python stops the program and shows an error. In real applications, we do not want our program to stop like this. We want to handle the situation properly.

That is where exception handling comes in.

## Try and except

`try` lets you test code that might fail.  
`except` lets you handle the error instead of crashing.

### Example

```python
chai_menu = {"masala": 30, "ginger": 40}

try:
    chai_menu["elaichi"]
except KeyError:
    print("The key that you are trying to access does not exist")

print("Hello chai code")
```

### Output

```plaintext
The key that you are trying to access does not exist
Hello chai code
```

### Explanation

The key `elaichi` is not in the dictionary. Python raises a `KeyError`.  
Instead of stopping the program, the `except` block runs.  
The program continues and prints the next line.

## Try, except, else, and finally

These four together give you full control.

* try: code that may fail
    
* except: runs if an error happens
    
* else: runs if no error happens
    
* finally: always runs
    

### Example

```python
def serve_chai(flavor):
    try:
        print(f"Preparing {flavor} chai...")
        if flavor == "unknow":
            raise ValueError("We don't know that flavor")
    except ValueError as e:
        print("Error:", e)
    else:
        print(f"{flavor} chai is served")
    finally:
        print("Next customer please")

serve_chai("masala")
serve_chai("unknown")
```

### Output

```plaintext
Preparing masala chai...
masala chai is served
Next customer please
Preparing unknown chai...
Error: We don't know that flavor
Next customer please
```

### Explanation

For masala, no error occurs so `else` runs.  
For unknown, a `ValueError` is raised and caught.  
`finally` runs every time, like cleaning up after serving a customer.

## Catching multiple exceptions

Sometimes different things can go wrong and each needs a different response.

### Example

```python
def process_order(item, quantity):
    try:
        price = {"masala": 20}[item]
        cost = price * quantity
        print(f"total cost is {cost}")
    except KeyError:
        print("Sorry that chai is not on menu")
    except TypeError:
        print("Quantity must be in number")

process_order("ginger", 2)
process_order("masala", "two")
```

### Output

```plaintext
Sorry that chai is not on menu
total cost is twotwotwotwotwotwotwotwotwotwotwotwotwotwotwotwotwotwotwotwo
```

### Explanation

The first call fails because ginger is not in the menu.  
The second call does not raise a TypeError. Python allows string multiplication, so `"two" * 20` repeats the string. This shows why understanding data types is important and why logic checks matter.

## Raising your own errors

Sometimes Python will not raise an error automatically, but logically something is wrong. You can raise your own exceptions.

### Example

```python
def brew_chai(flavor):
    if flavor not in ["masala", "ginger", "elaichi"]:
        raise ValueError("Unsupported chai flavor")
    print(f"Brewing {flavor} chai...")

brew_chai("mint")
```

### Output

```plaintext
ValueError: Unsupported chai flavor
```

### Explanation

Mint is not allowed. We manually tell Python that this is an error. This makes our code more strict and predictable.

## Custom exceptions

For bigger projects, custom exceptions make errors clearer and easier to debug.

### Example

```python
class OutOfIngredientsError(Exception):
    pass

def make_chai(milk, sugar):
    if milk == 0 or sugar == 0:
        raise OutOfIngredientsError("Missing milk or sugar")
    print("chai is ready...")

make_chai(0, 1)
```

### Output

```plaintext
OutOfIngredientsError: Missing milk or sugar
```

### Explanation

Instead of using a generic error, we created our own. This helps when many different error cases exist in a system.

## Mini project: chai billing with exception handling

This combines everything learned so far.

### Example

```python
class InvalidChaiError(Exception):
    pass

def bill(flavor, cups):
    menu = {"masala": 20, "ginger": 40}
    try:
        if flavor not in menu:
            raise InvalidChaiError("that chai is not available")
        if not isinstance(cups, int):
            raise TypeError("Number of cups must be an integer")
        total = menu[flavor] * cups
        print(f"Your bill for {cups} cups of {flavor} chai: rupees {total}")
    except Exception as e:
        print("Error:", e)
    finally:
        print("Thank you for visiting")

bill("mint", 2)
bill("masala", "four")
bill("ginger", 3)
```

### Output

```plaintext
Error: that chai is not available
Thank you for visiting
Error: Number of cups must be an integer
Thank you for visiting
Your bill for 3 cups of ginger chai: rupees 120
Thank you for visiting
```

### Explanation

Each bad input is handled cleanly. The program never crashes. This is how real systems should behave.

## File handling with try, except, and with

Files also need proper handling to avoid leaks and corruption.

### Old way

```python
file = open("order.txt", "w")
try:
    file.write("Masala chai - 2 cups")
finally:
    file.close()
```

### Better way using `with`

```python
with open("order.txt", "w") as file:
    file.write("ginger tea - 4 cups")
```

### Output in order.txt

```plaintext
ginger tea - 4 cups
```

### Explanation

`with` automatically closes the file even if an error happens. This is cleaner and safer.

---

## Closing thoughts

Exception and file handling are not advanced topics. They are foundations. When you move into AI, data pipelines, or backend systems, small mistakes can cause big failures. Learning to handle errors step by step builds confidence and discipline.

Write code that expects things to go wrong. That mindset matters more than any library.

## Documenting my Full Stack → AI journey, step by step.

## **By** [**Payal Kumari**](https://www.linkedin.com/in/payalkumari10/)