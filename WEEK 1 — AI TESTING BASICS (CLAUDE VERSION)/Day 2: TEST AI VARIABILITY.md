# [Day 2] TEST AI VARIABILITY.md

## Goal
We will observe that AI is probabilistic.



## What are we going to perform
We would ask teh same question multiple times and observe that response changes everytime. 




## TEST AI VARIABILITY -Day_2_AIVariability.py
```python
from anthropic import Anthropic
import os
from dotenv import load_dotenv

load_dotenv()

client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))

# Run same prompt multiple times
for i in range(5):

    response = client.messages.create(
        model="claude-3-5-sonnet-20240620",
        max_tokens=300,
        temperature=0.7,

        messages=[
            {
                "role": "user",
                "content": "Explain API testing"
            }
        ]
    )

    print(f"\nRun {i+1}")
    print(response.content[0].text)
```




## What do we observe
We have recieved response 5 times but there is a variation every time.
