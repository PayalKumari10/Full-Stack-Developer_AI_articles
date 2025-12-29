---
title: "Python Data Types: A Beginner's Guide"
seoTitle: "Python Data Types: A Beginner's Guide"
seoDescription: "Learn Python data types: mutable/immutable objects, float precision, strings, lists, sets, dictionaries. Perfect for beginners!"
datePublished: Mon Dec 22 2025 15:26:53 GMT+0000 (Coordinated Universal Time)
cuid: cmjhb78ta000202kz98jh6osm
slug: python-data-types-a-beginners-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1766416940607/ca18fa14-26b4-46b9-bbcc-06499684dc90.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1766417150974/145c00ea-1cd4-4f43-92ca-e47b4ed31fe6.png
tags: ai, technology, python, engineering, community, developer, full-stack, hashnode, articles, technical-writing-1, build-in-public, buildinpublic, payalkumari11, fullstackai, payallearnsai

---

---

### 1\. Objects in Python: Mutable and Immutable

In Python, **everything is an object**.  
Some objects can be changed after creation, and some cannot.

#### Immutable objects

Immutable objects cannot be changed in place. Numbers are a good example.

```python
sugar_amount = 2
print(f"Initial sugar: {sugar_amount}")

sugar_amount = 12
print(f"Second Initial sugar: {sugar_amount}")

print(f"ID of 2: {id(2)}")
print(f"ID of 12: {id(12)}")
```

```plaintext
Output:
Initial sugar: 2
Second Initial sugar: 12
ID of 2: 140734340625352
ID of 12: 140734340625672
```

Here, changing the value creates a **new object**, not a modification of the old one.  
That is why the memory ID changes.

#### Mutable objects

Mutable objects can be changed without creating a new object.

```python
spice_mix = set()
print(f"Initial spice mix id: {id(spice_mix)}")
print(f"Initial spice mix: {spice_mix}")

spice_mix.add("Turmeric")
spice_mix.add("Cumin")
spice_mix.add("Coriander")

print(f"Spice mix after additions: {spice_mix}")
print(f"Spice mix after additions id: {id(spice_mix)}")
```

```plaintext
Output:
Initial spice mix: set()
Spice mix after additions: {'Coriander', 'Cumin', 'Turmeric'}
```

The ID stays the same because the object itself is modified.

Understanding this difference is very important when working with data in Python.

### 2\. Numbers, Booleans, and Operators

Python supports all common numeric operations and handles them cleanly.

```python
black_tea_grams = 14
ginger_gram = 2

total_grams = black_tea_grams + ginger_gram
print(total_grams)

remaining_tea = black_tea_grams - ginger_gram
print(remaining_tea)

milk_litres = 7
servings = 4
print(milk_litres / servings)

print(7 // 4)
print(10 % 3)
print(2 ** 3)

total_tea_leaves = 1_000_000_000
print(total_tea_leaves)
```

```plaintext
Output:
16
12
1.75
1
1
8
1000000000
```

Booleans also interact with numbers in Python.

```python
is_boiling = True
stir_count = 5

total_actions = stri_count + is_boiling
print(total_actions)

milk_present = 11
print(bool(milk_present))

water_hot = True
tea_added = True
print(water_hot and tea_added)
```

```plaintext
Output:
6
True
True
```

This automatic type behavior is powerful, but it also means we must be careful.

### 3\. Floating Point Precision

Floating point numbers are not always exact.

```python
import sys

ideal_temp = 95.5
current_temp = 95.49999999999

print(ideal_temp - current_temp)
print(sys.float_info)
```

This explains why Python provides tools like `Decimal` and `Fraction` when precision matters.

### 4\. Strings: Indexing, Slicing, and Encoding

Strings in Python are immutable and very powerful.

```python
chai_type = "Masala Chai"
customer_name = "Alex"

print(f"Order for {customer_name}: {chai_type}")

chai_description = "Aromatic and Bold"
print(chai_description[:8])
print(chai_description[12:])
print(chai_description[::-1])
```

```plaintext
Output:
Order for Alex: Masala Chai
Aromatic
Bold
dloB dna citaromA
```

Encoding and decoding are important when working with text data.

```python
label_text = "Chai Special"
encoded_label = label_text.encode("utf-8")
print(encoded_label)

decoded_label = encoded_label.decode("utf-8")
print(decoded_label)
```

### 5\. Tuples and Membership Testing

Tuples are ordered and immutable.

```python
masala_spices = ("cardamom", "cloves", "cinnamon")

(spice1, spice2, spice3) = masala_spices
print(spice1, spice2, spice3)

ginger_ratio, cardamom_ratio = 2, 1
ginger_ratio, cardamom_ratio = cardamom_ratio, ginger_ratio
print(ginger_ratio, cardamom_ratio)

print("ginger" in masala_spices)
print("cardamom" in masala_spices)
```

Tuples are great when data should not change.

### 6\. Lists, Operator Overloading, and Bytearray

Lists are mutable and widely used.

```python
ingredients = ["water", "milk"]
ingredients.extend(["ginger", "cardamom"])
ingredients.insert(2, "black tea")

print(ingredients)

ingredients.pop()
ingredients.reverse()
ingredients.sort()
print(ingredients)
```

Operators behave differently based on data types.

```python
print(["black tea", "water"] * 3)
```

Bytearray allows modification of raw bytes.

```python
raw_spice_data = bytearray(b"CINNAMON")
raw_spice_data = raw_spice_data.replace(b"CINNAMON", b"CARDAMOM")
print(raw_spice_data)
```

### 7\. Sets and Frozensets

Sets store unique values and support powerful operations.

```python
essential_spices = {"cinnamon", "nutmeg", "cloves"}
optional_spices = {"cardamom", "star anise", "fennel"}

print(essential_spices | optional_spices)
print(essential_spices & optional_spices)
print(essential_spices - optional_spices)
```

```plaintext
Output:
{'cinnamon', 'nutmeg', 'cloves', 'cardamom', 'star anise', 'fennel'}
set()
{'cinnamon', 'nutmeg', 'cloves'}
```

Sets are very useful for comparisons and filtering.

### 8\. Dictionaries in Python

Dictionaries store data as key-value pairs.

```python
chai_order = {"type": "Ginger Chai", "sugar_level": 2, "milk": False}
print(chai_order)

chai_recipe = {}
chai_recipe["base"] = "black tea"
chai_recipe["liquid"] = "milk"

del chai_recipe["liquid"]
print(chai_recipe)

print(chai_order.get("size", "NO Note"))

last_item = chai_order.popitem()
print(last_item)
```

```plaintext
Output:
{'type': 'Ginger Chai', 'sugar_level': 2, 'milk': False}
{'base': 'black tea'}
NO Note
('milk', False)
```

Dictionaries are one of the most used data structures in real applications.

### 9\. Touch on Advanced Data Types

Python provides powerful tools in the standard library.

```python
import arrow
from collections import namedtuple

brewing_time = arrow.utcnow()
brewing_time.to("Asia/Kolkata")

chaiProfile = namedtuple(
    "chaiProfile",
    ["tea_type", "milk_type", "sweetener", "spices"]
)
```

These tools help write clean and structured code without extra complexity.

---

### Closing Thoughts

This chapter helped me understand how Python handles data at a deeper level.

Nothing here is about memorizing syntax.  
It is about understanding how data behaves and why Python feels so natural once you understand its basics.

As a Full Stack Developer moving into AI, this foundation matters a lot.  
More learning and more chapters coming next.

### **Documenting my Full Stack → AI journey, step by step.**

## **By** [**Payal Kumari**](https://www.linkedin.com/in/payalkumari10/)