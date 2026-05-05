# UseCase:

A company has a chatbot that answers: refund requests,password resets, product questions, policy explanations.

AI might:
  - give wrong refund policy 
  - hallucinate pricing 
  - respond too vaguely 
  - break compliance rules 
So they need a fast filter system.

## Example scenario:
User asks: “Can I get a refund after 60 days?”
AI responds: “Yes, refunds are always allowed anytime.”  :(

Why is it an issue:
  - It is wrong policy
  - It creates financial/legal risk


## We need Quality checks.
These guaity checks are often called 
- LLM guardrails
- AI Response Validation Layer
- quality filters
- safety checks

**We define simple rules:**
Contains policy contradiction > 	FAIL
Too short / vague	            >   FAIL
Mentions unsafe claim	        >   FAIL
Otherwise	                    >   PASS


## Script
Implementing the rules:
``` python
def evaluate_ai_response(response):

    # Rule 1: too short = not useful
    if len(response) < 30:
        return "FAIL - too short"

    # Rule 2: dangerous absolute claims
    if "always" in response.lower():
        return "FAIL - unsafe generalization"

    # Rule 3: missing context or vague answer
    if "not sure" in response.lower():
        return "FAIL - uncertain response"

    # If nothing breaks rules
    return "PASS"
```



## Evolution of these quality checks at advanced levels:
- Level 1	    |          Rule-based pass/fail (WE ARE HERE)
- Level 2	    |          LLM-as-a-judge scoring. i.e. AI evaluates AI responses
- Level 3	    |          Multi-metric evaluation (quality, safety, relevance)
- Level 4	    |          Autonomous evaluation agents. i.e. If FAIL, Then AI regenerates better answer
