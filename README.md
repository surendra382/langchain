
# Introduction to LangChain

LangChain is a powerful framework for building complex applications using large language models (LLMs). It is designed for advanced use cases that require multiple components to work together, far beyond simple summarization tasks. Whether you're building conversational agents, document processing systems, or anything that involves LLMs working together with various tools, LangChain provides a highly flexible and modular structure.

## Table of Contents

1. [What is LangChain?](#what-is-langchain)
2. [The LangChain Ecosystem](#the-langchain-ecosystem)
3. [Installing LangChain](#installing-langchain)
4. [Working with LangChain Models](#working-with-langchain-models)
5. [Runnables and Integrations](#runnables-and-integrations)
6. [Prompt Templates](#prompt-templates)
    - [String Prompt Templates](#string-prompt-templates)
    - [Chat Prompt Templates](#chat-prompt-templates)
7. [Message Placeholders](#message-placeholders)
8. [Chains](#chains)
9. [Exercises and Labs](#exercises-and-labs)
10. [Conclusion](#conclusion)

## What is LangChain?

LangChain is a framework that enables users to create complex LLM applications by chaining components like models, prompts, and output parsers. If your application needs to call multiple APIs or process data through various stages, LangChain allows you to set up these workflows effortlessly.

LangChain allows for the integration of models such as OpenAI, Groq, and others, enabling users to easily switch between them with minimal effort.

## The LangChain Ecosystem

LangChain is composed of several parts that enable complex workflows:

- **LangChain Core**: The core package that includes LCEL (LangChain Expression Language), which helps connect various components together.
- **LangChain Community**: A collection of third-party integrations, including vector databases, parsers, and data connectors.
- **LangChain-specific packages**: Packages like `langchain-openai` to integrate OpenAI’s models and others.
- **LangServe**: A deployment tool for exposing LangChain's Runnables and Chains as REST APIs.
- **LangSmith**: An observability platform for debugging and tracing LangChain applications in real-time.

## Installing LangChain

To get started, you'll need to install the LangChain core package along with any specific integrations you need. For example, to integrate OpenAI's models:

```bash
pip install langchain
pip install langchain-openai
pip install langchain-groq
```

These commands install LangChain and the integrations for OpenAI and Groq. Some integrations, such as Groq, come in separate packages.

## Working with LangChain Models

LangChain offers an abstraction for different models, allowing you to interact with models from various providers in a unified way.

Here’s an example of how you would set up and interact with OpenAI’s model using LangChain:

```python
import getpass
import os

# Set your OpenAI API key
os.environ["OPENAI_API_KEY"] = getpass.getpass()

from langchain_openai import ChatOpenAI
model_openai = ChatOpenAI()

response = model_openai.invoke("Translate the following from French into English: Bonjour ! Il fait très beau à Paris aujourd'hui.")
print(response)
```

This sets up the OpenAI model and sends a prompt to it.

## Runnables and Integrations

A **Runnable** is an object in LangChain that exposes a standard interface for interacting with it. Whether it's a model, an output parser, or a prompt template, they all expose methods like `invoke`, `stream`, and `batch`. This makes it easy to chain them together.

LangChain supports integrations with models from various providers like OpenAI, Groq, and others.

### Example of switching between different models (OpenAI and Groq):

```python
from langchain_groq import ChatGroq
os.environ["GROQ_API_KEY"] = getpass.getpass()

model_groq = ChatGroq(model="llama3-8b-8192")
response_groq = model_groq.invoke("Tell me a story about transformers.")
print(response_groq)
```

## Prompt Templates

Prompt templates allow you to define dynamic prompts, making it easy to reuse them with different inputs. There are two types of prompt templates:

### String Prompt Templates

These templates allow you to create simple text-based prompts with placeholders that are dynamically replaced at runtime.

Example:

```python
from langchain_core.prompts import PromptTemplate

prompt = PromptTemplate.from_template("Write a blog post about {topic}.")
response = prompt.invoke({"topic": "transformers"})
print(response)
```

### Chat Prompt Templates

Chat prompt templates are useful for applications that interact with message-based APIs. These templates allow for more complex interactions, with support for conversation history.

Example:

```python
from langchain_core.prompts import ChatPromptTemplate

prompt_template = ChatPromptTemplate.from_messages([
    ("system", "You are an expert in {topic}."),
    ("user", "What are some key points about {topic}?")
])

input_dict = {
    "topic": "transformers"
}

response = prompt_template.invoke(input_dict)
print(response)
```

## Message Placeholders

Message placeholders allow you to create dynamic conversations by inserting conversation history or other message-based data into your prompts.

### Example of using message placeholders:

```python
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain_core.messages import HumanMessage, AIMessage

prompt_template = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant"),
    MessagesPlaceholder("conversation_history")
])

response = prompt_template.invoke({
    "conversation_history": [
        HumanMessage("Hello!"),
        AIMessage("Hi!"),
    ]
})
print(response)
```

## Chains

A **Chain** in LangChain is a sequence of modular components working together. It’s an efficient way to handle multi-step workflows where each step’s output becomes the next step’s input. You can create chains using the pipe (`|`) operator.

Example:

```python
chain = prompt_template | model_openai | parser
result = chain.invoke({"topic": "transformers"})
print(result)
```

## Exercises and Labs

In this section, you can apply your knowledge by building more complex chains, like generating interview questions based on job descriptions. You'll design prompt templates, initialize models, and combine them using LangChain’s chaining mechanism.

## Conclusion

LangChain is a powerful tool for building complex language model applications by chaining multiple components together. It simplifies integrating different models and tools, whether you're building a conversational agent, a document processor, or any other type of LLM-based application.

By leveraging components like Runnables, Prompt Templates, and Chains, LangChain makes it easier than ever to build scalable and sophisticated workflows.

For further learning, you can refer to the official documentation and explore more advanced features like LangServe, LangSmith, and integration with third-party services.

---

References:

- [LangChain Official Documentation](https://python.langchain.com/)
- [LangChain GitHub Repository](https://github.com/hwchase17/langchain)
- [OpenAI API Documentation](https://platform.openai.com/docs/models)
- [Groq API](https://console.groq.com/)
