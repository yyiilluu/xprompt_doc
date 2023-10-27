# Index and search with XPrompt  

To use XPrompt to search various documents, mainly there are two steps. The first step is indexing, which adds documents to the vectorDB, and the second step is search, which simply retrieve the most relevant documents indexed using a search query.  

## Indexing
Indexing heavily depends on where data is coming from. XPrompt provides a few ways to index data into our managed vectorDB. 

### Index from plain text
If the document is in plain text, regardless of length. User could use XPrompt to index those text documents using python client and XPrompt will handel long passage segemantation by chunking it into proper length.  
Example:
...


### Index from google doc

### Other data sources

## Search
Instead of preparing data for search and connect with prompt, XPrompt allows user to add search query directly into prompt by adding search tag like `<search query='user search query' n_results=2 />`.  

There are two attributes that can be specified in the `search` tag,
1. (required) query: this is searh query to search against document indexed. The best search query usually have similar meaning with the desired documents. They don't have to have exact word overlaps as long as they have similar meaning
2. (optional) n_results: default 2. Number of document to retrieve. If there are more than one document retrieved, XPrompt will format all documents by prefixing each document with ID, for example `Doc 1: ... \n Doc 2: ...`. Retrieved documents are sorted based on similarity against query

### Search Example
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

### Search from local index
When long context might be required for a particular generation, it is usually better to search for the relevant part of the long context before generation rather than feeding the entire long context to the prompt. For that purpose, XPrompt allows user to add a long local context and search just on this context for generation.

```python
import xprompt

prompt = """
	Based on the following content
	
	content:
	<index collection='local_index'>
		long context to be search
	</index>
	<search query='request refund docs' n_results=2 collection='local_index' />
	
	Question: Can i get refund?
	Answer:
"""

```

This search query will only search passages from the indexed long document encapsulated by `<index></index>` with `collection='local_index'`. Without specifiying the collection, the search query will searchs against all the indexed documents.
