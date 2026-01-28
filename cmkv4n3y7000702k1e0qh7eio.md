---
title: "From Full Stack to AI: Building AI Agents and Agentic Workflow"
seoTitle: "From Full Stack to AI: Building AI Agents and Agentic Workflow"
seoDescription: "Learn to build AI agents from scratch using Python, transitioning from full stack to AI development with agentic workflows"
datePublished: Mon Jan 26 2026 12:11:44 GMT+0000 (Coordinated Universal Time)
cuid: cmkv4n3y7000702k1e0qh7eio
slug: from-full-stack-to-ai-building-ai-agents-and-agentic-workflow
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769429160703/1880d7d2-a0e0-42bd-923b-1cd55c61f62e.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1769429366361/bad14d13-d31c-453f-8a02-7b58917f9e57.png
tags: ai, technology, python, developers, developer, hashnode, articles, technical-writing-1, build-in-public, technology-trends, rag, aiagents, payalkumari11, fullstackai, payallearnsai

---

As a full stack developer moving into AI, one confusing shift is moving from simple request response APIs to systems that can think in steps, make decisions, and use tools.  
This is where AI agents come in.

This article is my learning note on building a basic AI agent and an agent style workflow using Python. Nothing fancy. Just clear ideas, simple code, and practical understanding.

---

### What is an agentic workflow in simple words

An agentic workflow is a way of building AI systems that do more than just reply to a prompt.

Instead of answering immediately, the system:

* Thinks about what needs to be done
    
* Breaks the task into steps
    
* Uses tools if needed
    
* Then gives a final answer
    

For a Full Stack developer, this feels similar to a controller that decides which service to call before returning a response.

## 1) Agentic AI Fundamentals – Section Intro

### What this means

Agentic AI focuses on behavior, not just output.

The model does not only generate text.  
It plans actions and executes them step by step.

### Why it matters

As applications grow:

* One prompt is not enough
    
* Decisions need structure
    
* Tools need to be used safely
    

Agentic workflows make AI systems more predictable and easier to debug.

## 2) What Exactly Are AI Agents? (Core Concepts)

### What is an AI agent

An AI agent is a program that:

* Receives a goal
    
* Thinks about how to reach it
    
* Uses tools when needed
    
* Produces a final result
    

The key idea is that thinking and acting are separated.

### Real world example

Think of a backend request:

* First you validate input
    
* Then decide business logic
    
* Then call services
    
* Then return a response
    

An AI agent works in a similar way.

## 3) Coding Your First AI Agent

### What we are building

We will build a simple loop based agent that:

* Takes user input
    
* Lets the model decide what step comes next
    
* Calls tools when required
    
* Stops when output is ready
    

### Environment setup

```python
from dotenv import load_dotenv
from openai import OpenAI
import requests
import os
import json

load_dotenv()
client = OpenAI()
```

### Explanation

* `load_dotenv` loads API keys
    
* `OpenAI` is the client used to talk to the model
    
* Other imports help with tools and system calls
    

### Tool functions

```python
def run_command(cmd: str):
    result = os.system(cmd)
    return result

def get_weather(city: str):
    url = f"https://wttr.in/{city.lower}?format=%C+%t"
    response = requests.get(url)

    if response.status_code == 200:
        return f"The weather in {city} is: {response.text}"
    
    return "Something went wrong"
```

### Explanation

These tools let the agent:

* Run system commands
    
* Fetch weather data
    

This is similar to exposing internal services to a controller.

## 4) Enforcing Structured Outputs with Pydantic

### Why structure matters

Without structure, AI responses can be unpredictable.  
Structured output keeps things safe and readable.

### Pydantic model

```python
from pydantic import BaseModel, Field
from typing import Optional

class MyOutputFormat(BaseModel):
    step: str = Field(..., description="The step name like PLAN or OUTPUT")
    content: Optional[str] = None
    tool: Optional[str] = None
    input: Optional[str] = None
```

### Explanation

This ensures every response has:

* A step name
    
* Optional content
    
* Tool info if needed
    

Think of it like validating API responses.

## 5) Building a CLI Coding Agent from Scratch

### System prompt

```python
SYSTEM_PROMPT = """
You are an expert AI assistant.
You work in START, PLAN, and OUTPUT steps.
You may use tools if required.
Always respond in structured JSON format.
"""
```

### Explanation

The system prompt defines rules for the agent.  
This is similar to middleware rules in backend systems.

### Message history

```python
message_history = [
    {"role": "system", "content": SYSTEM_PROMPT}
]
```

This keeps context between steps.

### Main agent loop

```python
while True:
    user_query = input("👉 ")
    message_history.append({"role": "user", "content": user_query})

    response = client.chat.completions.parse(
        model="gpt-4o-mini",
        response_format={"type": "json_object"},
        messages=message_history
    )

    parsed_result = response.choices[0].message.parsed
```

### Explanation

* User gives input
    
* Model decides next step
    
* Output is parsed as structured data
    

### Handling steps

```python
if parsed_result.step == "PLAN":
    print(parsed_result.content)

if parsed_result.step == "TOOL":
    tool_response = available_tools[parsed_result.tool](parsed_result.input)
    message_history.append({
        "role": "developer",
        "content": json.dumps({
            "step": "OBSERVE",
            "output": tool_response
        })
    })

if parsed_result.step == "OUTPUT":
    print(parsed_result.content)
    break
```

### Explanation

* PLAN shows reasoning
    
* TOOL executes actions
    
* OUTPUT ends the loop
    

This is the heart of the agentic workflow.

## Chain of Thought Prompting (Simple View)

### What it is

Chain of thought means letting the model think in steps instead of jumping to an answer.

### Why it helps

* Easier debugging
    
* Better accuracy
    
* More control
    

This works well when combined with structured output.

---

### Closing Thoughts

Building AI agents feels complex at first, but the ideas are familiar.  
Planning, validation, tools, and clear outputs are things full stack developers already understand.

Start small.  
Add structure.  
Let the system think step by step.

Strong AI systems are built the same way good backend systems are built. Slowly and clearly.

## 𝐃𝐨𝐜𝐮𝐦𝐞𝐧𝐭𝐢𝐧𝐠 𝐦𝐲 𝐅𝐮𝐥𝐥 𝐒𝐭𝐚𝐜𝐤 𝐭𝐨 𝐀𝐈 𝐣𝐨𝐮𝐫𝐧𝐞𝐲, 𝐬𝐭𝐞𝐩 𝐛𝐲 𝐬𝐭𝐞𝐩.

## **By** [**Payal Kumari**](https://www.linkedin.com/in/payalkumari10/)