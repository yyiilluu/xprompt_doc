# API features for LLM request
XPrompt is a managed API service that provides production API features.


## Switching among different LLM providers
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


## Rate limiting
Rating limiting is a feature that limits the number of requests that can be sent to a LLM provider for a minute.
Set the `rate_limit` for `xprompt` at the module level for the number of requests per minute.


```python
import xprompt
xprompt.rate_limit = 1000
```



## Answer caching
It is often wasteful to send the same request or similar request to a LLM provider over and over again. XPrompt provides a simple way to cache the exact same prompt, semantic similar prompt or by custom cache key.


When there are more than one caching enabled, the order of precedence would be in the following order:
1. custome cache key
2. exact prompt caching
3. semantic prompt caching


### Exact prompt caching
Once enabled, the next time the same prompt sent to the same model provider will be retrieved from cache rather than sending the request again.


```python
import xprompt
xpromp.exact_caching = True
```


### Semantic prompt caching
If the prompts contain similar semantic information, using the cached answer could be desired in some cases. Note that semantic similar prompts might not mean exactly the same thing, and don't enable semantic caching if requires the LLM to capture nuanced differences among each prompt.


```python
import xprompt
xpromp.semantic_caching = True
```





