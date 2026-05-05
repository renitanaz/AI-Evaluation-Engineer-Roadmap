# Pydantic Schema Validation

In WEEK 1, we have covered:
-  Call Claude API
-  Test AI variability
-  Log outputs to JSON
-  Build test suites
-  Evaluate with basic rules

## Why is schema Validation important:
- Evaluation rules may fail because the text isn't what you expected
- You can't guarantee the AI output has the fields you need

## Part 1: Install Pydantic
```bash
pip install pydantic
```

## Part 2: Pydantic Basics

 Pydantic
   -  Enforces types
   -  Provides defaults
   -  Auto-converts compatible types (int → float)
   -  Validates data before it's saved
   -  Produces clean JSON

### Day_6_PydanticBasics_Simple.py
``` python
from pydantic import BaseModel, Field

# Define the SHAPE of data you expect
class TestResult(BaseModel): # Inherit from BaseModel
    prompt: str
    response: str
    model_name: str
    temperature: float

# Create an instance
result = TestResult(  # This will validate the data type as you create it. 
    prompt="What is API testing?",                                      # If you try to pass an integer here, Pydantic will raise a validation error because it expects a string.   
    response="API testing is...",                                       # if you pass a non-string value, Pydantic will raise an error.
    model_name="claude-haiku-4-5-20251001",                             # This should be a string, so if you pass an integer like 123, Pydantic will raise a validation error.  
    temperature=0.7                                                     # This should be a float, so if you pass a string like "0.7", Pydantic will try to convert it to a float. If it can't, it will raise a validation error.    
)

# model_dump() is the new method in Pydantic v2 to get a dictionary representation of the model, ready to be saved as JSON or used in your application.
# In Pydantic v1, you would use .dict() instead.
print(result.model_dump())
```

### Output

```python
{
    'prompt': 'What is API testing?',
    'response': 'API testing is...',
    'model_name': 'claude-haiku-4-5-20251001',
    'temperature': 0.7
}
```

## Part 3: Update Schema for Week 1 Projects

## Production-Ready Schema to use
```python
from pydantic import BaseModel, Field, computed_field # New in Pydantic v2, used for defining computed properties that are automatically included in the model's output.    
from typing import Optional # Used for defining optional fields in the model, which can be None if not provided.
from datetime import datetime 

class AITestResponse(BaseModel):
    """Schema for AI test execution with full metadata"""
    test_id: int
    prompt: str
    response: str
    model: str = Field(default="claude-haiku-4-5-20251001") # Default model, can be overridden
    temperature: float = Field(default=0.7, ge=0.0, le=1.0) # ge=0.0 means >= 0, le=1.0 means <= 1, ensuring the temperature is within the valid range for LLMs.# Must be 0-1 
    max_tokens: int = Field(default=300, gt=0)  # Must be > 0
    
    # Evaluation
    eval_status: str = Field(default="PASS")  # PASS or FAIL
    eval_reason: Optional[str] = None
    
    # Metadata
    timestamp: datetime = Field(default_factory=datetime.utcnow) # Automatically set to current time when the model is created
    run_id: str  # for grouping multiple runs
    
    @computed_field # New in Pydantic v2, used for defining computed properties that are automatically included in the model's output.
    @property # This allows you to access the computed property like a regular attribute, without needing to call it as a method.
    def response_length(self) -> int:   # This is a computed property that calculates the length of the response string. It will be automatically included in the model's output when you call model_dump() or dict().  
        """Auto-calculate response length"""
        return len(self.response)

    @computed_field
    @property
    def is_valid_length(self) -> bool:
        """Auto-validate response isn't too short"""
        return self.response_length >= 30
```

## Day 3 upgraded to pydantic
``` python
"""
Day 3 UPGRADED: Store Outputs with Pydantic Schema Validation

What's new:
- Every output is validated against a Pydantic schema
- Results are guaranteed to have consistent structure
- Easy to catch bad data before saving to JSON
"""

import json
from anthropic import Anthropic
from pydantic import BaseModel
import os
from dotenv import load_dotenv

load_dotenv()
client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY"))

# ============================================
# STEP 1: Define your data schema (Pydantic)
# ============================================
class AITestResponse(BaseModel):
    """Schema for a single test case response"""
    test_id: int
    prompt: str
    response: str
    model: str
    temperature: float = 0.7
    max_tokens: int = 300
    response_length: int  # Auto-calculated

# ============================================
# STEP 2: Collect test results
# ============================================
results = []
prompt = "What is regression testing?"

print("Running 5 tests with Pydantic validation...\n")

for i in range(5):
    # Call Claude
    response = client.messages.create(
        model="claude-haiku-4-5-20251001",
        max_tokens=300,
        temperature=0.7,
        messages=[
            {
                "role": "user",
                "content": prompt
            }
        ]
    )
    
    answer = response.content[0].text
    
    # ✨ NEW: Validate with Pydantic before saving
    test_result = AITestResponse(
        test_id=i + 1,
        prompt=prompt,
        response=answer,
        model="claude-haiku-4-5-20251001",
        temperature=0.7,
        max_tokens=300,
        response_length=len(answer)
    )
    
    # Store the validated dict (not raw text)
    results.append(test_result.model_dump())
    
    print(f"✅ Test {i+1}: {len(answer)} chars | Status: VALID")

# ============================================
# STEP 3: Save to JSON
# ============================================
with open("results.json", "w") as f:
    json.dump(results, f, indent=2)

# ============================================
# STEP 4: Verify structure
# ============================================
print(f"\n{'='*50}")
print(f"Saved {len(results)} validated results to results.json")
print(f"{'='*50}\n")

print("Structure of saved data:")
print(json.dumps(results[0], indent=2))

# ============================================
# STEP 5: (Bonus) Analyze the results
# ============================================
avg_length = sum(r["response_length"] for r in results) / len(results)
print(f"\nAnalysis:")
print(f"  Total tests: {len(results)}")
print(f"  Avg response length: {avg_length:.0f} chars")
print(f"  Min length: {min(r['response_length'] for r in results)} chars")
print(f"  Max length: {max(r['response_length'] for r in results)} chars")
```

## Data Validation Layer (Pydantic)

| Concept            | What It Does                         | Why It Matters                          |
|------------------|-------------------------------------|----------------------------------------|
| Pydantic Model   | Defines the shape of data           | Enforces consistency                   |
| Validation       | Checks data before saving           | Catches errors early                   |
| `model_dump()`   | Converts to JSON-safe dictionary    | Easy to serialize                      |
| Computed Fields  | Auto-calculates values on the fly   | Single source of truth                 |
| Field Validation | Applies rules (e.g., `ge=0`, `gt=0`) | Ensures type-safe constraints          |
