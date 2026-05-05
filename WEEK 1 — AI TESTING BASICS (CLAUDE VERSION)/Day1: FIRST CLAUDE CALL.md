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

## What to observe
We should see a response to  "What is software testing?" printed. 


## Troubleshoot 1: 404 model not found

Anthropic updates model IDs frequently, and there are two important realities:
    - Some models are only available in the web app
    - Some models are available in the API

Hence, if models that are not available to API are used, the 404 will be encountered. 

**Solution**: Ask the API itself for a list of models aviable for API

```
from anthropic import Anthropic
import os
from dotenv import load_dotenv

load_dotenv()

client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))

models = client.models.list()

for model in models.data:
    print(model.id)
```

**Response**: Print of all the models. Replace one of the models in the 'model' variable in our day1.py

```
claude-3-5-sonnet-20241022
claude-3-haiku-20240307
claude-3-opus-20240229
```

