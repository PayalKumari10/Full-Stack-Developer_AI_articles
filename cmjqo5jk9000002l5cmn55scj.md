---
title: "From Full Stack to AI: Functions in Python"
seoTitle: "Python Functions: From Basics to AI Integration"
seoDescription: "From Full Stack to AI Development: The Role of Python Functions"
datePublished: Mon Dec 29 2025 04:39:24 GMT+0000 (Coordinated Universal Time)
cuid: cmjqo5jk9000002l5cmn55scj
slug: from-full-stack-to-ai-functions-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1766982907753/74413566-e426-46c5-8992-b8d326f6ab82.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1766983062059/559f7229-8f67-4504-a895-501cadbf273e.jpeg
tags: ai, technology, python, engineering, community, developer, full-stack, hashnode, articles, technical-writing-1, build-in-public, buildinpublic, payalkumari11, fullstackai, payallearnsai

---

When you move from full stack development into AI or data related work, Python becomes unavoidable. One thing that stands out very quickly is how much Python relies on functions. Not just to reduce code size, but to structure thinking.

Functions help you write code that is easier to read, easier to test, and easier to change. This chapter is a personal learning note written from a full stack developer’s perspective, focused on building strong basics before moving deeper into AI.

---

## What is a Function in Python

A function is a named block of code that performs one specific task.  
You define it once and reuse it whenever needed.

In simple terms, functions help you:

* avoid repeating the same logic
    
* break large problems into smaller steps
    
* understand code by reading names instead of tracing logic
    

---

## 1\. Reducing Code Duplication

### What this means

Copy pasting the same code again and again makes software fragile. If something changes, you must update many places. Functions solve this by keeping logic in one place.

### Example: Printing chai orders

```python
def print_order(name, chai_type):
    print(f"{name} ordered {chai_type} chai")

print_order("Payal", "masala")
print_order("Ankit", "ginger")
print_order("Sonal", "cardamom")
```

### Output

```plaintext
Payal ordered masala chai
Ankit ordered ginger chai
Sonal ordered cardamom chai
```

### Explanation

The formatting logic exists only once.  
If the message format changes, you update the function and everything stays consistent.

---

## 2\. Splitting Complex Tasks into Smaller Functions

### What this means

Large tasks become easier when broken into smaller steps. Each function handles one responsibility.

### Example: Monthly cafe report

```python
def fetch_sales():
    print("Fetching sales data...")

def filter_valid_sales():
    print("Filtering valid sales data...")

def summarize_data():
    print("Summarizing sales data...")

def generate_report():
    fetch_sales()
    filter_valid_sales()
    summarize_data()
    print("Report is ready!")

generate_report()
```

### Output

```plaintext
Fetching sales data...
Filtering valid sales data...
Summarizing sales data...
Report is ready!
```

### Explanation

`generate_report()` reads like a process description.  
You focus on flow instead of internal details.

---

## 3\. Hiding Implementation Details

### What this means

The main function should explain what happens, not how it happens.  
Details are hidden inside smaller functions.

### Example: User registration flow

```python
def get_input():
    print("Getting user input...")

def validate_input():
    print("Validating user info...")

def save_to_db():
    print("Saving to database...")

def register_user():
    get_input()
    validate_input()
    save_to_db()
    print("User registration complete!")

register_user()
```

### Output

```plaintext
Getting user input...
Validating user info...
Saving to database...
User registration complete!
```

### Explanation

Anyone reading `register_user()` understands the system without reading each function.

---

## 4\. Improving Readability with Functions

### What improving readability means

Readable code explains itself.  
Six months later, you should understand your own code without mental effort.

Functions improve readability by:

* using meaningful names
    
* hiding calculations
    
* reducing mental load while reading
    

### Example: Bill calculation

```python
def calculate_bill(cups, price_per_cup):
    return cups * price_per_cup

my_bill = calculate_bill(3, 20)
print(my_bill)

print("Order for table 2:", calculate_bill(5, 15))
```

### Output

```plaintext
60
Order for table 2: 75
```

### Explanation

You see what is happening, not how it is calculated.  
This is how readable systems are built.

---

## 5\. Improving Traceability

### What this means

Business rules should live in one place.  
When rules change, updates should be easy to trace.

### Example: Adding VAT

```python
def add_vat(price, vat_rate):
    return price * (100 + vat_rate) / 100

orders = [100, 150, 200, 250]

for price in orders:
    final_amount = add_vat(price, 20)
    print(f"Original: {price}, Final with VAT: {final_amount}")
```

### Output

```plaintext
Original: 100, Final with VAT: 120.0
Original: 150, Final with VAT: 180.0
Original: 200, Final with VAT: 240.0
Original: 250, Final with VAT: 300.0
```

### Explanation

If VAT changes, you update one function instead of searching everywhere.

---

## 6\. Scope and Name Resolution in Functions

### What is scope

Scope defines where a variable can be accessed.  
Python searches variables in this order:

* Local
    
* Enclosing
    
* Global
    
* Built in
    

---

### Local and Global Scope

```python
def server_chai():
    chai_type = "Masala Chai"
    print("Inside function:", chai_type)

chai_type = "Ginger Chai"
server_chai()
print("Outside function:", chai_type)
```

### Output

```plaintext
Inside function: Masala Chai
Outside function: Ginger Chai
```

### Explanation

Local variables do not affect global variables.

---

### Enclosing Scope with Nested Functions

```python
def chai_counter():
    chai_order = "Ginger Chai"
    def print_order():
        chai_order = "Masala Chai"
        print("Inner:", chai_order)
    print_order()
    print("Outer:", chai_order)

chai_order = "Tulsi"
chai_counter()
print("Global:", chai_order)
```

### Output

```plaintext
Inner: Masala Chai
Outer: Ginger Chai
Global: Tulsi
```

### Explanation

Each level has its own scope unless explicitly modified.

---

## 7\. Nonlocal vs Global

### Using nonlocal

Use `nonlocal` when modifying a variable from the enclosing function.

```python
chai_type = "Masala"

def update_order():
    chai_type = "Chai"
    def kitchen():
        nonlocal chai_type
        chai_type = "Cardamom"
    kitchen()
    print("After kitchen update:", chai_type)

update_order()
```

### Output

```plaintext
After kitchen update: Cardamom
```

---

### Using global

Use `global` to modify a top level variable. This is usually avoided.

```python
chai_type = "Plain"

def front_desk():
    def kitchen():
        global chai_type
        chai_type = "Kesar Chai"
    kitchen()

front_desk()
print("Final global chai:", chai_type)
```

### Output

```plaintext
Final global chai: Kesar Chai
```

---

## 8\. Handling Function Arguments

### Why arguments matter

Arguments make functions flexible and reusable.

### Positional and Keyword Arguments

```python
def make_chai(tea, milk, sugar):
    print(tea, milk, sugar)

make_chai("Ginger", "Full Cream", "2 spoons")
make_chai(milk="Skimmed", sugar="No sugar", tea="Masala")
```

---

### Variable Arguments

```python
def special_chai(*ingredients, **extras):
    print("Ingredients:", ingredients)
    print("Extras:", extras)

special_chai("Cardamom", "Saffron", milk="Almond Milk", sugar="Honey")
```

---

## 9\. Return Values in Functions

### Returning nothing

```python
def make_chai():
    print("Here is your masala chai")

result = make_chai()
print(result)
```

### Output

```plaintext
Here is your masala chai
None
```

---

### Multiple return values

```python
def chai_report():
    return 100, 20, 10

sold, remaining, not_paid = chai_report()
print("Sold:", sold)
print("Remaining:", remaining)
```

---

## 10\. Pure vs Impure Functions

### Pure functions

Depend only on inputs.

```python
def pure_chai(cups):
    return cups * 10
```

### Impure functions

Depend on external state.

```python
total_chai = 0

def impure_chai(cups):
    global total_chai
    total_chai += cups
    return total_chai
```

Pure functions are easier to test and reason about.

---

## 11\. Lambda Functions

### What lambdas are

Short one line functions for very small logic.

```python
chai_types = ["Masala", "Kadak", "Ginger", "Kesar", "Masala", "Kadak"]

strong_chai = list(filter(lambda chai: chai != "kadak", chai_types))
print(strong_chai)
```

Use lambdas sparingly. Named functions are clearer for most cases.

---

## 12\. Documenting Functions and Project Structure

### Function documentation

```python
def generate_bill(chai=0, samosa=0):
    """
    Calculate the total bill for chai and samosa.
    :param chai: Number of chai cups (10 rupees each)
    :param samosa: Number of samosas (15 rupees each)
    :return: total amount and thank you message
    """
    total = chai * 10 + samosa * 15
    return total, "Thank you for visiting!"
```

---

### Project file structure

```plaintext
06_chai_business/
│
├── recipes/
│   ├── __init__.py
│   ├── flavor.py
│
├── utils/
│   ├── discounts.py
│
├── main.py
├── LICENSE
```

### Why this structure matters

* [`main.py`](http://main.py) controls the program flow
    
* `recipes/` contains domain logic
    
* `utils/` contains helpers
    
* `__init__.py` enables imports
    

This is how scripts grow into real projects.

---

## Closing Thoughts

Functions are not just Python syntax.  
They shape how you think, design, and debug systems. Strong function fundamentals make learning AI far easier later.

### **Documenting my Full Stack → AI journey, step by step.**

## **By** [**Payal Kumari**](https://www.linkedin.com/in/payalkumari10/)