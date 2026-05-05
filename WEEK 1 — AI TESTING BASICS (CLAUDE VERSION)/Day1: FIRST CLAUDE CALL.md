# Day 1 — FIRST CLAUDE CALL

## Goal
We first understand how to talk to Claude programmatically

## What are we going to perform
We are sending a question to Claude model and print the response.

## First Claude API Call - day1.py
```
# Import Claude SDK
from anthropic import Anthropic
import os
from dotenv import load_dotenv

# Load API key from .env file
load_dotenv()

# Create client (connection to Claude)
client = Anthropic(
    api_key=os.getenv("ANTHROPIC_API_KEY")
)

# Send request to AI model
response = client.messages.create(
    model="claude-3-5-sonnet-20240620",  # Claude model

    max_tokens=300,

    messages=[
        {
            "role": "user",
            "content": "What is software testing?"
        }
    ]
)

# Print response
print(response.content[0].text)
```
