# SPrompt

### SPrompt is a feature-rich prompt that user can write in text to connect with data sources, search against VectorDB and achieve more

### What does SPrompt offer

Building LLM application requires connecting with different data sources or modalities, search against domain specific data using vector db, and combating with hallucination. Despite it is easy to write a prompt for OpenAI, preparing the right content for prompt and building instruments around LLM API calls remains to be challenging engineering problem.

As such, SPrompt is a managed API layer enables users to **ingest different types of data**, such as pdf, audio file, **search against domain specific index** and more directly into prompt seamlessly. Also it enables users to get the most out of LLM, such as **hallucination checking, TTS, and long content splitting**.

SPrompt is fully compatible with OpenAI request and response payload, so that same request to OpenAI can now be sent to SPrompt.

### How to use SPrompt

SPrompt introduces HTML like components into prompts, so that user could perform more complicates tasks before calling OpenAI

**Question answering with PDF**

For example, to ask a questions for a PDF file, user could write a prompt like below in text.

```html
import sprompt

prompt = """
	Base on this paper below
	
	Paper:
	<pdf src='https://arxiv.org/pdf/2302.13971.pdf', page_range=0 />
	
	Question: what is the different between llama and other models?
	Answer:
"""

sprompt.OpenAIChatCompletion.create(
    model="gpt-3.5-turbo", 
    messages=[{"role": "user", "content": prompt}],
    output_format="audio"
)
```

SPrompt will return the response just like OpenAI seamlessly. 

**Talk to a conversational agent**

User could also easily build a voice conversational agent with SPrompt

```html
import sprompt

prompt = """
	You are a helpful conversational agent for user.
	
	User: <audio src='user audio file path' />
	Agent:
"""
sprompt.OpenAIChatCompletion.create(
    model="gpt-3.5-turbo", 
    messages=[{"role": "user", "content": prompt}],
    output_format="audio"
)
```

You could specify the response to be in audio format in SPrompt as well, so that instantly you have voice conversational agent built!

**Search against domain-specific data**

LLM Applications often require searching against domain-specific data and then generate the response. Building such retrieve and generate (RAG) system is now 

```html
prompt = """
	Based on the following content
	
	content:
	<search query='request refund docs' n_results=2 />
	
	Question: Can i get refund?
	Answer:
"""
```

Using the search tag, LLM now will answer question based on retrieved content. SPrompt will help you to fully manage the search index.

**Checking for hallucination**

One of the biggest challenge for production usage of LLM is the chance of model hallucination, which means model generate content that is purely counter-factual. SPrompt incorporate best practices for validating if the response is hallucinated and warns users for potential hallucinated tokens. If enabled, SPrompt will response everything a normal request has with additional information such as 

```json
{
	...,

  "answer_quality": {
    "controversy_tokens": [
      "maybe_hallucinated_token"
    ],
    "pass_corroboration": true
  }
}
```