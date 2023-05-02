# ThinkGPT <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:91.95064629847238% 85.95769682726204%;background-size:5418.75% 5418.75%" data-codepoints="1f9e0"></span></span><span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:73.97179788484137% 83.96004700352526%;background-size:5418.75% 5418.75%" data-codepoints="1f916"></span></span>
ThinkGPT is a Python library aimed at implementing Chain of Thoughts for Large Language Models (LLMs), prompting the model to think, reason, and to create generative agents. 
The library aims to help with the following:
* solve limited context with long memory and compressed knowledge
* enhance LLMs' one-shot reasoning with higher order reasoning primitives
* add intelligent decisions to your code base


## Key Features <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:99.94124559341951% 2.056404230317274%;background-size:5418.75% 5418.75%" data-codepoints="2728"></span></span>
* Thinking building blocks 🧱:
    * Memory <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:91.95064629847238% 85.95769682726204%;background-size:5418.75% 5418.75%" data-codepoints="1f9e0"></span></span>: GPTs that can remember experience
    * Self-refinement <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:55.99294947121034% 14.042303172737954%;background-size:5418.75% 5418.75%" data-codepoints="1f527"></span></span>: Improve model-generated content by addressing critics
    * Compress knowledge <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:12.044653349001175% 28.025851938895418%;background-size:5418.75% 5418.75%" data-codepoints="1f310"></span></span>: Compress knowledge and fit it in LLM's context either by anstracring rules out of observations or summarize large content
    * Inference <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:50% 44.00705052878966%;background-size:5418.75% 5418.75%" data-codepoints="1f4a1"></span></span>️: Make educated guesses based on available information
    * Natural Language Conditions <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:51.99764982373678% 71.97414806110459%;background-size:5418.75% 5418.75%" data-codepoints="1f4dd"></span></span>: Easily express choices and conditions in natural language
* Efficient and Measurable GPT context length <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:51.99764982373678% 46.00470035252644%;background-size:5418.75% 5418.75%" data-codepoints="1f4d0"></span></span>
* Extremely easy setup and pythonic API <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:18.037602820211514% 36.01645123384254%;background-size:5418.75% 5418.75%" data-codepoints="1f3af"></span></span> thanks to [DocArray](https://github.com/docarray/docarray)

## Installation <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:51.99764982373678% 4.054054054054054%;background-size:5418.75% 5418.75%" data-codepoints="1f4bb"></span></span>
You can install ThinkGPT using pip:

```shell
pip install git+https://github.com/followDev/Python-Thinkgpt.git
```

## API Documentation <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:51.99764982373678% 65.98119858989423%;background-size:5418.75% 5418.75%" data-codepoints="1f4da"></span></span>
### Basic usage:
```python
from thinkgpt.llm import ThinkGPT
llm = ThinkGPT(model_name="gpt-3.5-turbo")
# Make the llm object learn new concepts
llm.memorize(['DocArray is a library for representing, sending and storing multi-modal data.'])
llm.predict('what is DocArray ?', remember=llm.remember('DocArray definition'))
```

### Memorizing and Remembering information
```python
llm.memorize([
    'DocArray allows you to send your data, in an ML-native way.',
    'This means there is native support for Protobuf and gRPC, on top of HTTP and serialization to JSON, JSONSchema, Base64, and Bytes.',
])

print(llm.remember('Sending data with DocArray', limit=1))
```
```text
['DocArray allows you to send your data, in an ML-native way.']
```

Use the `limit` parameter to specify the maximum number of documents to retrieve.
In case you want to fit documents into a certain context size, you can also use the `max_tokens` parameter to specify the maximum number of tokens to retrieve.
For instance:
```python
from examples.knowledge_base import knowledge
from thinkgpt.helper import get_n_tokens

llm.memorize(knowledge)
results = llm.remember('hello', max_tokens=1000, limit=1000)
print(get_n_tokens(''.join(results)))
```
```text
1000
```
However, keep in mind that concatenating documents with a separator will add more tokens to the final result.
The `remember` method does not account for those tokens.

### Predicting with context from long memory
```python
from examples.knowledge_base import knowledge
llm.memorize(knowledge)
llm.predict('Implement a DocArray schema with 2 fields: image and TorchTensor', remember=llm.remember('DocArray schemas and types'))
```

### Self-refinement

```python
print(llm.refine(
    content="""
import re
    print('hello world')
        """,
    critics=[
        'File "/Users/user/PyCharm2022.3/scratches/scratch_166.py", line 2',
        "  print('hello world')",
        'IndentationError: unexpected indent'
    ],
    instruction_hint="Fix the code snippet based on the error provided. Only provide the fixed code snippet between `` and nothing else."))

```

```text
import re
print('hello world')
```

One of the applications is self-healing code generation implemented by projects like [gptdeploy](https://github.com/jina-ai/gptdeploy) and [wolverine](https://github.com/biobootloader/wolverine)

### Compressing knowledge
In case you want your knowledge to fit into the LLM's context, you can use the following techniques to compress it:
#### Summarize content
Summarize content using the LLM itself.
We offer 2 methods
1. one-shot summarization using the LLM
```python
llm.summarize(
  large_content,
  max_tokens= 1000,
  instruction_hint= 'Pay attention to code snippets, links and scientific terms.'
)
```
Since this technique relies on summarizing using a single LLM call, you can only pass content that does not exceed the LLM's context length.

2. Chunked summarization
```python
llm.chunked_summarize(
  very_large_content,
  max_tokens= 4096,
  instruction_hint= 'Pay attention to code snippets, links and scientific terms.'
)
```
This technique relies on splitting the content into different chunks, summarizing each of those chunks and then combining them all together using an LLM.

#### Induce rules from observations
Amount to higher level and more general observations from current observations:
```python
llm.abstract(observations=[
    "in tunisian, I did not eat is \"ma khditech\"",
    "I did not work is \"ma khdemtech\"",
    "I did not go is \"ma mchitech\"",
])
```

```text
['Negation in Tunisian Arabic uses "ma" + verb + "tech" where "ma" means "not" and "tech" at the end indicates the negation in the past tense.']
```

This can help you end up with compressed knowledge that fits better the limited context length of LLMs.
For instance, instead of trying to fit code examples in the LLM's context, use this to prompt it to understand high level rules and fit them in the context.

### Natural language condition
Introduce intelligent conditions to your code and let the LLM make decisions
```python
llm.condition(f'Does this represent an error message ? "IndentationError: unexpected indent"')
```
```text
True
```
### Natural language select
Alternatively, let the LLM choose among a list of options:
```python
llm.select(
    question="Which animal is the king of the jungle?",
    options=["Lion", "Elephant", "Tiger", "Giraffe"]
)
```
```text
['Lion']
```

You can also prompt the LLM to choose an exact number of answers using `num_choices`. By default, it's set to `None` which means the LLM will select any number he thinks it's correct.
## Use Cases <span class="emoji-outer emoji-sizer"><span class="emoji-inner" style="background: url(chrome-extension://gaoflciahikhligngeccdecgfjngejlh/emoji-data/sheet_apple_32.png);background-position:67.97884841363103% 34.01880141010576%;background-size:5418.75% 5418.75%" data-codepoints="1f680"></span></span>
Find out below example demos you can do with `thinkgpt`
### Teaching ThinkGPT a new language
```python
from thinkgpt.llm import ThinkGPT

llm = ThinkGPT(model_name="gpt-3.5-turbo")

rules = llm.abstract(observations=[
    "in tunisian, I did not eat is \"ma khditech\"",
    "I did not work is \"ma khdemtech\"",
    "I did not go is \"ma mchitech\"",
], instruction_hint="output the rule in french")
llm.memorize(rules)

llm.memorize("in tunisian, I studied is \"9rit\"")

task = "translate to Tunisian: I didn't study"
llm.predict(task, remember=llm.remember(task))
```
```text
The translation of "I didn't study" to Tunisian language would be "ma 9ritech".
```

### Teaching ThinkGPT how to code with `thinkgpt` library
```python
from thinkgpt.llm import ThinkGPT
from examples.knowledge_base import knowledge

llm = ThinkGPT(model_name="gpt-3.5-turbo")

llm.memorize(knowledge)

task = 'Implement python code that uses thinkgpt to learn about docarray v2 code and then predict with remembered information about docarray v2. Only give the code between `` and nothing else'
print(llm.predict(task, remember=llm.remember(task, limit=10, sort_by_order=True)))
```

Code generated by the LLM:
```text
from thinkgpt.llm import ThinkGPT
from docarray import BaseDoc
from docarray.typing import TorchTensor, ImageUrl

llm = ThinkGPT(model_name="gpt-3.5-turbo")

# Memorize information
llm.memorize('DocArray V2 allows you to represent your data, in an ML-native way')


# Predict with the memory
memory = llm.remember('DocArray V2')
llm.predict('write python code about DocArray v2', remember=memory)
```
### Replay Agent memory and infer new observations
Refer to the following script for an example of an Agent that replays its memory and induces new observations.
This concept was introduced in [the Generative Agents: Interactive Simulacra of Human Behavior paper](https://arxiv.org/abs/2304.03442).

```shell
python -m examples.replay_expand_memory
```
```text
new thoughts:
Klaus Mueller is interested in multiple topics
Klaus Mueller may have a diverse range of interests and hobbies
```

### Replay Agent memory, criticize and refine the knowledge in memory
Refer to the following script for an example of an Agent that replays its memory, performs self-criticism and adjusts its memory knowledge based on the criticism.
```shell
python -m examples.replay_criticize_refine
```
```text
refined "the second number in Fibonacci sequence is 2" into "Observation: The second number in the Fibonacci sequence is actually 1, not 2, and the sequence starts with 0, 1."
...
```
This technique was mainly implemented in the [the Self-Refine: Iterative Refinement with Self-Feedback paper](https://arxiv.org/abs/2303.17651)


For more detailed usage and code examples check `./examples`.

