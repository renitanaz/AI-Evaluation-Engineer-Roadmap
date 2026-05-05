# Pydantic Quick Reference for AI Evaluation Engineers

## Installation
```bash
pip install pydantic
```

## 1. Basic Model Definition

```python
from pydantic import BaseModel, Field
from typing import Optional

class MyModel(BaseModel):
    field1: str                           # Required string
    field2: int                           # Required int
    field3: float = 0.5                   # Float with default
    field4: Optional[str] = None          # Optional (can be None)
    field5: str = Field(default="test")   # Alternative default syntax
```

## 2. Field Validation Rules

```python
from pydantic import BaseModel, Field

class ValidatedModel(BaseModel):
    # Min/Max for numbers
    temperature: float = Field(ge=0.0, le=1.0)  # 0 <= temp <= 1
    max_tokens: int = Field(gt=0)               # max_tokens > 0
    
    # Min/Max length for strings
    name: str = Field(min_length=1, max_length=100)
    
    # Regex pattern
    email: str = Field(pattern=r"^[\w\.-]+@[\w\.-]+\.\w+$")
    
    # Description (shows in schema)
    status: str = Field(default="PASS", description="PASS or FAIL")
```

## 3. AI Response Schema (Template)

```python
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime

class AITestResponse(BaseModel):
    # Input/Output
    test_id: int
    prompt: str
    response: str
    
    # Model Config
    model: str = Field(default="claude-haiku-4-5-20251001")
    temperature: float = Field(default=0.7, ge=0.0, le=1.0)
    max_tokens: int = Field(default=300, gt=0)
    
    # Evaluation
    eval_status: str = Field(default="PASS")
    eval_reason: Optional[str] = None
    
    # Metadata
    response_length: int
    timestamp: str = Field(default_factory=lambda: datetime.utcnow().isoformat())
    run_id: str = "default"
```

## 4. Creating Instances

```python
# Method 1: Positional args
result = AITestResponse(
    test_id=1,
    prompt="What is testing?",
    response="Testing is...",
    response_length=150
)

# Method 2: Keyword args (safer)
result = AITestResponse(
    test_id=1,
    prompt="What is testing?",
    response="Testing is...",
    model="claude-haiku-4-5-20251001",
    temperature=0.7,
    max_tokens=300,
    eval_status="PASS",
    eval_reason=None,
    response_length=150
)

# Method 3: From dict (common with API responses)
data_dict = {
    "test_id": 1,
    "prompt": "...",
    "response": "...",
    "response_length": 150
}
result = AITestResponse(**data_dict)
```

## 5. Converting to JSON/Dict

```python
# To Python dict
result_dict = result.model_dump()
# or with custom settings
result_dict = result.model_dump(exclude={"timestamp"})

# To JSON string
json_string = result.model_dump_json(indent=2)

# Save to file
import json
with open("results.json", "w") as f:
    json.dump([r.model_dump() for r in results], f, indent=2)
```

## 6. Validation & Error Handling

```python
from pydantic import ValidationError, BaseModel

class TestResult(BaseModel):
    test_id: int
    status: str

# Validation succeeds
try:
    result = TestResult(test_id=1, status="PASS")
    print(" Valid:", result.model_dump())
except ValidationError as e:
    print(" Error:", e)

# Validation fails (missing field)
try:
    result = TestResult(test_id=1)  # Missing 'status'
except ValidationError as e:
    print(" Missing field:", e)

# Validation fails (wrong type)
try:
    result = TestResult(test_id="not_int", status="PASS")  # ID should be int
except ValidationError as e:
    print(" Wrong type:", e)
    # Pydantic will try to convert (str "1" → int 1)
    # But will fail if conversion impossible
```

## 7. Computed Fields (Auto-Calculate)

```python
from pydantic import BaseModel, computed_field

class TestResult(BaseModel):
    prompt: str
    response: str
    
    @computed_field
    @property
    def response_length(self) -> int:
        """Auto-calculate response length"""
        return len(self.response)
    
    @computed_field
    @property
    def is_valid(self) -> bool:
        """Auto-validate response isn't too short"""
        return len(self.response) >= 30

# Usage
result = TestResult(
    prompt="test",
    response="This is a test response"
)
print(result.response_length)  # 22 (auto-calculated)
print(result.is_valid)         # True (auto-calculated)
```

## 8. Nested Models (Advanced)

```python
from pydantic import BaseModel
from typing import List

class EvalMetrics(BaseModel):
    status: str
    reason: str

class TestResult(BaseModel):
    test_id: int
    prompt: str
    response: str
    metrics: EvalMetrics  # Nested model

# Usage
result = TestResult(
    test_id=1,
    prompt="...",
    response="...",
    metrics=EvalMetrics(status="PASS", reason=None)
)
```

## 9. Model Configuration

```python
from pydantic import BaseModel, ConfigDict

class MyModel(BaseModel):
    model_config = ConfigDict(
        validate_assignment=True,      # Validate on assignment
        use_enum_values=True,          # Use enum values, not names
        json_encoders={...},           # Custom JSON encoders
    )
```

## 10. Common Patterns for AI Testing

### Pattern 1: Simple Response Logging
```python
class SimpleLog(BaseModel):
    test_id: int
    prompt: str
    response: str
    response_length: int
```

### Pattern 2: With Evaluation
```python
class EvaluatedResponse(BaseModel):
    test_id: int
    prompt: str
    response: str
    eval_status: str = "PASS"
    eval_reason: Optional[str] = None
    response_length: int
```

### Pattern 3: Multi-Model Comparison
```python
class MultiModelTest(BaseModel):
    test_id: int
    prompt: str
    models: dict  # {"gpt4": "...", "claude": "..."}
    best_model: str
    scores: dict  # {"gpt4": 0.8, "claude": 0.9}
```

### Pattern 4: RAG Evaluation
```python
class RAGTest(BaseModel):
    test_id: int
    query: str
    retrieved_docs: List[str]
    generated_response: str
    retrieval_score: float  # 0-1
    generation_score: float  # 0-1
    overall_score: float    # average
```

---

## Key Takeaways

| Concept | Use Case |
|---------|----------|
| **BaseModel** | Define data structure |
| **Field()** | Add validation rules & descriptions |
| **Optional[T]** | Allow None values |
| **model_dump()** | Convert to JSON-safe dict |
| **@computed_field** | Auto-calculate fields |
| **ValidationError** | Catch validation issues |
| **Field validators** | Custom validation logic |


