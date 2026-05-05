# Day 5 — Simple Rule-Based Evaluation (QA Logic)

## Goal
Introduce a basic evaluation layer that classifies AI-generated responses as **PASS** or **FAIL** using simple rules.



## Overview
In previous steps, we generated AI responses. In this step, we add a **basic evaluation function** to assess output quality. This is the first step toward building an **AI evaluation system**.



## Approach
- Define a simple evaluation function  
- Apply rule-based logic to assess response quality  
- Return a classification result (`PASS` or `FAIL`)  
- Test the function with a sample response  



## Script

**File:** `day5.py`

```python
# Simple rule-based evaluation (QA logic)

def evaluate(answer):

    # If response is too short → likely low quality
    if len(answer) < 30:
        return "FAIL"

    return "PASS"


# Example input
sample = "API testing ensures APIs work correctly."

print(evaluate(sample))
```
