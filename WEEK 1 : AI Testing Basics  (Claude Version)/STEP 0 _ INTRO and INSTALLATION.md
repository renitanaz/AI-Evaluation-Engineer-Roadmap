# WHAT WE WILL BUILD (CLAUDE VERSION)
- > Claude API integration
- > AI test execution system
- > Test suite (prompts)
- > Output logging
- > Basic evaluation logic

# Why API integration
-  Real AI systems are API-based : They do not use webiste or chat screens. They use API calls from backend code.
  Sample flow: User → Your App → Claude API → Response → User
-  The evaluation system needs automation which we cannot realistically do manually in chat UI. We will be building datasets, test suites, evaluation pipelines, AI agents which requires sending hundreds of prompts automatically, storing results and scoring outputs.


# INSTALLATION :Setup Guide (Claude API)

## 1. Install Python

Download and install Python from:
https://www.python.org/downloads/

Verify installation:

```bash
py --version 
```


## 2. Install Required Libraries
```
pip install anthropic python-dotenv 
```

## 3 Get API Key
- Go to: https://console.anthropic.com/
- Create an account
- Generate API key
- Copy it

## 4 Create .env File

Create a file named .env in your project folder:
```
ANTHROPIC_API_KEY=your_key_here
```
