# Feature rich prompt
XPrompt enables user to write feature riched prompt by introducing smart components directly.  

## Prompt compression
It is common that there are long context in the prompt besides instructions. For example, in typical retrieve and generate system, prompt includes a short instruction, such as question and answering guideline, while there is a large chunk of knowledge articles or context. Knowledge articles and context in the prompt can be compressed to 60% of its original length and retain the same generation as original.  

Therefore, with prompt compression, LLM cost could be saved by about 40% without sacrifising performance.  

```python
prompt = """
	Based on the following content
	
	content:
	<compress>
        article 1: ...

        article 2: ...
        ...
	</compress>
	
	Question: Can i get refund?
	Answer:
"""
```

Number of tokens of the compressed prompt will be instruction (such as, "based on the following content") + 60% of all articles tokens.  

## [Search](./search.md)
XPrompt support managed search index for LLM applicationl. User can leverage it by including the `<search>` tag in the prompt.   
Learn more about [search](./search.md).

for example:  
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




## [Data integration](./data_integration.md)
Different data sources could be directly used in the prompt by including corresponding data source tag, such as `<pdf>`.  
Learn more about [data integration in prompt](./data_integration.md).

List of data sources supported
- pdf
- google doc
- jira


For example:
```python
prompt = """Answer question based on the following request from customer

Request message: "can you tell me why i received this?"
Request attachment: 
"<pdf src='./attachment.pdf' page_range=0 />"

Question: summarize the issue from message and attachment
Answer:
"""

r = xprompt.ChatCompletion.create(model="gpt-3.5-turbo", messages=[{"role": "user", "content": prompt}])

```
