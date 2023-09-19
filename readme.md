# XPrompt

### XPrompt is a feature-rich prompt enables user can write in text to connect with data sources, search against VectorDB and achieve more

## What does XPrompt offer

Building LLM application requires connecting with different data sources or modalities, search against domain specific data using vector db, and combating with hallucination. Despite it is easy to write a prompt for OpenAI, preparing the right content for prompt and building instruments around LLM API calls remains to be challenging engineering problem.

As such, XPrompt is a managed API layer simplify LLM application development though
**Prepare data for prompt**
- ingest different types of data**, such as pdf, audio file
- Search against domain specific index and more directly into prompt seamlessly.

**Prepare request for best use of LLM**
- Chunking long context in prompt into different requests and merge later
- Data redaction on client
- prompt compression
 
**Request post processing**
- Check hallucination
- Answer caching

And more!

XPrompt is fully compatible with OpenAI request and response payload, so that same request to OpenAI can now be sent to xprompt.

## How to use XPrompt

XPrompt introduces HTML like components into prompts, so that user could perform more complicates tasks before calling OpenAI

### Question answering with PDF

For example, to ask a questions for a PDF file, user could write a prompt like below in text.

```python
import xprompt

prompt = """
	Base on this paper below
	
	Paper:
	<pdf src='https://arxiv.org/pdf/2302.13971.pdf', page_range=0 />
	
	Question: what is the different between llama and other models?
	Answer:
"""

xprompt.OpenAIChatCompletion.create(
    model="gpt-3.5-turbo", 
    messages=[{"role": "user", "content": prompt}],
)
```

XPrompt will return the response just like OpenAI seamlessly. 

### Build a voice conversational agent

User could also easily build a voice conversational agent with XPrompt

```python
import xprompt

prompt = """
	You are a helpful conversational agent for user.
	
	User: <audio src='user audio file path' />
	Agent:
"""
xprompt.OpenAIChatCompletion.create(
    model="gpt-3.5-turbo", 
    messages=[{"role": "user", "content": prompt}],
    output_format="audio"
)
```

You could specify the response to be in audio format in XPrompt as well, so that instantly you have voice conversational agent built!

## Search against domain-specific data

LLM Applications often require searching against domain-specific data and then generate the response. Building such retrieve and generate (RAG) system is now 

```python
import xprompt

prompt = """
	Based on the following content
	
	content:
	<search query='request refund docs' n_results=2 />
	
	Question: Can i get refund?
	Answer:
"""

xprompt.OpenAIChatCompletion.create(
    model="gpt-3.5-turbo", 
    messages=[{"role": "user", "content": prompt}],
)
```

Using the search tag, LLM now will answer question based on retrieved content. XPrompt will help you to fully manage the search index.

## Checking for hallucination

One of the biggest challenge for production usage of LLM is the chance of model hallucination, which means model generate content that is purely counter-factual. XPrompt incorporate best practices for validating if the response is hallucinated and warns users for potential hallucinated tokens. If enabled, XPrompt will response everything a normal request has with additional information such as 

```python
{
	# ...,

  "answer_quality": {
    "controversy_tokens": [
      "maybe_hallucinated_token"
    ],
    "pass_corroboration": true
  }
}
```