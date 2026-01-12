---
title: "From Full Stack to AI: API Setup and Integration"
seoTitle: "From Full Stack to AI: API Setup and Integration"
seoDescription: "Integrate AI APIs with Python, set up OpenAI and Google Gemini accounts, and connect AI models to systems seamlessly"
datePublished: Mon Jan 12 2026 10:04:46 GMT+0000 (Coordinated Universal Time)
cuid: cmkazxwh1000e02jl2g7sh4jm
slug: from-full-stack-to-ai-api-setup-and-integration
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1768212040908/b4044a9a-f6a6-4a67-ab1b-d185cdbc1fac.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1768212116687/5b3b887f-248f-4405-8ab7-3bb635d4330a.png
tags: ai, technology, python, developers, developer, full-stack, hashnode, articles, technical-writing-1, build-in-public, technology-trends, buildinpublic, payalkumari11, fullstackai, payallearnsai

---

When you move from full stack development into AI, the first real hands on step is calling an AI model from your own code. This chapter focuses on exactly that. No theory, no heavy math. Just setting up accounts, connecting APIs, and getting real responses using Python.

If you have built REST APIs or consumed third party services before, this will feel familiar. AI APIs work in a very similar way.

---

## 1\. Configuring Your OpenAI Account

### What this step means

Before you can call OpenAI models, you need an API key. This key is how OpenAI knows who you are and how to bill usage. Think of it like any other service API key you have used before.

### What you need to do

1. Create an OpenAI account
    
2. Generate an API key from the dashboard
    
3. Store the key securely, usually as an environment variable
    

Example using environment variables:

```bash
export OPENAI_API_KEY="your_api_key_here"
```

### Why this matters

Hard coding API keys inside code is risky. Using environment variables keeps your credentials safe and follows good engineering practices.

## 2\. Invoking OpenAI APIs with Python

### What this does

This is the simplest possible example of sending text to an OpenAI model and getting a response back. The model reads your input and generates text based on it.

### Python example

```python
from openai import OpenAI

client = OpenAI()

response = client.responses.create(
    model="gpt-5.2",
    input="Write a short bedtime story about a unicorn."
)

print(response.output_text)
```

### Example output

```plaintext
Once upon a time, a gentle unicorn lived in a quiet forest and helped other animals feel safe at night.
```

### Explanation

You sent a plain text prompt to the model. The model predicted the next tokens and returned a complete response. The `output_text` field already combines everything into readable text.

If you have used APIs like Stripe or Firebase, this pattern should feel very familiar.

## 3\. Creating and Setting Up a Google Gemini Account

### What is Gemini?

Gemini is Google’s family of generative AI models. Like OpenAI models, Gemini can generate text, explain concepts, and answer questions.

### Basic Python example

```python
from google import genai

client = genai.Client()

response = client.models.generate_content(
    model="gemini-3-flash-preview",
    contents="Explain how AI works in a few words"
)

print(response.text)
```

### Example output

```plaintext
AI learns patterns from data and uses them to make predictions or generate responses.
```

### Explanation

The flow is very similar to OpenAI. You create a client, send a prompt, and read the generated text. Once you understand one API, learning the other becomes much easier.

## 4\. Using Google Gemini with OpenAI Compatible APIs

### Why this is useful

Some teams already use the OpenAI SDK in their projects. Gemini now supports OpenAI compatible APIs, which means you can switch models without rewriting all your code.

### Python example using OpenAI style syntax

```python
from openai import OpenAI

client = OpenAI(
    api_key="gemini_api_key",
    base_url="https://generativelanguage.googleapis.com/v1beta/"
)

response = client.chat.completions.create(
    model="gemini-1.5-flash",
    n=1,
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {
            "role": "user",
            "content": "Explain to me how AI works"
        }
    ]
)

print(response.choices[0].message)
```

### Example output

```plaintext
AI works by learning patterns from data and using those patterns to generate answers or predictions.
```

### Explanation

Here, the OpenAI client is used to talk to Gemini instead of OpenAI. The only changes are the API key, base URL, and model name. This makes experimentation much easier for developers.

---

## Closing Thoughts

API setup and integration is where AI starts feeling real for full stack developers. Once you can call a model, you can plug AI into forms, APIs, dashboards, and background jobs.

Learn one provider well, understand the request and response flow, and then explore others. Strong foundations make everything else easier.

## **Documenting my Full Stack → AI journey, step by step.**

## By [Payal Kumari](https://www.linkedin.com/in/payalkumari10/)