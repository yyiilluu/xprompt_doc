# Integrating different data sources into XPrompt
XPrompt allows users to directly add different data source into the prompt.

## PDF
Ingesting PDF into the prompt can be done with the following
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
Parameters for the `pdf` tag are: 
- [required] src: local file path or path can be accessed directly from the internet, such as arxiv
- [optional] page_rage: default is all pages. user can specific a single page with a number like `page_range=0` for the first page, or a range of pages like `page_range=1-5` for the 2nd page to the 6th page


## Google Doc
User can add google doc directly to the prompt. User needs to get the credentials by following google's documentation (#TODO: add links) 
```python
prompt = """Based on the following
<gdoc doc_id='1isUZmxTNu0dU0AGBajv7L2qdRIMt4pxBiFiFNQj_0_8' />

Question: What is XPrompt?
Answer:
"""

r = xprompt.ChatCompletion.create(
    model="gpt-3.5-turbo", 
    messages=[{"role": "user", "content": prompt}], 
    temperature=0)
```

Parameters for `gdoc` tag are:
- [required] doc_id: the google doc id, usually can be found from the url link of a google doc. For example if the url is `https://docs.google.com/document/d/111111/edit#heading=h.xxx`, the doc id would be `11111`
