 # XPrompt


### XPrompt is a feature-rich prompt simplifies the process of building LLM application


## What does XPrompt offer


Building a LLM application requires connecting with different data sources or modalities, searching against domain specific data using vector db, and combating with hallucination. Although it is easy to write a prompt for OpenAI, preparing the right content for prompt and building instruments around LLM API calls remains to be a challenging engineering problem.


As such, XPrompt is a managed API layer simplify LLM application development though


**Feature rich prompt**
- ingest different types of data, such as pdf and audio file
- Search against domain specific indexes and more directly into prompt seamlessly.
- Compress prompt to be more cost efficient


**Post generation features**
- Defense against hallucination
- Convert output into audio
**Production API support**
- API rate limiting
- Answer caching

And more!


XPrompt is 100% compatible with OpenAI request and response payload, so that same request to OpenAI can now be sent to xprompt and get identical responses back. Example notebook [here](./example_notebooks/demo.ipynb).

## Installation
Install the python client with 
```sh
pip install xprompt-client
```

## [Feature rich prompt](./features/feature_rich_prompt.md)


XPrompt introduces HTML like components into prompts, so that users could perform more complicated tasks before calling OpenAI. Learn more about other [feature rich components](./features/feature_rich_prompt.md).


### Question answering with PDF
For example, to ask a question for a PDF file, users could write a prompt like below in text.

```python
import xprompt


prompt = """
Base on this paper below
Paper:
<pdf src='https://arxiv.org/pdf/2302.13971.pdf', page_range=0 />
Question: what is the difference between llama and other models?
Answer:
"""

xprompt.OpenAIChatCompletion.create(
model="gpt-3.5-turbo",
messages=[{"role": "user", "content": prompt}],
)
```


XPrompt will return the response just like OpenAI seamlessly.
Learn more about [ingesting different data types](./features/data_ingestion.md).




To add an audio file as input, users could write `<audio src='user audio file path' />` to the prompt. And to get audio back, specify the response to be in audio format in XPrompt `create` function, like `output_format="audio"`.


This way you just built a voice conversational agent!
### Search against domain-specific data
LLM Applications often require searching against domain-specific data and then generate the response. Building such retrieve and generate (RAG) system is now

```python
import xprompt


prompt = """
Based on the following content
content:
<search query='request refund docs' n_results=2 />
Question: Can I get a refund?
Answer:
"""

xprompt.OpenAIChatCompletion.create(
model="gpt-3.5-turbo",
messages=[{"role": "user", "content": prompt}],
)
```


Using the search tag, LLM now will answer question based on retrieved content. XPrompt will help you to fully manage the search index.


Read more about [search](./features/search.md)




## [Post generation features](./features/post_generation.md)
XPrompt provides convenient features
### Confidence estimation
Hallucinated responses usually have low confidence scores, although not all low confidence cases are caused by hallucination. Usually when the question is ambiguous or confusing, the model tends to give a low confidence score and hallucinate an answer to give the best shot. So filtering out low confidence generation is a common tactic ML engineers use to identify potential problematic responses.




```python
xprompt.ChatCompletion.create(
model="gpt-3.5-turbo",
messages=[{"role": "user", "content": "Hello world"}],
include_confidence=True
)
```


### Build a voice conversational agent


User could also easily build a voice conversational agent with XPrompt


```python
import xprompt


prompt = """
You are a helpful conversational agent for users.
User: <audio src='user audio file path' />
Agent:
"""
xprompt.OpenAIChatCompletion.create(
model="gpt-3.5-turbo",
messages=[{"role": "user", "content": prompt}],
output_format="audio"
)
```


## [Production API features](./features/post_generation.md)
XPrompt has support for different API features that would be useful in the production environment. Check out all of [production API features](./features/api_features.md).


### [WIP]Switching among different LLM providers
Different LLM providers require different API clients and request formats. XPrompt allows users to switch among different LLM providers while following the exact same OpenAI request format.


```python
# send request to OpenAI
xprompt.ChatCompletion.create(
model="gpt-3.5-turbo",
messages=[{"role": "user", "content": "Hello world"}]
)

# swtich to claude_v2
xprompt.ChatCompletion.create(
model="claude_v2", # claude_v2, bard
messages=[{"role": "user", "content": "Hello world"}]
)
```




## [XPrompt design principal](./features/xprompt_design_principal.md)
To make it easy to follow, XPrompt has a consistent design language which is similar to HTML to lower the learning curve and be consistent.
There are mainly two places users interacts with XPrompt, which are **prompt level** and **API level**.
In the prompt level, XPrompt allows HTML like smart components to add new content or decorate text in there.
At the API level, users could collect more signals like confidence score or convert output format into audio.


Learn more about our [design principal](./features/xprompt_design_principal.md)

