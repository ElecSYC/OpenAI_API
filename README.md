OpenAI Python Library
The OpenAI Python library provides convenient access to the OpenAI API from applications written in the Python language. It includes a pre-defined set of classes for API resources that initialize themselves dynamically from API responses which makes it compatible with a wide range of versions of the OpenAI API.

You can find usage examples for the OpenAI Python library in our API reference and the OpenAI Cookbook.

Installation
You don't need this source code unless you want to modify the package. If you just want to use the package, just run:

pip install --upgrade openai
Install from source with:

python setup.py install
Optional dependencies
Install dependencies for openai.embeddings_utils:

pip install openai[embeddings]
Install support for Weights & Biases:

pip install openai[wandb]
Data libraries like numpy and pandas are not installed by default due to their size. They’re needed for some functionality of this library, but generally not for talking to the API. If you encounter a MissingDependencyError, install them with:

pip install openai[datalib]
Usage
The library needs to be configured with your account's secret key which is available on the website. Either set it as the OPENAI_API_KEY environment variable before using the library:

export OPENAI_API_KEY='sk-...'
Or set openai.api_key to its value:

import openai
openai.api_key = "sk-..."

# list models
models = openai.Model.list()

# print the first model's id
print(models.data[0].id)

# create a chat completion
chat_completion = openai.ChatCompletion.create(model="gpt-3.5-turbo", messages=[{"role": "user", "content": "Hello world"}])

# print the chat completion
print(chat_completion.choices[0].message.content)
Params
All endpoints have a .create method that supports a request_timeout param. This param takes a Union[float, Tuple[float, float]] and will raise an openai.error.Timeout error if the request exceeds that time in seconds (See: https://requests.readthedocs.io/en/latest/user/quickstart/#timeouts).

Microsoft Azure Endpoints
In order to use the library with Microsoft Azure endpoints, you need to set the api_type, api_base and api_version in addition to the api_key. The api_type must be set to 'azure' and the others correspond to the properties of your endpoint. In addition, the deployment name must be passed as the engine parameter.

import openai
openai.api_type = "azure"
openai.api_key = "..."
openai.api_base = "https://example-endpoint.openai.azure.com"
openai.api_version = "2023-05-15"

# create a chat completion
chat_completion = openai.ChatCompletion.create(deployment_id="deployment-name", model="gpt-3.5-turbo", messages=[{"role": "user", "content": "Hello world"}])

# print the completion
print(chat_completion.choices[0].message.content)
Please note that for the moment, the Microsoft Azure endpoints can only be used for completion, embedding, and fine-tuning operations. For a detailed example of how to use fine-tuning and other operations using Azure endpoints, please check out the following Jupyter notebooks:

Using Azure completions
Using Azure fine-tuning
Using Azure embeddings
Microsoft Azure Active Directory Authentication
In order to use Microsoft Active Directory to authenticate to your Azure endpoint, you need to set the api_type to "azure_ad" and pass the acquired credential token to api_key. The rest of the parameters need to be set as specified in the previous section.

from azure.identity import DefaultAzureCredential
import openai

# Request credential
default_credential = DefaultAzureCredential()
token = default_credential.get_token("https://cognitiveservices.azure.com/.default")

# Setup parameters
openai.api_type = "azure_ad"
openai.api_key = token.token
openai.api_base = "https://example-endpoint.openai.azure.com/"
openai.api_version = "2023-05-15"

# ...
Command-line interface
This library additionally provides an openai command-line utility which makes it easy to interact with the API from your terminal. Run openai api -h for usage.

# list models
openai api models.list

# create a chat completion (gpt-3.5-turbo, gpt-4, etc.)
openai api chat_completions.create -m gpt-3.5-turbo -g user "Hello world"

# create a completion (text-davinci-003, text-davinci-002, ada, babbage, curie, davinci, etc.)
openai api completions.create -m ada -p "Hello world"

# generate images via DALL·E API
openai api image.create -p "two dogs playing chess, cartoon" -n 1

# using openai through a proxy
openai --proxy=http://proxy.com api models.list
Example code
Examples of how to use this Python library to accomplish various tasks can be found in the OpenAI Cookbook. It contains code examples for:

Classification using fine-tuning
Clustering
Code search
Customizing embeddings
Question answering from a corpus of documents
Recommendations
Visualization of embeddings
And more
Prior to July 2022, this OpenAI Python library hosted code examples in its examples folder, but since then all examples have been migrated to the OpenAI Cookbook.

Chat Completions
Conversational models such as gpt-3.5-turbo can be called using the chat completions endpoint.

import openai
openai.api_key = "sk-..."  # supply your API key however you choose

completion = openai.ChatCompletion.create(model="gpt-3.5-turbo", messages=[{"role": "user", "content": "Hello world"}])
print(completion.choices[0].message.content)
Completions
Text models such as babbage-002 or davinci-002 (and our legacy completions models) can be called using the completions endpoint.

import openai
openai.api_key = "sk-..."  # supply your API key however you choose

completion = openai.Completion.create(model="davinci-002", prompt="Hello world")
print(completion.choices[0].text)
Embeddings
In the OpenAI Python library, an embedding represents a text string as a fixed-length vector of floating point numbers. Embeddings are designed to measure the similarity or relevance between text strings.

To get an embedding for a text string, you can use the embeddings method as follows in Python:

import openai
openai.api_key = "sk-..."  # supply your API key however you choose

# choose text to embed
text_string = "sample text"

# choose an embedding
model_id = "text-embedding-ada-002"

# compute the embedding of the text
embedding = openai.Embedding.create(input=text_string, model=model_id)['data'][0]['embedding']
An example of how to call the embeddings method is shown in this embeddings guide.

Examples of how to use embeddings are shared in the following Jupyter notebooks:

Classification using embeddings
Clustering using embeddings
Code search using embeddings
Semantic text search using embeddings
User and product embeddings
Zero-shot classification using embeddings
Recommendation using embeddings
For more information on embeddings and the types of embeddings OpenAI offers, read the embeddings guide in the OpenAI documentation.

Fine-tuning
Fine-tuning a model on training data can both improve the results (by giving the model more examples to learn from) and lower the cost/latency of API calls by reducing the need to include training examples in prompts.

# Create a fine-tuning job with an already uploaded file
openai.FineTuningJob.create(training_file="file-abc123", model="gpt-3.5-turbo")

# List 10 fine-tuning jobs
openai.FineTuningJob.list(limit=10)

# Retrieve the state of a fine-tune
openai.FineTuningJob.retrieve("ft-abc123")

# Cancel a job
openai.FineTuningJob.cancel("ft-abc123")

# List up to 10 events from a fine-tuning job
openai.FineTuningJob.list_events(id="ft-abc123", limit=10)

# Delete a fine-tuned model (must be an owner of the org the model was created in)
openai.Model.delete("ft-abc123")
For more information on fine-tuning, read the fine-tuning guide in the OpenAI documentation.

Moderation
OpenAI provides a Moderation endpoint that can be used to check whether content complies with the OpenAI content policy

import openai
openai.api_key = "sk-..."  # supply your API key however you choose

moderation_resp = openai.Moderation.create(input="Here is some perfectly innocuous text that follows all OpenAI content policies.")
See the moderation guide for more details.

Image generation (DALL·E)
import openai
openai.api_key = "sk-..."  # supply your API key however you choose

image_resp = openai.Image.create(prompt="two dogs playing chess, oil painting", n=4, size="512x512")
Audio transcription (Whisper)
import openai
openai.api_key = "sk-..."  # supply your API key however you choose
f = open("path/to/file.mp3", "rb")
transcript = openai.Audio.transcribe("whisper-1", f)
Async API
Async support is available in the API by prepending a to a network-bound method:

import openai
openai.api_key = "sk-..."  # supply your API key however you choose

async def create_chat_completion():
    chat_completion_resp = await openai.ChatCompletion.acreate(model="gpt-3.5-turbo", messages=[{"role": "user", "content": "Hello world"}])
To make async requests more efficient, you can pass in your own aiohttp.ClientSession, but you must manually close the client session at the end of your program/event loop:

import openai
from aiohttp import ClientSession

openai.aiosession.set(ClientSession())
# At the end of your program, close the http session
await openai.aiosession.get().close()
See the usage guide for more details.

Requirements
Python 3.7.1+
In general, we want to support the versions of Python that our customers are using. If you run into problems with any version issues, please let us know on our support page.

Credit
This library is forked from the Stripe Python Library.
 
