# Build a simple LLM application with chat models and prompt templates

This guide will show how to build a simple LLM application with LangChain. This application will translate text from English into another 
language. It serves as an introduction to LangChain, demonstrating how to use language models, create prompt templates and structure inputs for LLMs.

## Requirements

- Jupyter Notebook.
- LangChain.
- OpenAI API key.

## Architecture and components

The application consists of the following components:

- **Language Model:** The core component that performs the translation.
- **Prompt Templates:** Structures the input for the language model by combining user input with predefined instructions.

## Installation

### Langchain

- To install run:
```
pip install langhchain
```

- Create a langsmith account to inspect the logs of the application, then import the following environment variables within the
notebook and insert the langsmith api key when the input is prompted.

```
import getpass
import os

os.environ["LANGSMITH_TRACING"] = "true"
os.environ["LANGSMITH_API_KEY"] = getpass.getpass()
```

## Using Language models

- Now we will use the language model of OpenAI, but first, we need to install it by running the following command, then enter the API key of OpenAI
when the input is prompted

```
pip install -qU "langchain[openai]"
```

```
import getpass
import os

if not os.environ.get("OPENAI_API_KEY"):
  os.environ["OPENAI_API_KEY"] = getpass.getpass("Enter API key for OpenAI: ")

from langchain.chat_models import init_chat_model

model = init_chat_model("gpt-4o-mini", model_provider="openai")
```

- Interact with the model by calling an exposed interface through the .invoke method, this will translate the message "hi" to Italian.

```
from langchain_core.messages import HumanMessage, SystemMessage

messages = [
    SystemMessage("Translate the following from English into Italian"),
    HumanMessage("hi!"),
]

model.invoke(messages)
```

![imagen](https://github.com/user-attachments/assets/391d884c-bfd3-40ce-832b-6dfb8d4428cf)

- You can also interact via strings or OpenAI format

```
model.invoke("Hello")

model.invoke([{"role": "user", "content": "Hello"}])

model.invoke([HumanMessage("Hello")])
```

![imagen](https://github.com/user-attachments/assets/ba468622-7b90-460c-8b70-fe074fb2ca7f)

## Prompt Templates

- The application will take raw user input and turn into messages that will be ready to be passed to the language model.
For this two variables, will be needed:
  - language: The language to translate the text.
  - text: The text to be translated.

```
from langchain_core.prompts import ChatPromptTemplate

system_template = "Translate the following from English into {language}"

prompt_template = ChatPromptTemplate.from_messages(
    [("system", system_template), ("user", "{text}")]
)
```

This prompt will translate the text that has been passed to it from English to the asked language.

- Now we can pass a dictionary to change the parameters, the translation language and the message. 

```
prompt = prompt_template.invoke({"language": "Italian", "text": "hi!"})

prompt
```

![imagen](https://github.com/user-attachments/assets/4faa714c-b5b2-4e94-a307-0cec64b8f2e3)

- Finally, we can see the answer of the prompt by executing the prompt and obtaining the content of the response.

```
response = model.invoke(prompt)
print(response.content)
```

![imagen](https://github.com/user-attachments/assets/26c682a4-b979-4ba0-93c2-7dbe1270a5e7)

- Testing the prompt with a change in the language. 

![imagen](https://github.com/user-attachments/assets/6acf72d9-777d-448b-8dee-f86d8a91c5e2)

## Author

Samuel Rojas - SamuRoj
