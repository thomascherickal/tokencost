<p align="center">
  <img src="https://github.com/AgentOps-AI/tokencost/blob/c6c29613ec1131e72a27525de3aa5af8966fb6af/tokencost.png" height="300" alt="Tokencost" />
</p>

<p align="center">
  <em>Clientside token counting + price estimation for LLM apps and AI agents.</em>
</p>
<p align="center">
    <a href="https://pypi.org/project/tokencost/" target="_blank">
        <img alt="Python" src="https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54" />
        <img alt="Version" src="https://img.shields.io/pypi/v/tokencost?style=for-the-badge&color=3670A0">
    </a>
</p>
<p align="center">
<a href="https://twitter.com/agentopsai/">🐦 Twitter</a>
<span>&nbsp;&nbsp;•&nbsp;&nbsp;</span>
<a href="https://discord.gg/JHPt4C7r">📢 Discord</a>
<span>&nbsp;&nbsp;•&nbsp;&nbsp;</span>
<a href="https://agentops.ai/?tokencost">🖇️ AgentOps</a>
</p>


# TokenCost
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) ![PyPI - Version](https://img.shields.io/pypi/v/tokencost)
[![X (formerly Twitter) Follow](https://img.shields.io/twitter/follow/AgentOpsAI)](https://x.com/agentopsai)

Tokencost helps calculate the USD cost of using major Large Language Model (LLMs) APIs by calculating the estimated cost of prompts and completions.

Building AI agents? Check out [AgentOps](https://agentops.ai/?tokencost)


### Features
* **LLM Price Tracking** Major LLM providers frequently add new models and update pricing. This repo helps track the latest price changes
* **Token counting** Accurately count prompt tokens before sending OpenAI requests
* **Easy integration** Get the cost of a prompt or completion with a single function

### Example usage:

```python
from tokencost import calculate_prompt_cost, calculate_completion_cost

model = "gpt-3.5-turbo"
prompt = [{ "role": "user", "content": "Hello world"}]
completion = "How may I assist you today?"

prompt_cost = calculate_prompt_cost(prompt, model)
completion_cost = calculate_completion_cost(completion, model)

print(f"{prompt_cost} + {completion_cost} = {prompt_cost + completion_cost}")
# 0.0000135 + 0.000014 = 0.0000275
```

## Installation

#### Recommended: [PyPI](https://pypi.org/project/tokencost/):

```bash
pip install tokencost
```

## Usage

### Cost estimates
Calculating the cost of prompts and completions from OpenAI requests
```python
from openai import OpenAI

client = OpenAI()
model = "gpt-3.5-turbo"
prompt = [{ "role": "user", "content": "Say this is a test"}]

chat_completion = client.chat.completions.create(
    messages=prompt, model=model
)

completion = chat_completion.choices[0].message.content
# "This is a test."

prompt_cost = calculate_prompt_cost(prompt, model)
completion_cost = calculate_completion_cost(completion, model)
print(f"{prompt_cost} + {completion_cost} = {prompt_cost + completion_cost}")
# 0.0000180 + 0.000010 = 0.0000280
```

**Calculating cost using string prompts instead of messages:**
```python
from tokencost import calculate_prompt_cost

prompt_string = "Hello world" 
response = "How may I assist you today?"
model= "gpt-3.5-turbo"

prompt_cost = calculate_prompt_cost(prompt_string, model)
print(f"Cost: ${prompt_cost}")
# Cost: $3e-06
```

**Counting tokens**

```python
from tokencost import count_message_tokens, count_string_tokens

message_prompt = [{ "role": "user", "content": "Hello world"}]
# Counting tokens in prompts formatted as message lists
print(count_message_tokens(message_prompt, model="gpt-3.5-turbo"))
# 9

# Alternatively, counting tokens in string prompts
print(count_string_tokens(prompt="Hello world", model="gpt-3.5-turbo"))
# 2

```

## Cost table
Units denominated in USD. All prices can be located in `model_prices.json`.


* Prices last updated Jan 30, 2024 from: https://openai.com/pricing and https://github.com/BerriAI/litellm/blob/main/model_prices_and_context_window.json

|                                                                                             | max_tokens   | max_input_tokens   | input_cost_per_token   | output_cost_per_token   |
|:--------------------------------------------------------------------------------------------|:-------------|:-------------------|:-----------------------|:------------------------|
| gpt-4                                                                                       | 4,096        | 8,192              | $0.00003000            | $0.00006000             |
| gpt-4o                                                                                      | 4,096        | 128,000            | $0.00000500            | $0.00001500             |
| gpt-4o-2024-05-13                                                                           | 4,096        | 128,000            | $0.00000500            | $0.00001500             |
| gpt-4-turbo-preview                                                                         | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| gpt-4-0314                                                                                  | 4,096        | 8,192              | $0.00003000            | $0.00006000             |
| gpt-4-0613                                                                                  | 4,096        | 8,192              | $0.00003000            | $0.00006000             |
| gpt-4-32k                                                                                   | 4,096        | 32,768             | $0.00006000            | $0.00012000             |
| gpt-4-32k-0314                                                                              | 4,096        | 32,768             | $0.00006000            | $0.00012000             |
| gpt-4-32k-0613                                                                              | 4,096        | 32,768             | $0.00006000            | $0.00012000             |
| gpt-4-turbo                                                                                 | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| gpt-4-turbo-2024-04-09                                                                      | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| gpt-4-1106-preview                                                                          | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| gpt-4-0125-preview                                                                          | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| gpt-4-vision-preview                                                                        | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| gpt-4-1106-vision-preview                                                                   | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| gpt-3.5-turbo                                                                               | 4,097        | 16,385             | $0.00000150            | $0.00000200             |
| gpt-3.5-turbo-0301                                                                          | 4,097        | 4,097              | $0.00000150            | $0.00000200             |
| gpt-3.5-turbo-0613                                                                          | 4,097        | 4,097              | $0.00000150            | $0.00000200             |
| gpt-3.5-turbo-1106                                                                          | 16,385       | 16,385             | $0.00000100            | $0.00000200             |
| gpt-3.5-turbo-0125                                                                          | 16,385       | 16,385             | $0.00000050            | $0.00000150             |
| gpt-3.5-turbo-16k                                                                           | 16,385       | 16,385             | $0.00000300            | $0.00000400             |
| gpt-3.5-turbo-16k-0613                                                                      | 16,385       | 16,385             | $0.00000300            | $0.00000400             |
| ft:gpt-3.5-turbo                                                                            | 4,097        | 4,097              | $0.00000300            | $0.00000600             |
| text-embedding-3-large                                                                      | 8,191        | 8,191              | $0.00000013            | $0.00000000             |
| text-embedding-3-small                                                                      | 8,191        | 8,191              | $0.00000002            | $0.00000000             |
| text-embedding-ada-002                                                                      | 8,191        | 8,191              | $0.00000010            | $0.00000000             |
| text-embedding-ada-002-v2                                                                   | 8,191        | 8,191              | $0.00000010            | $0.00000000             |
| text-moderation-stable                                                                      | 32,768       | 32,768             | $0.00000000            | $0.00000000             |
| text-moderation-007                                                                         | 32,768       | 32,768             | $0.00000000            | $0.00000000             |
| text-moderation-latest                                                                      | 32,768       | 32,768             | $0.00000000            | $0.00000000             |
| 256-x-256/dall-e-2                                                                          | nan          | nan                | --                     | --                      |
| 512-x-512/dall-e-2                                                                          | nan          | nan                | --                     | --                      |
| 1024-x-1024/dall-e-2                                                                        | nan          | nan                | --                     | --                      |
| hd/1024-x-1792/dall-e-3                                                                     | nan          | nan                | --                     | --                      |
| hd/1792-x-1024/dall-e-3                                                                     | nan          | nan                | --                     | --                      |
| hd/1024-x-1024/dall-e-3                                                                     | nan          | nan                | --                     | --                      |
| standard/1024-x-1792/dall-e-3                                                               | nan          | nan                | --                     | --                      |
| standard/1792-x-1024/dall-e-3                                                               | nan          | nan                | --                     | --                      |
| standard/1024-x-1024/dall-e-3                                                               | nan          | nan                | --                     | --                      |
| whisper-1                                                                                   | nan          | nan                | --                     | --                      |
| azure/whisper-1                                                                             | nan          | nan                | --                     | --                      |
| azure/gpt-4-turbo-2024-04-09                                                                | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| azure/gpt-4-0125-preview                                                                    | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| azure/gpt-4-1106-preview                                                                    | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| azure/gpt-4-0613                                                                            | 4,096        | 8,192              | $0.00003000            | $0.00006000             |
| azure/gpt-4-32k-0613                                                                        | 4,096        | 32,768             | $0.00006000            | $0.00012000             |
| azure/gpt-4-32k                                                                             | 4,096        | 32,768             | $0.00006000            | $0.00012000             |
| azure/gpt-4                                                                                 | 4,096        | 8,192              | $0.00003000            | $0.00006000             |
| azure/gpt-4-turbo                                                                           | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| azure/gpt-4-turbo-vision-preview                                                            | 4,096        | 128,000            | $0.00001000            | $0.00003000             |
| azure/gpt-35-turbo-16k-0613                                                                 | 4,096        | 16,385             | $0.00000300            | $0.00000400             |
| azure/gpt-35-turbo-1106                                                                     | 4,096        | 16,384             | $0.00000150            | $0.00000200             |
| azure/gpt-35-turbo-0125                                                                     | 4,096        | 16,384             | $0.00000050            | $0.00000150             |
| azure/gpt-35-turbo-16k                                                                      | 4,096        | 16,385             | $0.00000300            | $0.00000400             |
| azure/gpt-35-turbo                                                                          | 4,096        | 4,097              | $0.00000150            | $0.00000200             |
| azure/gpt-3.5-turbo-instruct-0914                                                           | 4,097        | 4,097              | $0.00000150            | $0.00000200             |
| azure/gpt-35-turbo-instruct                                                                 | 4,097        | 4,097              | $0.00000150            | $0.00000200             |
| azure/mistral-large-latest                                                                  | 32,000       | 32,000             | $0.00000800            | $0.00002400             |
| azure/mistral-large-2402                                                                    | 32,000       | 32,000             | $0.00000800            | $0.00002400             |
| azure/command-r-plus                                                                        | 4,096        | 128,000            | $0.00000300            | $0.00001500             |
| azure/ada                                                                                   | 8,191        | 8,191              | $0.00000010            | $0.00000000             |
| azure/text-embedding-ada-002                                                                | 8,191        | 8,191              | $0.00000010            | $0.00000000             |
| azure/text-embedding-3-large                                                                | 8,191        | 8,191              | $0.00000013            | $0.00000000             |
| azure/text-embedding-3-small                                                                | 8,191        | 8,191              | $0.00000002            | $0.00000000             |
| azure/standard/1024-x-1024/dall-e-3                                                         | nan          | nan                | --                     | $0.00000000             |
| azure/hd/1024-x-1024/dall-e-3                                                               | nan          | nan                | --                     | $0.00000000             |
| azure/standard/1024-x-1792/dall-e-3                                                         | nan          | nan                | --                     | $0.00000000             |
| azure/standard/1792-x-1024/dall-e-3                                                         | nan          | nan                | --                     | $0.00000000             |
| azure/hd/1024-x-1792/dall-e-3                                                               | nan          | nan                | --                     | $0.00000000             |
| azure/hd/1792-x-1024/dall-e-3                                                               | nan          | nan                | --                     | $0.00000000             |
| azure/standard/1024-x-1024/dall-e-2                                                         | nan          | nan                | --                     | $0.00000000             |
| babbage-002                                                                                 | 16,384       | 16,384             | $0.00000040            | $0.00000040             |
| davinci-002                                                                                 | 16,384       | 16,384             | $0.00000200            | $0.00000200             |
| gpt-3.5-turbo-instruct                                                                      | 4,096        | 8,192              | $0.00000150            | $0.00000200             |
| gpt-3.5-turbo-instruct-0914                                                                 | 4,097        | 8,192              | $0.00000150            | $0.00000200             |
| claude-instant-1                                                                            | 8,191        | 100,000            | $0.00000163            | $0.00000551             |
| mistral/mistral-tiny                                                                        | 8,191        | 32,000             | $0.00000015            | $0.00000046             |
| mistral/mistral-small                                                                       | 8,191        | 32,000             | $0.00000200            | $0.00000600             |
| mistral/mistral-small-latest                                                                | 8,191        | 32,000             | $0.00000200            | $0.00000600             |
| mistral/mistral-medium                                                                      | 8,191        | 32,000             | $0.00000270            | $0.00000810             |
| mistral/mistral-medium-latest                                                               | 8,191        | 32,000             | $0.00000270            | $0.00000810             |
| mistral/mistral-medium-2312                                                                 | 8,191        | 32,000             | $0.00000270            | $0.00000810             |
| mistral/mistral-large-latest                                                                | 8,191        | 32,000             | $0.00000800            | $0.00002400             |
| mistral/mistral-large-2402                                                                  | 8,191        | 32,000             | $0.00000800            | $0.00002400             |
| mistral/open-mixtral-8x7b                                                                   | 8,191        | 32,000             | $0.00000200            | $0.00000600             |
| mistral/mistral-embed                                                                       | 8,192        | 8,192              | $0.00000011            | --                      |
| deepseek-chat                                                                               | 4,096        | 32,000             | $0.00000014            | $0.00000028             |
| deepseek-coder                                                                              | 4,096        | 16,000             | $0.00000014            | $0.00000028             |
| groq/llama2-70b-4096                                                                        | 4,096        | 4,096              | $0.00000070            | $0.00000080             |
| groq/llama3-8b-8192                                                                         | 8,192        | 8,192              | $0.00000010            | $0.00000010             |
| groq/llama3-70b-8192                                                                        | 8,192        | 8,192              | $0.00000064            | $0.00000080             |
| groq/mixtral-8x7b-32768                                                                     | 32,768       | 32,768             | $0.00000027            | $0.00000027             |
| groq/gemma-7b-it                                                                            | 8,192        | 8,192              | $0.00000010            | $0.00000010             |
| claude-instant-1.2                                                                          | 8,191        | 100,000            | $0.00000016            | $0.00000055             |
| claude-2                                                                                    | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| claude-2.1                                                                                  | 8,191        | 200,000            | $0.00000800            | $0.00002400             |
| claude-3-haiku-20240307                                                                     | 4,096        | 200,000            | $0.00000025            | $0.00000125             |
| claude-3-opus-20240229                                                                      | 4,096        | 200,000            | $0.00001500            | $0.00007500             |
| claude-3-sonnet-20240229                                                                    | 4,096        | 200,000            | $0.00000300            | $0.00001500             |
| text-bison                                                                                  | 1,024        | 8,192              | $0.00000012            | $0.00000012             |
| text-bison@001                                                                              | 1,024        | 8,192              | $0.00000012            | $0.00000012             |
| text-unicorn                                                                                | 1,024        | 8,192              | $0.00001000            | $0.00002800             |
| text-unicorn@001                                                                            | 1,024        | 8,192              | $0.00001000            | $0.00002800             |
| chat-bison                                                                                  | 4,096        | 8,192              | $0.00000012            | $0.00000012             |
| chat-bison@001                                                                              | 4,096        | 8,192              | $0.00000012            | $0.00000012             |
| chat-bison@002                                                                              | 4,096        | 8,192              | $0.00000012            | $0.00000012             |
| chat-bison-32k                                                                              | 8,192        | 32,000             | $0.00000012            | $0.00000012             |
| code-bison                                                                                  | 1,024        | 6,144              | $0.00000012            | $0.00000012             |
| code-bison@001                                                                              | 1,024        | 6,144              | $0.00000012            | $0.00000012             |
| code-gecko@001                                                                              | 64           | 2,048              | $0.00000012            | $0.00000012             |
| code-gecko@002                                                                              | 64           | 2,048              | $0.00000012            | $0.00000012             |
| code-gecko                                                                                  | 64           | 2,048              | $0.00000012            | $0.00000012             |
| codechat-bison                                                                              | 1,024        | 6,144              | $0.00000012            | $0.00000012             |
| codechat-bison@001                                                                          | 1,024        | 6,144              | $0.00000012            | $0.00000012             |
| codechat-bison-32k                                                                          | 8,192        | 32,000             | $0.00000012            | $0.00000012             |
| gemini-pro                                                                                  | 8,192        | 32,760             | $0.00000025            | $0.00000050             |
| gemini-1.0-pro                                                                              | 8,192        | 32,760             | $0.00000025            | $0.00000050             |
| gemini-1.0-pro-001                                                                          | 8,192        | 32,760             | $0.00000025            | $0.00000050             |
| gemini-1.0-pro-002                                                                          | 8,192        | 32,760             | $0.00000025            | $0.00000050             |
| gemini-1.5-pro                                                                              | 8,192        | 1,000,000          | $0.00000063            | $0.00000188             |
| gemini-1.5-pro-preview-0215                                                                 | 8,192        | 1,000,000          | $0.00000063            | $0.00000188             |
| gemini-1.5-pro-preview-0409                                                                 | 8,192        | 1,000,000          | $0.00000063            | $0.00000188             |
| gemini-experimental                                                                         | 8,192        | 1,000,000          | $0.00000000            | $0.00000000             |
| gemini-pro-vision                                                                           | 2,048        | 16,384             | $0.00000025            | $0.00000050             |
| gemini-1.0-pro-vision                                                                       | 2,048        | 16,384             | $0.00000025            | $0.00000050             |
| gemini-1.0-pro-vision-001                                                                   | 2,048        | 16,384             | $0.00000025            | $0.00000050             |
| vertex_ai/claude-3-sonnet@20240229                                                          | 4,096        | 200,000            | $0.00000300            | $0.00001500             |
| vertex_ai/claude-3-haiku@20240307                                                           | 4,096        | 200,000            | $0.00000025            | $0.00000125             |
| vertex_ai/claude-3-opus@20240229                                                            | 4,096        | 200,000            | $0.00000150            | $0.00000750             |
| textembedding-gecko                                                                         | 3,072        | 3,072              | $0.00000001            | $0.00000000             |
| textembedding-gecko-multilingual                                                            | 3,072        | 3,072              | $0.00000001            | $0.00000000             |
| textembedding-gecko-multilingual@001                                                        | 3,072        | 3,072              | $0.00000001            | $0.00000000             |
| textembedding-gecko@001                                                                     | 3,072        | 3,072              | $0.00000001            | $0.00000000             |
| textembedding-gecko@003                                                                     | 3,072        | 3,072              | $0.00000001            | $0.00000000             |
| text-embedding-preview-0409                                                                 | 3,072        | 3,072              | $0.00000001            | $0.00000000             |
| text-multilingual-embedding-preview-0409                                                    | 3,072        | 3,072              | $0.00000001            | $0.00000000             |
| palm/chat-bison                                                                             | 4,096        | 8,192              | $0.00000012            | $0.00000012             |
| palm/chat-bison-001                                                                         | 4,096        | 8,192              | $0.00000012            | $0.00000012             |
| palm/text-bison                                                                             | 1,024        | 8,192              | $0.00000012            | $0.00000012             |
| palm/text-bison-001                                                                         | 1,024        | 8,192              | $0.00000012            | $0.00000012             |
| palm/text-bison-safety-off                                                                  | 1,024        | 8,192              | $0.00000012            | $0.00000012             |
| palm/text-bison-safety-recitation-off                                                       | 1,024        | 8,192              | $0.00000012            | $0.00000012             |
| gemini/gemini-pro                                                                           | 8,192        | 32,760             | $0.00000000            | $0.00000000             |
| gemini/gemini-1.5-pro                                                                       | 8,192        | 1,000,000          | $0.00000000            | $0.00000000             |
| gemini/gemini-1.5-pro-latest                                                                | 8,192        | 1,048,576          | $0.00000000            | $0.00000000             |
| gemini/gemini-pro-vision                                                                    | 2,048        | 30,720             | $0.00000000            | $0.00000000             |
| command-r                                                                                   | 4,096        | 128,000            | $0.00000050            | $0.00000150             |
| command-light                                                                               | 4,096        | 4,096              | $0.00001500            | $0.00001500             |
| command-r-plus                                                                              | 4,096        | 128,000            | $0.00000300            | $0.00001500             |
| command-nightly                                                                             | 4,096        | 4,096              | $0.00001500            | $0.00001500             |
| command                                                                                     | 4,096        | 4,096              | $0.00001500            | $0.00001500             |
| command-medium-beta                                                                         | 4,096        | 4,096              | $0.00001500            | $0.00001500             |
| command-xlarge-beta                                                                         | 4,096        | 4,096              | $0.00001500            | $0.00001500             |
| replicate/meta/llama-2-13b                                                                  | 4,096        | 4,096              | $0.00000010            | $0.00000050             |
| replicate/meta/llama-2-13b-chat                                                             | 4,096        | 4,096              | $0.00000010            | $0.00000050             |
| replicate/meta/llama-2-70b                                                                  | 4,096        | 4,096              | $0.00000065            | $0.00000275             |
| replicate/meta/llama-2-70b-chat                                                             | 4,096        | 4,096              | $0.00000065            | $0.00000275             |
| replicate/meta/llama-2-7b                                                                   | 4,096        | 4,096              | $0.00000005            | $0.00000025             |
| replicate/meta/llama-2-7b-chat                                                              | 4,096        | 4,096              | $0.00000005            | $0.00000025             |
| replicate/meta/llama-3-70b                                                                  | 4,096        | 4,096              | $0.00000065            | $0.00000275             |
| replicate/meta/llama-3-70b-instruct                                                         | 4,096        | 4,096              | $0.00000065            | $0.00000275             |
| replicate/meta/llama-3-8b                                                                   | 4,096        | 4,096              | $0.00000005            | $0.00000025             |
| replicate/meta/llama-3-8b-instruct                                                          | 4,096        | 4,096              | $0.00000005            | $0.00000025             |
| replicate/mistralai/mistral-7b-v0.1                                                         | 4,096        | 4,096              | $0.00000005            | $0.00000025             |
| replicate/mistralai/mistral-7b-instruct-v0.2                                                | 4,096        | 4,096              | $0.00000005            | $0.00000025             |
| replicate/mistralai/mixtral-8x7b-instruct-v0.1                                              | 4,096        | 4,096              | $0.00000030            | $0.00000100             |
| openrouter/microsoft/wizardlm-2-8x22b:nitro                                                 | 65,536       | nan                | $0.00000100            | $0.00000100             |
| openrouter/google/gemini-pro-1.5                                                            | 8,192        | 1,000,000          | $0.00000250            | $0.00000750             |
| openrouter/mistralai/mixtral-8x22b-instruct                                                 | 65,536       | nan                | $0.00000065            | $0.00000065             |
| openrouter/cohere/command-r-plus                                                            | 128,000      | nan                | $0.00000300            | $0.00001500             |
| openrouter/databricks/dbrx-instruct                                                         | 32,768       | nan                | $0.00000060            | $0.00000060             |
| openrouter/anthropic/claude-3-haiku                                                         | 200,000      | nan                | $0.00000025            | $0.00000125             |
| openrouter/anthropic/claude-3-sonnet                                                        | 200,000      | nan                | $0.00000300            | $0.00001500             |
| openrouter/mistralai/mistral-large                                                          | 32,000       | nan                | $0.00000800            | $0.00002400             |
| openrouter/cognitivecomputations/dolphin-mixtral-8x7b                                       | 32,769       | nan                | $0.00000050            | $0.00000050             |
| openrouter/google/gemini-pro-vision                                                         | 45,875       | nan                | $0.00000012            | $0.00000038             |
| openrouter/fireworks/firellava-13b                                                          | 4,096        | nan                | $0.00000020            | $0.00000020             |
| openrouter/meta-llama/llama-3-8b-instruct:free                                              | 8,192        | nan                | $0.00000000            | $0.00000000             |
| openrouter/meta-llama/llama-3-8b-instruct:extended                                          | 16,384       | nan                | $0.00000022            | $0.00000225             |
| openrouter/meta-llama/llama-3-70b-instruct:nitro                                            | 8,192        | nan                | $0.00000090            | $0.00000090             |
| openrouter/meta-llama/llama-3-70b-instruct                                                  | 8,192        | nan                | $0.00000059            | $0.00000079             |
| openrouter/openai/gpt-4-vision-preview                                                      | 130,000      | nan                | $0.00001000            | $0.00003000             |
| openrouter/openai/gpt-3.5-turbo                                                             | 4,095        | nan                | $0.00000150            | $0.00000200             |
| openrouter/openai/gpt-3.5-turbo-16k                                                         | 16,383       | nan                | $0.00000300            | $0.00000400             |
| openrouter/openai/gpt-4                                                                     | 8,192        | nan                | $0.00003000            | $0.00006000             |
| openrouter/anthropic/claude-instant-v1                                                      | 100,000      | nan                | $0.00000163            | $0.00000551             |
| openrouter/anthropic/claude-2                                                               | 100,000      | nan                | $0.00001102            | $0.00003268             |
| openrouter/anthropic/claude-3-opus                                                          | 4,096        | 200,000            | $0.00001500            | $0.00007500             |
| openrouter/google/palm-2-chat-bison                                                         | 25,804       | nan                | $0.00000050            | $0.00000050             |
| openrouter/google/palm-2-codechat-bison                                                     | 20,070       | nan                | $0.00000050            | $0.00000050             |
| openrouter/meta-llama/llama-2-13b-chat                                                      | 4,096        | nan                | $0.00000020            | $0.00000020             |
| openrouter/meta-llama/llama-2-70b-chat                                                      | 4,096        | nan                | $0.00000150            | $0.00000150             |
| openrouter/meta-llama/codellama-34b-instruct                                                | 8,096        | nan                | $0.00000050            | $0.00000050             |
| openrouter/nousresearch/nous-hermes-llama2-13b                                              | 4,096        | nan                | $0.00000020            | $0.00000020             |
| openrouter/mancer/weaver                                                                    | 8,000        | nan                | $0.00000563            | $0.00000563             |
| openrouter/gryphe/mythomax-l2-13b                                                           | 8,192        | nan                | $0.00000188            | $0.00000188             |
| openrouter/jondurbin/airoboros-l2-70b-2.1                                                   | 4,096        | nan                | $0.00001388            | $0.00001388             |
| openrouter/undi95/remm-slerp-l2-13b                                                         | 6,144        | nan                | $0.00000188            | $0.00000188             |
| openrouter/pygmalionai/mythalion-13b                                                        | 4,096        | nan                | $0.00000188            | $0.00000188             |
| openrouter/mistralai/mistral-7b-instruct                                                    | 8,192        | nan                | $0.00000013            | $0.00000013             |
| openrouter/mistralai/mistral-7b-instruct:free                                               | 8,192        | nan                | $0.00000000            | $0.00000000             |
| j2-ultra                                                                                    | 8,192        | 8,192              | $0.00001500            | $0.00001500             |
| j2-mid                                                                                      | 8,192        | 8,192              | $0.00001000            | $0.00001000             |
| j2-light                                                                                    | 8,192        | 8,192              | $0.00000300            | $0.00000300             |
| dolphin                                                                                     | 16,384       | 16,384             | $0.00000050            | $0.00000050             |
| chatdolphin                                                                                 | 16,384       | 16,384             | $0.00000050            | $0.00000050             |
| luminous-base                                                                               | 2,048        | nan                | $0.00003000            | $0.00003300             |
| luminous-base-control                                                                       | 2,048        | nan                | $0.00003750            | $0.00004125             |
| luminous-extended                                                                           | 2,048        | nan                | $0.00004500            | $0.00004950             |
| luminous-extended-control                                                                   | 2,048        | nan                | $0.00005625            | $0.00006187             |
| luminous-supreme                                                                            | 2,048        | nan                | $0.00017500            | $0.00019250             |
| luminous-supreme-control                                                                    | 2,048        | nan                | $0.00021875            | $0.00024063             |
| ai21.j2-mid-v1                                                                              | 8,191        | 8,191              | $0.00001250            | $0.00001250             |
| ai21.j2-ultra-v1                                                                            | 8,191        | 8,191              | $0.00001880            | $0.00001880             |
| amazon.titan-text-lite-v1                                                                   | 4,000        | 42,000             | $0.00000030            | $0.00000040             |
| amazon.titan-text-express-v1                                                                | 8,000        | 42,000             | $0.00000130            | $0.00000170             |
| amazon.titan-embed-text-v1                                                                  | 8,192        | 8,192              | $0.00000010            | $0.00000000             |
| amazon.titan-embed-text-v2:0                                                                | 8,192        | 8,192              | $0.00000020            | $0.00000000             |
| mistral.mistral-7b-instruct-v0:2                                                            | 8,191        | 32,000             | $0.00000015            | $0.00000020             |
| mistral.mixtral-8x7b-instruct-v0:1                                                          | 8,191        | 32,000             | $0.00000045            | $0.00000070             |
| mistral.mistral-large-2402-v1:0                                                             | 8,191        | 32,000             | $0.00000800            | $0.00002400             |
| bedrock/us-west-2/mistral.mixtral-8x7b-instruct-v0:1                                        | 8,191        | 32,000             | $0.00000045            | $0.00000070             |
| bedrock/us-east-1/mistral.mixtral-8x7b-instruct-v0:1                                        | 8,191        | 32,000             | $0.00000045            | $0.00000070             |
| bedrock/eu-west-3/mistral.mixtral-8x7b-instruct-v0:1                                        | 8,191        | 32,000             | $0.00000059            | $0.00000091             |
| bedrock/us-west-2/mistral.mistral-7b-instruct-v0:2                                          | 8,191        | 32,000             | $0.00000015            | $0.00000020             |
| bedrock/us-east-1/mistral.mistral-7b-instruct-v0:2                                          | 8,191        | 32,000             | $0.00000015            | $0.00000020             |
| bedrock/eu-west-3/mistral.mistral-7b-instruct-v0:2                                          | 8,191        | 32,000             | $0.00000020            | $0.00000026             |
| bedrock/us-east-1/mistral.mistral-large-2402-v1:0                                           | 8,191        | 32,000             | $0.00000800            | $0.00002400             |
| bedrock/us-west-2/mistral.mistral-large-2402-v1:0                                           | 8,191        | 32,000             | $0.00000800            | $0.00002400             |
| bedrock/eu-west-3/mistral.mistral-large-2402-v1:0                                           | 8,191        | 32,000             | $0.00001040            | $0.00003120             |
| anthropic.claude-3-sonnet-20240229-v1:0                                                     | 4,096        | 200,000            | $0.00000300            | $0.00001500             |
| anthropic.claude-3-haiku-20240307-v1:0                                                      | 4,096        | 200,000            | $0.00000025            | $0.00000125             |
| anthropic.claude-3-opus-20240229-v1:0                                                       | 4,096        | 200,000            | $0.00001500            | $0.00007500             |
| anthropic.claude-v1                                                                         | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/us-east-1/anthropic.claude-v1                                                       | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/us-west-2/anthropic.claude-v1                                                       | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/ap-northeast-1/anthropic.claude-v1                                                  | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/ap-northeast-1/1-month-commitment/anthropic.claude-v1                               | 8,191        | 100,000            | --                     | --                      |
| bedrock/ap-northeast-1/6-month-commitment/anthropic.claude-v1                               | 8,191        | 100,000            | --                     | --                      |
| bedrock/eu-central-1/anthropic.claude-v1                                                    | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/eu-central-1/1-month-commitment/anthropic.claude-v1                                 | 8,191        | 100,000            | --                     | --                      |
| bedrock/eu-central-1/6-month-commitment/anthropic.claude-v1                                 | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-east-1/1-month-commitment/anthropic.claude-v1                                    | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-east-1/6-month-commitment/anthropic.claude-v1                                    | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-west-2/1-month-commitment/anthropic.claude-v1                                    | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-west-2/6-month-commitment/anthropic.claude-v1                                    | 8,191        | 100,000            | --                     | --                      |
| anthropic.claude-v2                                                                         | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/us-east-1/anthropic.claude-v2                                                       | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/us-west-2/anthropic.claude-v2                                                       | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/ap-northeast-1/anthropic.claude-v2                                                  | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/ap-northeast-1/1-month-commitment/anthropic.claude-v2                               | 8,191        | 100,000            | --                     | --                      |
| bedrock/ap-northeast-1/6-month-commitment/anthropic.claude-v2                               | 8,191        | 100,000            | --                     | --                      |
| bedrock/eu-central-1/anthropic.claude-v2                                                    | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/eu-central-1/1-month-commitment/anthropic.claude-v2                                 | 8,191        | 100,000            | --                     | --                      |
| bedrock/eu-central-1/6-month-commitment/anthropic.claude-v2                                 | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-east-1/1-month-commitment/anthropic.claude-v2                                    | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-east-1/6-month-commitment/anthropic.claude-v2                                    | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-west-2/1-month-commitment/anthropic.claude-v2                                    | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-west-2/6-month-commitment/anthropic.claude-v2                                    | 8,191        | 100,000            | --                     | --                      |
| anthropic.claude-v2:1                                                                       | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/us-east-1/anthropic.claude-v2:1                                                     | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/us-west-2/anthropic.claude-v2:1                                                     | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/ap-northeast-1/anthropic.claude-v2:1                                                | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/ap-northeast-1/1-month-commitment/anthropic.claude-v2:1                             | 8,191        | 100,000            | --                     | --                      |
| bedrock/ap-northeast-1/6-month-commitment/anthropic.claude-v2:1                             | 8,191        | 100,000            | --                     | --                      |
| bedrock/eu-central-1/anthropic.claude-v2:1                                                  | 8,191        | 100,000            | $0.00000800            | $0.00002400             |
| bedrock/eu-central-1/1-month-commitment/anthropic.claude-v2:1                               | 8,191        | 100,000            | --                     | --                      |
| bedrock/eu-central-1/6-month-commitment/anthropic.claude-v2:1                               | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-east-1/1-month-commitment/anthropic.claude-v2:1                                  | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-east-1/6-month-commitment/anthropic.claude-v2:1                                  | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-west-2/1-month-commitment/anthropic.claude-v2:1                                  | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-west-2/6-month-commitment/anthropic.claude-v2:1                                  | 8,191        | 100,000            | --                     | --                      |
| anthropic.claude-instant-v1                                                                 | 8,191        | 100,000            | $0.00000163            | $0.00000551             |
| bedrock/us-east-1/anthropic.claude-instant-v1                                               | 8,191        | 100,000            | $0.00000080            | $0.00000240             |
| bedrock/us-east-1/1-month-commitment/anthropic.claude-instant-v1                            | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-east-1/6-month-commitment/anthropic.claude-instant-v1                            | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-west-2/1-month-commitment/anthropic.claude-instant-v1                            | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-west-2/6-month-commitment/anthropic.claude-instant-v1                            | 8,191        | 100,000            | --                     | --                      |
| bedrock/us-west-2/anthropic.claude-instant-v1                                               | 8,191        | 100,000            | $0.00000080            | $0.00000240             |
| bedrock/ap-northeast-1/anthropic.claude-instant-v1                                          | 8,191        | 100,000            | $0.00000223            | $0.00000755             |
| bedrock/ap-northeast-1/1-month-commitment/anthropic.claude-instant-v1                       | 8,191        | 100,000            | --                     | --                      |
| bedrock/ap-northeast-1/6-month-commitment/anthropic.claude-instant-v1                       | 8,191        | 100,000            | --                     | --                      |
| bedrock/eu-central-1/anthropic.claude-instant-v1                                            | 8,191        | 100,000            | $0.00000248            | $0.00000838             |
| bedrock/eu-central-1/1-month-commitment/anthropic.claude-instant-v1                         | 8,191        | 100,000            | --                     | --                      |
| bedrock/eu-central-1/6-month-commitment/anthropic.claude-instant-v1                         | 8,191        | 100,000            | --                     | --                      |
| cohere.command-text-v14                                                                     | 4,096        | 4,096              | $0.00000150            | $0.00000200             |
| bedrock/*/1-month-commitment/cohere.command-text-v14                                        | 4,096        | 4,096              | --                     | --                      |
| bedrock/*/6-month-commitment/cohere.command-text-v14                                        | 4,096        | 4,096              | --                     | --                      |
| cohere.command-light-text-v14                                                               | 4,096        | 4,096              | $0.00000030            | $0.00000060             |
| bedrock/*/1-month-commitment/cohere.command-light-text-v14                                  | 4,096        | 4,096              | --                     | --                      |
| bedrock/*/6-month-commitment/cohere.command-light-text-v14                                  | 4,096        | 4,096              | --                     | --                      |
| cohere.command-r-plus-v1:0                                                                  | 4,096        | 128,000            | $0.00000300            | $0.00001500             |
| cohere.command-r-v1:0                                                                       | 4,096        | 128,000            | $0.00000050            | $0.00000150             |
| cohere.embed-english-v3                                                                     | 512          | 512                | $0.00000010            | $0.00000000             |
| cohere.embed-multilingual-v3                                                                | 512          | 512                | $0.00000010            | $0.00000000             |
| meta.llama2-13b-chat-v1                                                                     | 4,096        | 4,096              | $0.00000075            | $0.00000100             |
| meta.llama2-70b-chat-v1                                                                     | 4,096        | 4,096              | $0.00000195            | $0.00000256             |
| meta.llama3-8b-instruct-v1:0                                                                | 8,192        | 8,192              | $0.00000040            | $0.00000060             |
| meta.llama3-70b-instruct-v1:0                                                               | 8,192        | 8,192              | $0.00000265            | $0.00000350             |
| 512-x-512/50-steps/stability.stable-diffusion-xl-v0                                         | 77           | 77                 | --                     | --                      |
| 512-x-512/max-steps/stability.stable-diffusion-xl-v0                                        | 77           | 77                 | --                     | --                      |
| max-x-max/50-steps/stability.stable-diffusion-xl-v0                                         | 77           | 77                 | --                     | --                      |
| max-x-max/max-steps/stability.stable-diffusion-xl-v0                                        | 77           | 77                 | --                     | --                      |
| 1024-x-1024/50-steps/stability.stable-diffusion-xl-v1                                       | 77           | 77                 | --                     | --                      |
| 1024-x-1024/max-steps/stability.stable-diffusion-xl-v1                                      | 77           | 77                 | --                     | --                      |
| sagemaker/meta-textgeneration-llama-2-7b                                                    | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| sagemaker/meta-textgeneration-llama-2-7b-f                                                  | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| sagemaker/meta-textgeneration-llama-2-13b                                                   | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| sagemaker/meta-textgeneration-llama-2-13b-f                                                 | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| sagemaker/meta-textgeneration-llama-2-70b                                                   | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| sagemaker/meta-textgeneration-llama-2-70b-b-f                                               | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| together-ai-up-to-3b                                                                        | nan          | nan                | $0.00000010            | $0.00000010             |
| together-ai-3.1b-7b                                                                         | nan          | nan                | $0.00000020            | $0.00000020             |
| together-ai-7.1b-20b                                                                        | 1,000        | nan                | $0.00000040            | $0.00000040             |
| together-ai-20.1b-40b                                                                       | nan          | nan                | $0.00000080            | $0.00000080             |
| together-ai-40.1b-70b                                                                       | nan          | nan                | $0.00000090            | $0.00000090             |
| together_ai/mistralai/Mixtral-8x7B-Instruct-v0.1                                            | nan          | nan                | $0.00000060            | $0.00000060             |
| together_ai/mistralai/Mistral-7B-Instruct-v0.1                                              | nan          | nan                | --                     | --                      |
| together_ai/togethercomputer/CodeLlama-34b-Instruct                                         | nan          | nan                | --                     | --                      |
| ollama/llama2                                                                               | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| ollama/llama2:13b                                                                           | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| ollama/llama2:70b                                                                           | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| ollama/llama2-uncensored                                                                    | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| ollama/mistral                                                                              | 8,192        | 8,192              | $0.00000000            | $0.00000000             |
| ollama/codellama                                                                            | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| ollama/orca-mini                                                                            | 4,096        | 4,096              | $0.00000000            | $0.00000000             |
| ollama/vicuna                                                                               | 2,048        | 2,048              | $0.00000000            | $0.00000000             |
| deepinfra/lizpreciatior/lzlv_70b_fp16_hf                                                    | 4,096        | 4,096              | $0.00000070            | $0.00000090             |
| deepinfra/Gryphe/MythoMax-L2-13b                                                            | 4,096        | 4,096              | $0.00000022            | $0.00000022             |
| deepinfra/mistralai/Mistral-7B-Instruct-v0.1                                                | 8,191        | 32,768             | $0.00000013            | $0.00000013             |
| deepinfra/meta-llama/Llama-2-70b-chat-hf                                                    | 4,096        | 4,096              | $0.00000070            | $0.00000090             |
| deepinfra/cognitivecomputations/dolphin-2.6-mixtral-8x7b                                    | 8,191        | 32,768             | $0.00000027            | $0.00000027             |
| deepinfra/codellama/CodeLlama-34b-Instruct-hf                                               | 4,096        | 4,096              | $0.00000060            | $0.00000060             |
| deepinfra/deepinfra/mixtral                                                                 | 4,096        | 32,000             | $0.00000027            | $0.00000027             |
| deepinfra/Phind/Phind-CodeLlama-34B-v2                                                      | 4,096        | 16,384             | $0.00000060            | $0.00000060             |
| deepinfra/mistralai/Mixtral-8x7B-Instruct-v0.1                                              | 8,191        | 32,768             | $0.00000027            | $0.00000027             |
| deepinfra/deepinfra/airoboros-70b                                                           | 4,096        | 4,096              | $0.00000070            | $0.00000090             |
| deepinfra/01-ai/Yi-34B-Chat                                                                 | 4,096        | 4,096              | $0.00000060            | $0.00000060             |
| deepinfra/01-ai/Yi-6B-200K                                                                  | 4,096        | 200,000            | $0.00000013            | $0.00000013             |
| deepinfra/jondurbin/airoboros-l2-70b-gpt4-1.4.1                                             | 4,096        | 4,096              | $0.00000070            | $0.00000090             |
| deepinfra/meta-llama/Llama-2-13b-chat-hf                                                    | 4,096        | 4,096              | $0.00000022            | $0.00000022             |
| deepinfra/amazon/MistralLite                                                                | 8,191        | 32,768             | $0.00000020            | $0.00000020             |
| deepinfra/meta-llama/Llama-2-7b-chat-hf                                                     | 4,096        | 4,096              | $0.00000013            | $0.00000013             |
| deepinfra/01-ai/Yi-34B-200K                                                                 | 4,096        | 200,000            | $0.00000060            | $0.00000060             |
| deepinfra/openchat/openchat_3.5                                                             | 4,096        | 4,096              | $0.00000013            | $0.00000013             |
| perplexity/codellama-34b-instruct                                                           | 16,384       | 16,384             | $0.00000035            | $0.00000140             |
| perplexity/codellama-70b-instruct                                                           | 16,384       | 16,384             | $0.00000070            | $0.00000280             |
| perplexity/pplx-7b-chat                                                                     | 8,192        | 8,192              | $0.00000007            | $0.00000028             |
| perplexity/pplx-70b-chat                                                                    | 4,096        | 4,096              | $0.00000070            | $0.00000280             |
| perplexity/pplx-7b-online                                                                   | 4,096        | 4,096              | $0.00000000            | $0.00000028             |
| perplexity/pplx-70b-online                                                                  | 4,096        | 4,096              | $0.00000000            | $0.00000280             |
| perplexity/llama-2-70b-chat                                                                 | 4,096        | 4,096              | $0.00000070            | $0.00000280             |
| perplexity/mistral-7b-instruct                                                              | 4,096        | 4,096              | $0.00000007            | $0.00000028             |
| perplexity/mixtral-8x7b-instruct                                                            | 4,096        | 4,096              | $0.00000007            | $0.00000028             |
| perplexity/sonar-small-chat                                                                 | 16,384       | 16,384             | $0.00000007            | $0.00000028             |
| perplexity/sonar-small-online                                                               | 12,000       | 12,000             | $0.00000000            | $0.00000028             |
| perplexity/sonar-medium-chat                                                                | 16,384       | 16,384             | $0.00000060            | $0.00000180             |
| perplexity/sonar-medium-online                                                              | 12,000       | 12,000             | $0.00000000            | $0.00000180             |
| anyscale/mistralai/Mistral-7B-Instruct-v0.1                                                 | 16,384       | 16,384             | $0.00000015            | $0.00000015             |
| anyscale/Mixtral-8x7B-Instruct-v0.1                                                         | 16,384       | 16,384             | $0.00000015            | $0.00000015             |
| anyscale/HuggingFaceH4/zephyr-7b-beta                                                       | 16,384       | 16,384             | $0.00000015            | $0.00000015             |
| anyscale/meta-llama/Llama-2-7b-chat-hf                                                      | 4,096        | 4,096              | $0.00000015            | $0.00000015             |
| anyscale/meta-llama/Llama-2-13b-chat-hf                                                     | 4,096        | 4,096              | $0.00000025            | $0.00000025             |
| anyscale/meta-llama/Llama-2-70b-chat-hf                                                     | 4,096        | 4,096              | $0.00000100            | $0.00000100             |
| anyscale/codellama/CodeLlama-34b-Instruct-hf                                                | 4,096        | 4,096              | $0.00000100            | $0.00000100             |
| cloudflare/@cf/meta/llama-2-7b-chat-fp16                                                    | 3,072        | 3,072              | $0.00000192            | $0.00000192             |
| cloudflare/@cf/meta/llama-2-7b-chat-int8                                                    | 2,048        | 2,048              | $0.00000192            | $0.00000192             |
| cloudflare/@cf/mistral/mistral-7b-instruct-v0.1                                             | 8,192        | 8,192              | $0.00000192            | $0.00000192             |
| cloudflare/@hf/thebloke/codellama-7b-instruct-awq                                           | 4,096        | 4,096              | $0.00000192            | $0.00000192             |
| voyage/voyage-01                                                                            | 4,096        | 4,096              | $0.00000010            | $0.00000000             |
| voyage/voyage-lite-01                                                                       | 4,096        | 4,096              | $0.00000010            | $0.00000000             |
| voyage/voyage-large-2                                                                       | 16,000       | 16,000             | $0.00000012            | $0.00000000             |
| voyage/voyage-law-2                                                                         | 16,000       | 16,000             | $0.00000012            | $0.00000000             |
| voyage/voyage-code-2                                                                        | 16,000       | 16,000             | $0.00000012            | $0.00000000             |
| voyage/voyage-2                                                                             | 4,000        | 4,000              | $0.00000010            | $0.00000000             |
| voyage/voyage-lite-02-instruct                                                              | 4,000        | 4,000              | $0.00000010            | $0.00000000             |

### Callback handlers
You may also calculate token costs in LLM wrapper/framework libraries using callbacks. 
#### LlamaIndex
```sh
pip install `'tokencost[llama-index]'`
```
To use the base callback handler, you may import it:

```python
from tokencost.callbacks.llama_index import BaseCallbackHandler
```

and pass to your framework callback handler.

#### Langchain
(Coming Soon)

### Running locally

#### Installation via [GitHub](https://github.com/AgentOps-AI/tokencost):

```bash
git clone git@github.com:AgentOps-AI/tokencost.git
cd tokencost
pip install -e .
```

## Running tests

0. Install `pytest` if you don't have it already

```shell
pip install pytest
```

1. Run the `tests/` folder while in the parent directory

```shell
pytest tests
```

This repo also supports `tox`, simply run `python -m tox`.

## Contributing

Contributions to TokenCost are welcome! Feel free to create an [issue](https://github.com/AgentOps-AI/tokencost/issues) for any bug reports, complaints, or feature suggestions.

## License

TokenCost is released under the MIT License.
