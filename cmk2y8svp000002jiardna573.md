---
title: "From Full Stack to AI: Asyncio in Python"
seoTitle: "From Full Stack to AI: Asyncio in Python"
seoDescription: "Asyncio enhances Python's efficiency for AI tasks in networking and file handling, aiding the transition from full stack development"
datePublished: Tue Jan 06 2026 18:55:06 GMT+0000 (Coordinated Universal Time)
cuid: cmk2y8svp000002jiardna573
slug: from-full-stack-to-ai-asyncio-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767725514423/0d9e55fe-c297-40bb-9269-13cd0ca1c16a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1767725588562/f5410157-05e5-48b9-a737-afffa11f3b9b.png
tags: ai, technology, python, engineering, community, developer, full-stack, hashnode, articles, technical-writing-1, build-in-public, buildinpublic, payalkumari11, fullstackai, payallearnsai

---

When you move from full stack development toward AI or data heavy systems, you start dealing with tasks that wait a lot. Network calls, reading files, talking to APIs, checking system state. If you write this code in a blocking way, everything feels slow and wasteful. Asyncio helps you handle many waiting tasks efficiently using a single thread. That is why it matters.

---

## What is asyncio in simple words

Asyncio is a built in Python library that helps you run many tasks that spend time waiting, without blocking the program.

It does not run code in parallel like multiprocessing.  
It cooperates between tasks and switches when one task is waiting.

Think of it like cooking chai.  
While water is boiling, you clean cups.  
You do not stand still staring at the stove.

## Core ideas you must understand

### Coroutine

A coroutine is a special function that can pause and resume.

You define it using `async def`.

```python
import asyncio

async def brew_chai():
    print("Brewing chai...")
    await asyncio.sleep(2)
    print("Chai is ready")

asyncio.run(brew_chai())
```

### Output

```plaintext
Brewing chai...
Chai is ready
```

### Explanation

The function starts.  
It reaches `await asyncio.sleep(2)` and pauses.  
After 2 seconds, it resumes and finishes.

Nothing else is blocked during the wait.

## await keyword

`await` tells Python:  
Pause here and let other async tasks run.

You can only use `await` inside an async function.

Blocking version vs non blocking version.

```python
import time
time.sleep(2)
```

This blocks everything.

```python
await asyncio.sleep(2)
```

This pauses only the current coroutine.

## Event loop

The event loop is the engine behind asyncio.

It keeps track of:

* which coroutine is running
    
* which one is waiting
    
* which one is ready to continue
    

You usually do not manage it manually.

```python
asyncio.run(main())
```

This creates an event loop, runs your async code, then closes it.

## Running multiple coroutines together

### Example with gather

```python
import asyncio
import time

async def brew(name):
    print(f"Brewing {name}...")
    await asyncio.sleep(3)
    time.sleep(3)
    print(f"{name} is ready")

async def main():
    await asyncio.gather(
        brew("Masala chai"),
        brew("Green chai"),
        brew("Ginger chai"),
    )

asyncio.run(main())
```

### Output

```plaintext
Brewing Masala chai...
Brewing Green chai...
Brewing Ginger chai...
Masala chai is ready
Green chai is ready
Ginger chai is ready
```

### Explanation

All three start together.  
But `time.sleep(3)` blocks the event loop.  
So even though this is async code, the blocking call slows everything.

Rule to remember:  
Never mix blocking calls inside async functions.

## Async HTTP requests with aiohttp

This is a real world use case.

```python
import asyncio
import aiohttp

async def fetch_url(session, url):
    async with session.get(url) as response:
        print(f"Fetched {url} with status {response.status}")

async def main():
    urls = ["https://httpbin.org/delay/2"] * 3
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_url(session, url) for url in urls]
        await asyncio.gather(*tasks)

asyncio.run(main())
```

### Output

```plaintext
Fetched https://httpbin.org/delay/2 with status 200
Fetched https://httpbin.org/delay/2 with status 200
Fetched https://httpbin.org/delay/2 with status 200
```

### Explanation

All three requests wait on the network at the same time.  
One thread handles all of them efficiently.

This pattern is common in data collection, scraping, and AI pipelines.

## Mixing threads with asyncio

Some libraries are blocking and cannot be async.  
You can offload them to threads.

```python
import asyncio
import time
from concurrent.futures import ThreadPoolExecutor

def check_stock(item):
    print(f"Checking {item} in store...")
    time.sleep(3)
    return f"{item} stock: 42"

async def main():
    loop = asyncio.get_running_loop()
    with ThreadPoolExecutor() as pool:
        result = await loop.run_in_executor(pool, check_stock, "Masala Chai")
        print(result)

asyncio.run(main())
```

### Output

```plaintext
Checking Masala Chai in store...
Masala Chai stock: 42
```

### Explanation

The blocking function runs in a separate thread.  
The event loop stays responsive.

Use this when you cannot avoid blocking code.

## Asyncio with multiprocessing

For CPU heavy tasks, threads are not enough.  
Use processes.

```python
import asyncio
from concurrent.futures import ProcessPoolExecutor

def encrypt(data):
    return data[::-1]

async def main():
    loop = asyncio.get_running_loop()
    with ProcessPoolExecutor() as pool:
        result = await loop.run_in_executor(pool, encrypt, "credit_card_93345")
        print(result)

if __name__ == "__main__":
    asyncio.run(main())
```

### Output

```plaintext
54339_drac_tiderc
```

### Explanation

The encryption runs in another process.  
This bypasses the GIL and uses multiple CPU cores.

## Asyncio with background threads

```python
import asyncio
import threading
import time

def background_worker():
    while True:
        time.sleep(1)
        print("Logging the system health")

async def fetch_orders():
    await asyncio.sleep(3)
    print("Order fetched")

threading.Thread(target=background_worker, daemon=True).start()
asyncio.run(fetch_orders())
```

### Output

```plaintext
Logging the system health
Logging the system health
Order fetched
```

### Explanation

The background thread runs continuously.  
The async task runs independently.

This is useful for monitoring and logging.

## Daemon vs non daemon threads

### What is a daemon thread

A daemon thread is a background helper thread.

It runs only to support the main program.  
When the main program finishes, the daemon thread is stopped automatically.

You do not need to manually stop it.

Think of it like background music in an app.  
When the app closes, the music stops.

```python
import threading
import time

def monitor_tea_temp():
    while True:
        print("Monitoring tea temperature")
        time.sleep(2)

t = threading.Thread(target=monitor_tea_temp, daemon=True)
t.start()

print("Main program done")
```

### Output

```plaintext
Monitoring tea temperature
Main program done
```

### Explanation

The background thread starts.  
As soon as the main program finishes, Python exits.  
The daemon thread is stopped automatically.

### What is a non-daemon thread

A non daemon thread is a normal working thread.

Python waits for it to finish before exiting the program.  
If it runs forever, the program will not stop.

Think of it like boiling milk.  
You must turn off the stove before leaving.

```python
import threading
import time

def monitor_tea_temp():
    while True:
        print("Monitoring tea temperature...")
        time.sleep(2)

t = threading.Thread(target=monitor_tea_temp)
t.start()

print("Main program done")

```

### Output

```plaintext
Monitoring tea temperature
Monitoring tea temperature
Monitoring tea temperature
```

### Explanation

The main program finishes.  
But Python keeps running because the thread is still alive.  
You must stop it manually. Program does not exit.

### Notes:-

Daemon threads stop when the main program exits.  
Non daemon threads keep the program alive.

## Debugging issues: race condition

```python
import threading

chai_stock = 0

def restock():
    global chai_stock
    for _ in range(100000):
        chai_stock += 1

threads = [threading.Thread(target=restock) for _ in range(2)]

for t in threads: t.start()
for t in threads: t.join()

print("Chai stock:", chai_stock)
```

### Output

```plaintext
Chai stock: 173421
```

Expected value is 200000.

### Explanation

Both threads update the same variable at the same time.  
Updates are lost.

This is called a race condition.

## Deadlock example

```python
import threading

lock_a = threading.Lock()
lock_b = threading.Lock()

def task1():
    with lock_a:
        print("Task 1 acquired lock a")
        with lock_b:
            print("Task 1 acquired lock b")

def task2():
    with lock_b:
        print("Task 2 acquired lock b")
        with lock_a:
            print("Task 2 acquired lock a")

t1 = threading.Thread(target=task1)
t2 = threading.Thread(target=task2)

t1.start()
t2.start()
```

### What happens

Both threads wait forever.  
Each holds one lock and waits for the other.

That is a deadlock.

---

## Closing thoughts

Asyncio is not hard, but it requires discipline.  
Avoid blocking calls.  
Understand what runs where.  
Use threads for blocking IO.  
Use processes for CPU heavy work.

If you are moving into AI, these basics matter more than fancy tools.  
Build step by step.  
Strong foundations always pay off.

## Documenting my Full Stack → AI journey, step by step.

## By [Payal Kumari](https://www.linkedin.com/in/payalkumari10/)