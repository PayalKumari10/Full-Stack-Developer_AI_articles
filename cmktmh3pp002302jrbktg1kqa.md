---
title: "From Full Stack to AI: Running LLMs vs Hugging Face Hub"
seoTitle: "From Full Stack to AI: Running LLMs vs Hugging Face Hub"
seoDescription: "Explore running large language models locally vs using Hugging Face Hub, accessible insights for transitioning from Full Stack development to AI"
datePublished: Sun Jan 25 2026 10:55:25 GMT+0000 (Coordinated Universal Time)
cuid: cmktmh3pp002302jrbktg1kqa
slug: from-full-stack-to-ai-running-llms-vs-hugging-face-hub
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1769338309112/048c44f1-1b8e-49db-b0da-28bc8207f18f.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1769338382697/a13e308e-c365-4d61-9920-294a8846f367.png
tags: ai, technology, python, developers, developer, full-stack, hashnode, articles, technical-writing-1, build-in-public, promptengineering, technology-trends, payalkumari11, fullstackai, payallearnsai

---

When you move from Full Stack development into AI, one common question comes up early.  
Should I run models locally or use platforms like Hugging Face Hub?

As backend developers, we are used to choosing between self hosted services and managed platforms. LLMs follow the same idea. This chapter focuses on understanding Hugging Face Hub, how models are accessed, and how they actually run in practice using Python.

This is written as a learning note, not a guide to everything. Just enough to understand what is happening and why it matters.

---

### What is Hugging Face Hub in simple words

Hugging Face Hub is like GitHub for AI models.

Instead of code repositories, it hosts:

* Pretrained models
    
* Model configs
    
* Tokenizers
    
* Documentation
    

You do not train the model yourself. You download it and use it in your application.

For Full Stack developers, this feels similar to using an npm package or a PyPI library instead of writing everything from scratch.

## 1) Hugging Face Model Deployment – Section Intro

### What this means

When we talk about Hugging Face model deployment, we are mostly talking about:

* Downloading a model from the Hub
    
* Running it inside our Python environment
    
* Using it through code, not a web UI
    

This is different from running a local runtime like Ollama. Here, Python controls the model directly.

### Why it matters

This approach is useful when:

* You want more control over model behavior
    
* You are building research or prototype features
    
* You want direct access to model outputs
    

## 2) Configuring and Securing Hugging Face Account

### What this step is

Some models on Hugging Face require authentication.  
To access them, you need a Hugging Face account and an access token.

This token works like an API key.

### Why it matters

Without proper authentication:

* Private or gated models will not download
    
* Your scripts will fail silently or throw errors
    

### Example: Logging in using CLI

```bash
huggingface-cli login
```

### Output

```bash
Token has been saved to ~/.huggingface/token
```

### Explanation

You paste your token once. After that, Python tools can download models securely.

Real world example:  
This is similar to logging in with AWS CLI before accessing S3 buckets.

## 3) Accessing Instruct-Tuned Models (Google Gemma)

### What are instruct-tuned models

Instruct-tuned models are trained to follow instructions clearly.

Instead of just predicting text, they respond to tasks like:

* Explain this
    
* Describe that
    
* Answer like a helpful assistant
    

Google Gemma Instruct models are designed for this purpose.

### Why they matter

As developers, we usually want:

* Clear responses
    
* Task based outputs
    
* Less prompt engineering
    

Instruct models make this easier.

## 4) Installing and Using Hugging Face CLI Tools

### What the CLI does

The Hugging Face CLI helps you:

* Authenticate
    
* Download models
    
* Manage local cache
    

It saves time and avoids manual downloads.

### Example: Installing dependencies

```bash
pip install transformers torch huggingface_hub
```

### Output

```bash
Successfully installed transformers huggingface_hub
```

### Explanation

These libraries allow Python to:

* Fetch models from Hugging Face
    
* Load them into memory
    
* Run inference
    

Think of this like installing Express before building an API.

## 5) Model Downloading and Execution from Hugging Face Hub

### What is happening here

Now we actually load a model and run it using Python code.

The pipeline API handles:

* Model loading
    
* Tokenization
    
* Inference
    

You focus on input and output.

### Code example

```python
from transformers import pipeline

pipe = pipeline(
    "image-text-to-text",
    model="google/gemma-3-4b-it"
)

messages = [
    {
        "role": "user",
        "content": [
            {"type": "image", "url": "https://huggingface.co/datasets/Narsil/image_dummy/raw/main/parrots.png"},
            {"type": "text", "text": "Describe the image in detail."}
        ]
    }
]

output = pipe(messages)
print(output)
```

### Output

```bash
[{'generated_text': 'The image shows two colorful parrots sitting on a branch...'}]
```

### Explanation of the output

Step by step:

* Python downloads the Gemma model if not already cached
    
* The image and text are sent together as input
    
* The model processes both inputs
    
* It generates a natural language description
    
* The result is returned as structured data
    

You can now store it, return it from an API, or show it in a UI.

Real world example:  
This is similar to sending a request to a service and receiving structured JSON back.

## Running LLMs locally vs Hugging Face Hub

In simple terms:

* Hugging Face Hub gives you direct control inside Python
    
* Local runtimes feel more like backend services
    
* Hugging Face is great for experiments and research
    
* Local runtimes are easier for production APIs
    

Both approaches are valid. The choice depends on your use case.

### Closing Thoughts

Moving into AI does not require learning everything at once.  
If you already understand APIs, environments, and services, you are closer than you think.

Start small. Load a model. Run it. Observe the output.  
Strong foundations are built step by step.

---

## 𝐃𝐨𝐜𝐮𝐦𝐞𝐧𝐭𝐢𝐧𝐠 𝐦𝐲 𝐅𝐮𝐥𝐥 𝐒𝐭𝐚𝐜𝐤 𝐭𝐨 𝐀𝐈 𝐣𝐨𝐮𝐫𝐧𝐞𝐲, 𝐬𝐭𝐞𝐩 𝐛𝐲 𝐬𝐭𝐞𝐩.

## **By** [**Payal Kumari**](https://www.linkedin.com/in/payalkumari10/)