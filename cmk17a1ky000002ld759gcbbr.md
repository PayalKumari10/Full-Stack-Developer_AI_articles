---
title: "From Full Stack to AI: Multithreading, Multiprocessing, and the GIL in Python"
seoTitle: "From Full Stack to AI: Multithreading, Multiprocessing, and the GIL in"
seoDescription: "Explore multithreading, multiprocessing, GIL, concurrency, and parallelism in Python to optimize AI and data-heavy tasks"
datePublished: Mon Jan 05 2026 13:32:28 GMT+0000 (Coordinated Universal Time)
cuid: cmk17a1ky000002ld759gcbbr
slug: from-full-stack-to-ai-multithreading-multiprocessing-and-the-gil-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767619756943/0cc2e2f0-970a-4bf2-96d7-5410588623c5.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1767619787777/b1afcb11-e1fd-4986-9f8d-274e548ee747.png
tags: ai, technology, python, engineering, community, developer, full-stack, hashnode, articles, technical-writing-1, build-in-public, buildinpublic, payalkumari11, fullstackai, payallearnsai

---

When you move from Full Stack development into AI or data heavy work, your code starts doing more than handling requests and UI updates. It crunches numbers, downloads large files, trains models, and processes data for long periods.  
That is where understanding concurrency, parallelism, threads, processes, and the GIL becomes important.

---

## What are Concurrency and Parallelism?

### Simple definition

**Concurrency** means handling multiple tasks at the same time by switching between them.  
**Parallelism** means running multiple tasks at the exact same time on different CPU cores.

Concurrency is about structure.  
Parallelism is about actual speed using hardware.

## Concurrency with Multithreading

### What it is

Multithreading lets your program create multiple threads inside the same process.  
Threads share memory and take turns using the CPU.

This works best for tasks that spend time waiting, like network calls or file downloads.

### Example: Taking orders and brewing chai

```python
import threading
import time

def take_orders():
    for i in range(1, 4):
        print(f"Taking order for #{i}")
        time.sleep(2)

def brew_chai():
    for i in range(1, 4):
        print(f"Brewing chai for #{i}")
        time.sleep(3)

order_thread = threading.Thread(target=take_orders)
brew_thread = threading.Thread(target=brew_chai)

order_thread.start()
brew_thread.start()

order_thread.join()
brew_thread.join()

print("All orders taken and chai brewed")
```

### Output

```plaintext
Taking order for #1
Brewing chai for #1
Taking order for #2
Brewing chai for #2
Taking order for #3
Brewing chai for #3
All orders taken and chai brewed
```

### Explanation

Both tasks run together.  
While one thread is waiting, the other keeps working.  
This feels faster even though only one CPU core is active.

Think of a barista taking orders while chai is boiling.

## Parallelism with Multiprocessing

### What it is

Multiprocessing creates separate processes.  
Each process has its own memory and its own Python interpreter.  
This allows true parallel execution on multiple CPU cores.

This is useful for CPU heavy tasks.

### Example: Brewing chai with multiple processes

```python
from multiprocessing import Process
import time

def brew_chai(name):
    print(f"Start of {name} chai brewing")
    time.sleep(3)
    print(f"End of {name} chai brewing")

if __name__ == "__main__":
    chai_makers = [
        Process(target=brew_chai, args=(f"Chai Maker #{i+1}",))
        for i in range(3)
    ]

    for p in chai_makers:
        p.start()

    for p in chai_makers:
        p.join()

    print("All chai served")
```

### Output

```plaintext
Start of Chai Maker #1 chai brewing
Start of Chai Maker #2 chai brewing
Start of Chai Maker #3 chai brewing
End of Chai Maker #1 chai brewing
End of Chai Maker #2 chai brewing
End of Chai Maker #3 chai brewing
All chai served
```

### Explanation

Each process runs independently.  
Your CPU cores are actually working at the same time.

This is real parallelism.

## What is the Global Interpreter Lock (GIL)?

### Simple definition

The GIL is a lock inside CPython that allows only one thread to execute Python bytecode at a time.

Even if you create many threads, only one thread runs Python code at any moment.

### Example: CPU bound work with threads

```python
import threading
import time

def brew_chai():
    print(f"{threading.current_thread().name} started brewing...")
    count = 0
    for _ in range(100_000_000):
        count += 1
    print(f"{threading.current_thread().name} finished brewing...")

thread1 = threading.Thread(target=brew_chai, name="Barista-1")
thread2 = threading.Thread(target=brew_chai, name="Barista-2")

start = time.time()
thread1.start()
thread2.start()
thread1.join()
thread2.join()
end = time.time()

print(f"Total time taken: {end - start:.2f} seconds")
```

### Output

```plaintext
Barista-1 started brewing...
Barista-2 started brewing...
Barista-1 finished brewing...
Barista-2 finished brewing...
Total time taken: 9.8 seconds
```

### Explanation

Even with two threads, the time is almost the same as running one.  
The GIL forces threads to take turns.

Threads do not help with CPU heavy tasks in Python.

## Multiprocessing without the GIL problem

```python
from multiprocessing import Process
import time

def crunch_number():
    print("Started the count process...")
    count = 0
    for _ in range(100_000_000):
        count += 1
    print("Ended the count process...")

if __name__ == "__main__":
    start = time.time()

    p1 = Process(target=crunch_number)
    p2 = Process(target=crunch_number)

    p1.start()
    p2.start()

    p1.join()
    p2.join()

    end = time.time()
    print(f"Total time with multiprocessing: {end - start:.2f} seconds")
```

### Explanation

Each process has its own GIL.  
Now both cores work at the same time.  
This is why multiprocessing is used for AI workloads.

## Threads working together for I/O tasks

### Example: Breakfast preparation

```python
import threading
import time

def boil_milk():
    print("Boiling milk...")
    time.sleep(2)
    print("Milk boiled")

def toast_bun():
    print("Toasting bun...")
    time.sleep(3)
    print("Bun toasted")

start = time.time()

t1 = threading.Thread(target=boil_milk)
t2 = threading.Thread(target=toast_bun)

t1.start()
t2.start()
t1.join()
t2.join()

end = time.time()
print(f"Breakfast ready in {end - start:.2f} seconds")
```

### Explanation

Both tasks wait on time.  
Threads save time by overlapping waiting periods.

## Threads and shared data with Lock

### Why locks matter

Threads share memory.  
Without locks, data can become incorrect.

### Example: Safe counter update

```python
import threading

counter = 0
lock = threading.Lock()

def increment():
    global counter
    for _ in range(100000):
        with lock:
            counter += 1

threads = [threading.Thread(target=increment) for _ in range(10)]

for t in threads:
    t.start()

for t in threads:
    t.join()

print("Final counter:", counter)
```

### Explanation

The lock ensures only one thread updates the counter at a time.  
This prevents race conditions.

## Multiprocessing with Queue

### Sharing data between processes

```python
from multiprocessing import Process, Queue

def prepare_chai(queue):
    queue.put("Masala chai is ready")

if __name__ == "__main__":
    queue = Queue()
    p = Process(target=prepare_chai, args=(queue,))
    p.start()
    p.join()
    print(queue.get())
```

### Explanation

Processes do not share memory.  
Queue helps them communicate safely.

## Multiprocessing with shared Value

```python
from multiprocessing import Process, Value

def increment(counter):
    for _ in range(100000):
        with counter.get_lock():
            counter.value += 1

if __name__ == "__main__":
    counter = Value('i', 0)
    processes = [Process(target=increment, args=(counter,)) for _ in range(4)]

    for p in processes:
        p.start()

    for p in processes:
        p.join()

    print("Final counter value:", counter.value)
```

### Explanation

Value allows controlled shared state across processes.  
Locks are still needed.

## Concurrency vs Parallelism

### What is Concurrency?

Concurrency means **dealing with more than one task at the same time**, but not necessarily running them at the exact same moment.

The program switches between tasks.  
When one task is waiting, another one runs.

Example:  
Your program downloads a file and waits for a response.  
While waiting, it can handle another task.

Concurrency is about **managing tasks efficiently**.

### What is Parallelism?

Parallelism means **running multiple tasks at the same time** on different CPU cores.

Tasks truly execute together.

Example:  
Two CPU cores crunching numbers at the same time.

Parallelism is about **doing work faster using hardware**.

### Simple way to remember

Concurrency: tasks take turns  
Parallelism: tasks run together

In Python:  
• Concurrency is commonly done with threads  
• Parallelism is achieved with multiprocessing

## Thread vs Process

### What is a Thread?

A thread is a lightweight unit of execution inside a process.

Threads:  
• Share the same memory  
• Start quickly  
• Are good for I/O tasks like network calls and file operations  
• Are affected by the GIL in Python

If one thread changes shared data, others see it immediately.

### What is a Process?

A process is an independent program with its own memory space.

Processes:  
• Do not share memory by default  
• Use more system resources  
• Can run on multiple CPU cores  
• Are not limited by the GIL

They are ideal for CPU heavy tasks like data processing or model training.

### Simple way to remember

Thread: workers in the same room  
Process: workers in separate rooms

Threads share tools.  
Processes bring their own tools.

### Why this matters for AI learners

If you use threads for CPU heavy work, your code will not get faster due to the GIL.  
If you use processes, your code can actually scale across cores.

Knowing when to use what saves time, confusion, and performance issues later.

---

## Closing thoughts

If you are moving into AI, do not rush this topic.  
Understand when threads help and when they do not.  
Learn why multiprocessing matters for CPU heavy work.  
The GIL is not a bug. It is a design choice.

Strong foundations make advanced topics easier later.

Take it step by step.

## Documenting my Full Stack → AI journey, step by step.

## **By** [**Payal Kumari**](https://www.linkedin.com/in/payalkumari10/)