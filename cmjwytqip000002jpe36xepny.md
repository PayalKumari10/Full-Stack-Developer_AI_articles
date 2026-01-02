---
title: "From Full Stack to AI: Object Oriented Programming in Python"
seoTitle: "From Full Stack to AI: Object Oriented Programming in Python"
seoDescription: "Learn Object Oriented Programming in Python with a focus on AI and machine learning, perfect for full stack developers transitioning to AI"
datePublished: Fri Jan 02 2026 14:24:46 GMT+0000 (Coordinated Universal Time)
cuid: cmjwytqip000002jpe36xepny
slug: from-full-stack-to-ai-object-oriented-programming-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767363547418/6ee41381-237e-45d1-bbb4-114922add2a3.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1767363734111/963ba6fd-d809-49cc-a80c-514eed9325ea.png
tags: ai, technology, python, engineering, community, developer, full-stack, hashnode, articles, technical-writing-1, build-in-public, buildinpublic, payalkumari11, fullstackai, payallearnsai

---

### Introduction

If you are coming from a full stack background and moving toward AI or machine learning, Python quickly becomes your main tool. Most AI libraries are written using Object Oriented Programming. If OOP feels confusing, learning AI will feel harder than it needs to be. This article is a simple walk through of core OOP ideas in Python using small examples and plain language. Think of it as personal learning notes shared openly.

---

## 1\. Building Your First Class and Object

### What is a class

A class is a blueprint. It tells Python how to create something.

### What is an object

An object is a real thing created from that blueprint.

### Code example

```python
class Chai:
    pass

class ChaiTime:
    pass

print(type(Chai))

ginger_tea = Chai()
print(type(ginger_tea))
print(type(ginger_tea) is Chai)
print(type(ginger_tea) is ChaiTime)
```

### Output

```plaintext
<class 'type'>
<class '__main__.Chai'>
True
False
```

### Explanation

* `Chai` itself is a class and its type is `type`
    
* `ginger_tea` is an object created from `Chai`
    
* Python confirms that `ginger_tea` belongs to `Chai`, not `ChaiTime`
    

## 2\. Class and Namespace

### What is a namespace

A namespace is like a storage area where variables live. Classes have their own namespace.

### Code example

```python
class Chai:
    origin = "India"

print(Chai.origin)

Chai.is_hot = True
print(Chai.is_hot)

masala = Chai
print(f"Masala {masala.origin}")
print(f"Masala {masala.is_hot}")

masala.is_hot = False

print("Class:", Chai.is_hot)
print(f"Masala {masala.is_hot}")
```

### Output

```plaintext
India
True
Masala India
Masala True
Class: False
Masala False
```

### Explanation

* `origin` lives inside the class
    
* You can add new attributes like `is_hot` dynamically
    
* `masala` points to the same class, not a new object
    
* Changing it affects the class itself
    

## 3\. Attribute Shadowing

### What is attribute shadowing

When an object creates its own attribute with the same name as the class attribute, it hides the class one.

### Code example

```python
class Chai:
    temperature = "hot"
    strength = "Strong"

cutting = Chai()
print(cutting.temperature)

cutting.temperature = "Mild"
cutting.cup = "small"

print("After changing", cutting.temperature)
print("cup size is", cutting.cup)
print("Direct look into the class", Chai.temperature)

del cutting.temperature
del cutting.cup

print(cutting.temperature)
print(cutting.cup)
```

### Output

```plaintext
hot
After changing Mild
cup size is small
Direct look into the class hot
hot
AttributeError
```

### Explanation

* Initially the object reads from the class
    
* Once you assign `cutting.temperature`, the object gets its own copy
    
* Deleting it reveals the class value again
    
* `cup` never existed on the class, so deleting it fully removes it
    

## 4\. The `self` Argument

### What is `self`

`self` is how an object refers to itself inside a class.

### Code example

```python
class Chaicup:
    size = 150

    def describe(self):
        return f"A {self.size}ml chai cup"

cup = Chaicup()
print(cup.describe())
print(Chaicup.describe(cup))

cup_two = Chaicup()
cup_two.size = 100
print(Chaicup.describe(cup_two))
```

### Output

```plaintext
A 150ml chai cup
A 150ml chai cup
A 100ml chai cup
```

### Explanation

* `cup.describe()` is just a cleaner way to call `Chaicup.describe(cup)`
    
* `self` receives the object automatically
    
* Each object can have its own values
    

## 5\. Constructors and `__init__`

### What is a constructor

A constructor runs when an object is created. It prepares the object.

### Code example

```python
class ChaiOrder:
    def __init__(self, type_, size):
        self.type = type_
        self.size = size

    def summary(self):
        return f"{self.size}ml of {self.type} chai"

order = ChaiOrder("Masala", 200)
print(order.summary())

order_two = ChaiOrder("Ginger", 220)
print(order_two.summary())
```

### Output

```plaintext
200ml of Masala chai
220ml of Ginger chai
```

### Explanation

* `__init__` assigns values at creation time
    
* Every object gets its own data
    
* This pattern is everywhere in real projects
    

## 6\. Inheritance and Composition

### Inheritance

One class builds on another.

### Composition

One class uses another class.

### Code example

```python
class BaseChai:
    def __init__(self, type_):
        self.type = type_

    def prepare(self):
        print(f"Preparing {self.type} chai")

class MasalaChai(BaseChai):
    def add_spices(self):
        print("Adding cardamom, ginger, cloves")

class ChaiShop:
    chai_cls = BaseChai

    def __init__(self):
        self.chai = self.chai_cls("Regular")

    def serve(self):
        print(f"Serving {self.chai.type} chai")
        self.chai.prepare()

class FancyChaiShop(ChaiShop):
    chai_cls = MasalaChai

shop = ChaiShop()
fancy = FancyChaiShop()

shop.serve()
fancy.serve()
fancy.chai.add_spices()
```

### Output

```plaintext
Serving Regular chai
Preparing Regular chai
Serving Regular chai
Preparing Regular chai
Adding cardamom, ginger, cloves
```

### Explanation

* `MasalaChai` inherits behavior from `BaseChai`
    
* `ChaiShop` uses another class internally
    
* This pattern is common in scalable systems
    

## 7\. Three Ways to Access a Base Class

### Code example

```python
class Chai:
    def __init__(self, type_, strength):
        self.type = type_
        self.strength = strength
        print(f"Chai created | Type: {self.type}, Strength: {self.strength}")

class GingerChai(Chai):
    def __init__(self, type_, strength, spice_level):
        super().__init__(type_, strength)
        self.spice_level = spice_level
        print(
            f"GingerChai created | Type: {self.type}, "
            f"Strength: {self.strength}, Spice Level: {self.spice_level}"
        )

chai = GingerChai("Ginger", "Strong", "High")
```

### Output

```plaintext
Chai created | Type: Ginger, Strength: Strong
GingerChai created | Type: Ginger, Strength: Strong, Spice Level: High
```

### Explanation

* `super()` is the cleanest way
    
* It avoids code duplication
    
* It works well with multiple inheritance
    

## 8\. Method Resolution Order (MRO)

### What is MRO

MRO decides which class Python checks first when looking for a method or attribute.

### Code example

```python
class A:
    label = "A: Base class"

class B(A):
    label = "B: Masala blend"

class C(A):
    label = "C: Herbal blend"

class D(B, C):
    pass

cup = D()
print(cup.label)
print(D.__mro__)
```

### Output

```plaintext
B: Masala blend
(<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>)
```

### Explanation

* Python checks from left to right
    
* Understanding MRO avoids strange bugs in large systems
    

## 9\. Static Methods

### What is a static method

A static method belongs to a class but does not use class or object data.

### Code example

```python
class ChaiUtils:
    @staticmethod
    def clean_ingrediants(text):
        return [item.strip() for item in text.split(",")]

raw = "water , milk , honey "
cleaned = ChaiUtils.clean_ingrediants(raw)
print(cleaned)
```

### Output

```plaintext
['water', 'milk', 'honey']
```

### Explanation

* No `self`
    
* No `cls`
    
* Useful for helper logic related to the class
    

## 10\. Classmethod vs Staticmethod

### Code example

```python
class ChaiOrder:
    def __init__(self, tea_type, sweetness, size):
        self.tea_type = tea_type
        self.sweetness = sweetness
        self.size = size

    @classmethod
    def from_dict(cls, order_data):
        return cls(
            order_data["tea_type"],
            order_data["sweetness"],
            order_data["size"],
        )

    @classmethod
    def from_string(cls, order_string):
        tea_type, sweetness, size = order_string.split("-")
        return cls(tea_type, sweetness, size)

class ChaiUtils:
    @staticmethod
    def is_valid_size(size):
        return size in ["Small", "Medium", "Large"]

print(ChaiUtils.is_valid_size("Medium"))

order1 = ChaiOrder.from_dict({"tea_type": "masala", "sweetness": "medium", "size": "Large"})
order2 = ChaiOrder.from_string("Ginger-Low-Small")
order3 = ChaiOrder("Large", "Low", "Large")

print(order1.__dict__)
print(order2.__dict__)
print(order3.__dict__)
```

### Explanation

* `classmethod` creates objects in different ways
    
* `staticmethod` validates or transforms data
    
* Both are common in real APIs
    

## 11\. Property Decorator

### What is a property

It lets you control how attributes are read and written.

### Code example

```python
class TeaLeaf:
    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        return self._age + 2

    @age.setter
    def age(self, age):
        if 1 <= age <= 5:
            self._age = age
        else:
            raise ValueError("Tea Leaf age must be between 1 and 5 years")

leaf = TeaLeaf(2)
print(leaf.age)

leaf.age = 4
print(leaf.age)
```

### Output

```plaintext
4
6
```

### Explanation

* You protect internal data
    
* You add logic without changing how users access it
    
* This is widely used in clean Python design
    

---

## Closing Thoughts

If OOP feels long or repetitive, that is normal. Strong foundations matter more than speed when moving into AI. Most frameworks assume you understand classes, objects, inheritance, and method behavior. Learn step by step, write small experiments, and keep revisiting the basics. Over time, these concepts start to feel natural.

## Documenting my Full Stack → AI journey, step by step.

## **By** [**Payal Kumari**](https://www.linkedin.com/in/payalkumari10/)