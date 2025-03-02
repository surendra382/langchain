
# Introduction to LangChain

LangChain is a powerful framework that facilitates the creation of complex language model applications. While simple summarization tasks may not require LangChain, it shines in applications that involve multiple components working together to deliver advanced results.

In this README, we will explore the LangChain ecosystem, which includes the following:

- **LangChain Core**: The core framework that supports LangChain Expression Language (LCEL) for chaining components together.
- **LangChain Community**: A collection of third-party integrations with data sources and language models.
- **LangChain-specific packages**: Packages like `langchain-openai` for using OpenAI's models, `langchain-groq` for integrating Groq models, and more.
- **LangServe**: A package to deploy LangChain Runnables and Chains as REST APIs.
- **LangSmith**: An observability platform for debugging and tracing LangChain applications.

LangChain allows you to chain various components such as models, prompts, and output parsers, making it an essential tool for creating complex workflows with language models.

## Key Features

- **Runnables**: Flexible objects that expose a standard interface for interaction, making it easy to switch between different service providers.
- **Prompt Templates**: Streamline prompt creation with dynamic templates.
- **Output Parsers**: Ensure that model outputs are consistent and in the desired format.
- **Chains**: Modular sequences of components that work together to solve complex tasks.
- **Streaming**: Allows real-time generation of outputs with LangChain’s streaming capabilities.

## Installation

To install LangChain and its integrations, use the following commands:

```bash
pip install langchain
pip install langchain-openai
pip install langchain-groq
```

## Example Code Walkthrough

### Initializing LangChain Models

We begin by setting up API keys for OpenAI and Groq models:

```python
import getpass
import os

# Set OpenAI API key
os.environ["OPENAI_API_KEY"] = getpass.getpass()

from langchain_openai import ChatOpenAI
model_openai = ChatOpenAI()  # default model is 'gpt-3.5-turbo'

# Set Groq API key
os.environ["GROQ_API_KEY"] = getpass.getpass()

from langchain_groq import ChatGroq
model_groq = ChatGroq(model="llama3-8b-8192")
```

### Using Runnables

LangChain uses `Runnable` components like `ChatOpenAI` and `ChatGroq` that offer flexible interaction with models. Here’s how to invoke them with a sample input:

```python
from langchain_core.messages import HumanMessage, SystemMessage

messages = [
    SystemMessage(content="Translate the following from French into English"),
    HumanMessage(content="Bonjour ! Il fait très beau à Paris aujourd'hui."),
]

response_openai = model_openai.invoke(messages)
response_groq = model_groq.invoke(messages)

print(response_openai)
print(response_groq)
```

### Output Parsers

In LangChain, output parsers help ensure that the output from models is structured consistently. For example, the `StrOutputParser` ensures that the result is always a string:

```python
from langchain_core.output_parsers import StrOutputParser

parser = StrOutputParser()

parsed_openai_response = parser.invoke(response_openai)
parsed_groq_response = parser.invoke(response_groq)

print(parsed_openai_response)
print(parsed_groq_response)
```

### Chaining Components

LangChain allows you to chain multiple components using the `|` operator, simplifying workflows. Below is an example of chaining a prompt template, a model, and an output parser:

```python
from langchain_core.prompts import ChatPromptTemplate

prompt_template = ChatPromptTemplate.from_messages([
    ("system", "Translate the following into {language}:"),
    ("user", "{text}")
])

chain = prompt_template | model_groq | parser
result = chain.invoke({"language": "italian", "text": "hi"})
print(result)
```

### Advanced Features

- **Streaming**: You can stream outputs in real-time, as shown in the following example:

```python
import nest_asyncio
nest_asyncio.apply()

prompt = ChatPromptTemplate.from_template("tell me a story about {topic}")
chain = prompt | model_groq | parser

async for chunk in chain.astream({"topic": "parrot"}):
    print(chunk, end="", flush=True)
```

- **Message Placeholders**: Create dynamic conversation history in chat prompts using `MessagesPlaceholder`:

```python
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_core.messages import HumanMessage

prompt_template = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant"),
    MessagesPlaceholder("msgs")
])

result = prompt_template.invoke({"msgs": [HumanMessage(content="hi!")]})
print(result.to_messages())
```

### Conclusion

LangChain provides a comprehensive framework for developing advanced language model applications. With its ability to chain components together and integrate various service providers, LangChain is perfect for building complex workflows and dynamic, real-time applications.
