# Post generation features
After getting the generation from LLM, XPrompt offers features to make response more usable, such as confidence estimation and output conversion.


## Defense against hallucination
Model hallucination is a common problem when LLM generates a response that is not grounded by factual evidence or even context.
To deal with hallucination, XPrompt provides a couple options.


### Confidence estimation
Hallucinated responses usually have low confidence scores, although not all low confidence cases are caused by hallucination. Usually when the question is ambiguous or confusing, the model tends to give a low confidence score and hallucinate an answer to give the best shot. So filtering out low confidence generation is a common tactic ML engineers use to identify potential problematic responses.


```python
xprompt.ChatCompletion.create(
model="gpt-3.5-turbo",
messages=[{"role": "user", "content": "Hello world"}],
include_confidence=True
)
```


The downside of getting a calibrated confidence score is it will incur more cost to the LLM provider and potentially slow down the request by 2x. In most cases, latency is not a concern since each request is really fast. But it is worth noting for very latency sensitive use cases.






### Answer corroboration
If a model would hallucinate, it is unlikely that the model would hallucinate the same thing every time. Therefore, XPrompt provides the ability to corroborate different responses from the same model and different models to identify potentially hallucinated or problematic sentences or words.


```python
xprompt.ChatCompletion.create(
model="gpt-3.5-turbo",
messages=[{"role": "user", "content": "Hello world"}],
answer_quality={'answer_corroboration_count': 2}
)


# response
"answer_quality": {
"pass_corroboration": true,
"controversial_tokens": []
},
```


## Convert output format
XPrompt allows outputs to be converted into different formats, including audio.


### Output audio
XPrompt has a built-in feature to ingest audio files and text to speech to output an audio as well.
Enable this feature by passing `output_format="audio"`. Response will include `byte_content` for each message.




```python
xprompt.ChatCompletion.create(
model="gpt-3.5-turbo",
messages=[{"role": "user", "content": prompt}],
output_format="audio"
)


# response
with open('./response.mp3', 'wb') as f:
f.write(r["choices"][0]["message"]["byte_content"])


```
