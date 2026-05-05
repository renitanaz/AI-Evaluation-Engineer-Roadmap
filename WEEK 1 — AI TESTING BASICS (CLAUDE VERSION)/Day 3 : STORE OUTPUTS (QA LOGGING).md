# [ Day 3 ] STORE OUTPUTS (QA LOGGING)

## Goal
Capture AI responses and store them in a structured format instead of printing them to the console.

## What are we going to perform
- Reuse the same prompt from Day 2  
- Call the AI model multiple times  
- Collect responses  
- Save them into a `.json` file  


## Why This Matters (QA Perspective)

In traditional QA:
- Test results are logged  
- Execution data is stored  
- Results are used for analysis  

In AI systems:
- Outputs must be recorded  
- Responses are evaluated later  
- Logging enables debugging and comparison

  
## Script
STORE OUTPUTS (QA LOGGING) - Day_3_Storeoutput_QALogging.py
``` 
import json
from anthropic import Anthropic
import os
from dotenv import load_dotenv

load_dotenv()

client = Anthropic(api_key=os.getenv("ANTHROPIC_API_KEY")) # Replace with your actual API key

results = []

for i in range(5):

    response = client.messages.create(
        model="claude-haiku-4-5-20251001", # Replace with the desired model
        max_tokens=300, 

        messages=[
            {
                "role": "user",
                "content": "What is regression testing?"
            }
        ]
    )

    answer = response.content[0].text # Extract the answer from the response

    results.append(answer) # Store the answer in the results list

# Save results to a JSON file like QA logs
with open("results.json", "w") as f:  
    json.dump(results, f, indent=2) # with indentation for readability
```

## What to observe
We observe a JSON file is created with responces from claude.

##  Output Example
```
[
  "# Regression Testing\n\nRegression testing is the process of **re-testing software to ensure that recent code changes haven't broken existing functionality**.\n\n## Key Characteristics\n\n- **Focuses on existing features** that were previously working\n- **Runs after modifications** like bug fixes, patches, or new features\n- **Verifies unintended side effects** aren't introduced\n- Usually performed on already-tested functionality\n\n## When It's Used\n\n- After bug fixes\n- Following new feature implementation\n- During version updates or patches\n- After code refactoring\n- Before production releases\n\n## Common Approaches\n\n| Approach | Description |\n|----------|-------------|\n| **Selective Testing** | Re-test only affected areas |\n| **Full Testing** | Re-test the entire application |\n| **Automated Testing** | Use scripts/tools to repeat tests |\n| **Smoke Testing** | Quick sanity checks of core features |\n\n## Example\n\nYou fix a login bug. Regression testing verifies that:\n- The login bug is fixed \u2713\n- Password reset still works \u2713\n- User registration isn't affected \u2713\n- Session management functions normally \u2713\n\n## Benefits\n\n\u2713 Catches unintended side effects early  \n\u2713 Ensures stability across releases  \n\u2713 Reduces production bugs  \n\u2713 Improves",
  "# Regression Testing\n\nRegression testing is a type of software testing that verifies that **previously working features still function correctly after code changes** (like bug fixes, updates, or new features).\n\n## Key Purpose\nIt ensures that recent modifications haven't inadvertently broken existing functionality.\n\n## When It's Used\n- After bug fixes\n- After adding new features\n- After code refactoring\n- After system updates\n- Before releasing new versions\n\n## Simple Example\nIf you fix a login bug, regression testing would confirm that:\n- The login bug is fixed \u2713\n- The password reset feature still works \u2713\n- User account creation still works \u2713\n- Other features weren't accidentally broken \u2713\n\n## Common Approaches\n\n| Approach | Description |\n|----------|-------------|\n| **Full Regression** | Test all features (thorough but time-consuming) |\n| **Selective** | Test only areas affected by changes (faster) |\n| **Automated** | Use automated tests (efficient for repetitive testing) |\n\n## Benefits\n\u2713 Catches unintended side effects  \n\u2713 Maintains product quality  \n\u2713 Saves time with automation  \n\u2713 Increases confidence in releases\n\nRegression testing is essential in modern software development because changes are frequent, and you need to ensure stability while improving the product.",
  "# Regression Testing\n\nRegression testing is the process of **retesting software after changes to ensure that new code modifications haven't broken existing functionality**.\n\n## Key Characteristics\n\n| Aspect | Detail |\n|--------|--------|\n| **Purpose** | Verify that new changes don't negatively impact working features |\n| **When to do it** | After code updates, bug fixes, patches, or feature additions |\n| **Scope** | Both modified areas and unmodified areas that might be affected |\n| **Approach** | Rerun previously executed test cases |\n\n## Common Scenarios\n\n- **Bug fixes** - Ensuring the fix doesn't cause other problems\n- **Feature updates** - Verifying existing features still work\n- **System patches** - Testing after maintenance or updates\n- **Integration changes** - Checking impacts across components\n\n## Example\n\n```\nOriginal code works \u2192 Developer fixes a bug \u2192 \nRun regression tests \u2192 Confirm:\n  \u2713 The bug is fixed\n  \u2713 Other features still work\n  \u2713 No new bugs introduced\n```\n\n## Benefits\n\n\u2705 Catches unintended side effects early  \n\u2705 Builds confidence in software quality  \n\u2705 Reduces risk of new defects  \n\u2705 Ensures stability during development  \n\n## Common Approaches\n\n- **Manual testing** -",
  "# Regression Testing\n\nRegression testing is the process of **re-testing software to ensure that recent changes or updates haven't broken existing functionality**.\n\n## Key Purpose\nIt verifies that new code changes don't negatively impact features that were previously working correctly.\n\n## When It's Used\n- After bug fixes\n- Following feature additions\n- During code refactoring\n- Before software releases\n- After patches or updates\n\n## Common Approaches\n\n| Method | Description |\n|--------|-------------|\n| **Full Regression** | Test all functionality (thorough but time-consuming) |\n| **Partial Regression** | Test affected modules only (more efficient) |\n| **Smoke Testing** | Quick tests of critical functions |\n| **Automated Testing** | Use test scripts to repeat test cases quickly |\n\n## Simple Example\nIf you fix a login bug, regression testing would verify that:\n- \u2705 The login bug is fixed\n- \u2705 Password reset still works\n- \u2705 User profile pages load correctly\n- \u2705 Session management functions properly\n\n## Benefits\n- Catches unintended side effects early\n- Increases software reliability\n- Reduces bugs in production\n- Saves time with automation\n\n**In short:** It's quality assurance to make sure you didn't break anything while fixing or improving something else.",
  "# Regression Testing\n\nRegression testing is the practice of **re-running existing tests** to ensure that recent code changes haven't broken previously working functionality.\n\n## Key Concepts\n\n**Purpose:**\n- Verify that new features, bug fixes, or code modifications don't introduce new defects\n- Ensure previously passing tests still pass after changes\n\n**When it's done:**\n- After bug fixes\n- After adding new features\n- After code refactoring\n- After performance improvements\n- Before releases\n\n## Example\n\n```\nOriginal Code \u2192 All tests pass \u2713\n     \u2193\nDeveloper fixes a bug\n     \u2193\nRegression Testing \u2192 Run all previous tests again\n     \u2193\nResult: Confirm nothing broke (or find new issues)\n```\n\n## Types of Regression Testing\n\n1. **Unit Regression** - Test individual functions/methods\n2. **Functional Regression** - Test entire features\n3. **System Regression** - Test complete application\n4. **Full Regression** - Test entire test suite\n\n## Approaches\n\n| Approach | Details |\n|----------|---------|\n| **Re-test All** | Run entire test suite (thorough but time-consuming) |\n| **Selective** | Run only affected tests (faster but riskier) |\n| **Prioritized** | Run most critical tests first |\n| **"
]
```
