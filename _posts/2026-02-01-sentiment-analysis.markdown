---
layout: post
title: Sentiment Analysis Demo, Comparing OpenAI LLMs and TextBlob
date: 2026-02-01 23:24
categories: [blog]
tags: [AI, LangChain]
description:
---

# Sentiment Analysis Demo: Comparing OpenAI LLMs and TextBlob

## Introduction

Sentiment analysis is a classic Natural Language Processing (NLP) task that aims to determine whether a piece of text expresses a **positive**, **negative**, or **neutral** sentiment.

In this blog post, we’ll walk through a simple but practical demo that compares:

- A **Large Language Model (LLM)** from OpenAI  
- A **traditional rule/statistics-based NLP library**, TextBlob  

By the end, you’ll clearly see the strengths and limitations of each approach.

---

## Tools Used

### 1. OpenAI LLM (GPT-style)
- Context-aware
- Handles negation and nuanced language well
- Can provide *explanations* in addition to labels

### 2. TextBlob
- Lightweight and fast
- Polarity-based scoring (`-1.0` to `1.0`)
- Limited understanding of complex phrasing

---

## Step-by-Step Implementation

### Step 1: Import Required Libraries

```python
import openai
import time
from textblob import TextBlob
```

- `openai`: interacts with OpenAI language models  
- `time`: handles timeout-related logic  
- `TextBlob`: performs traditional sentiment analysis  

---

### Step 2: Define the Sentiment Analysis Function

We define a function that:
1. Sends text to an OpenAI model for sentiment analysis
2. Extracts a sentiment label from the LLM output
3. Computes a polarity score using TextBlob
4. Prints and compares the results

```python
def print_sentiment_score(text):
    # --- OpenAI Sentiment Analysis ---
    result = openai.Completion.create(
        engine="gpt-3.5-turbo-instruct",
        prompt=f"Please analyze and give the detailed sentiment of the following text: {text}",
        temperature=0,
        max_tokens=128,
        timeout=10
    )

    openai_sentiment = (
        result.choices[0].text
        .strip()
        .replace("The sentiment of the text is ", "")
        .rstrip(".")
    )

    if "positive" in openai_sentiment.lower():
        openai_sentiment_score = "positive"
    elif "negative" in openai_sentiment.lower():
        openai_sentiment_score = "negative"
    else:
        openai_sentiment_score = "neutral"

    # --- TextBlob Sentiment Analysis ---
    blob = TextBlob(text)
    polarity = blob.sentiment.polarity

    if polarity > 0:
        textblob_sentiment_score = "positive"
    elif polarity < 0:
        textblob_sentiment_score = "negative"
    else:
        textblob_sentiment_score = "neutral"

    # --- Output ---
    print(f"Text: {text}")
    print(f"OpenAI Sentiment Analysis: {openai_sentiment_score} ({openai_sentiment})")
    print(f"TextBlob Sentiment Analysis: {textblob_sentiment_score}")
```

---

### Step 3: Run the Demo

```python
text = "I absolutely do not feel like waking up early in the morning"
print_sentiment_score(text)
```

### Sample Output

```text
Text: I absolutely do not feel like waking up early in the morning
OpenAI Sentiment Analysis: negative (strong dislike and reluctance toward waking up early)
TextBlob Sentiment Analysis: neutral
```

---

## Why the Results Differ

### TextBlob Limitation
TextBlob mainly relies on individual word polarity.  
Phrases like **“absolutely not”** and emotional context are poorly captured.

### OpenAI LLM Advantage
The LLM understands:
- Negation
- Emphasis words
- Emotional intent
- Sentence-level meaning

This makes it far more reliable for real-world sentiment analysis.

---

## When to Use Which?

| Use Case | Recommended Tool |
|--------|------------------|
| Quick prototype | TextBlob |
| Large-scale batch processing | TextBlob |
| User feedback analysis | OpenAI |
| Reviews / surveys / support tickets | OpenAI |
| Emotion-heavy text | OpenAI |

---

## Conclusion

This demo highlights a key takeaway:

> **Traditional NLP tools are fast, but LLMs are far more accurate and expressive.**

If accuracy, context, and explainability matter — LLM-based sentiment analysis is the clear winner.

---

## Next Steps

- Add confidence scores
- Batch process CSV data
- Store results in a database
- Replace legacy sentiment pipelines with LLM-based analysis

Happy building 🚀
