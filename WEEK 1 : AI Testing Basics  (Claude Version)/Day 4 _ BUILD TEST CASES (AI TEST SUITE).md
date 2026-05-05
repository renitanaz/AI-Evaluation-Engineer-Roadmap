#[ Day 4 ]  Build Test Cases (AI Test Suite)

## Goal
Create a structured set of test cases and execute them against an AI model.



## Overview
Instead of testing a single prompt, this step introduces a **test suite**—a collection of prompts that simulate QA-style test cases. Each prompt is executed sequentially, and the model’s responses are captured.



## Approach
- Define multiple prompts representing test scenarios  
- Iterate through each prompt  
- Send each prompt to the AI model  
- Print the prompt and corresponding response  



## Script

Day_4_BuildTestCases.py`

```python
from anthropic import Anthropic
import os
from dotenv import load_dotenv

load_dotenv()

client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))

# QA-style test cases
prompts = [
    "What is API testing?",
    "What is regression testing?",
    "What is a test case?",
    "Difference between smoke and sanity testing"
]

for prompt in prompts:

    response = client.messages.create(
        model="claude-3-5-sonnet-20240620",  # Replace with valid model if needed
        max_tokens=300,

        messages=[
            {
                "role": "user",
                "content": prompt
            }
        ]
    )

    print("\nTEST:", prompt)
    print("ANSWER:", response.content[0].text)
```

## Sample Output
```
TEST: What is API testing?
ANSWER: API testing is the process of validating APIs...

TEST: What is regression testing?
ANSWER: Regression testing ensures that recent changes...
```
