# Build a simple LLM application with chat models and prompt templates

This guide will show how to build a simple LLM application with LangChain. This application will translate text from English into another 
language.

## Requirements

- Jupyter Notebook
- LangChain

### Installation Langchain

- To install run:
```
pip install langhchain
```

- Create a langsmith account to inspect the logs of the application, then import the following environment variables within the
notebook.

```
import getpass
import os

os.environ["LANGSMITH_TRACING"] = "true"
os.environ["LANGSMITH_API_KEY"] = getpass.getpass()
```

## Using Language models

- Now we will use the language model of openAI, but first, we need to install it by running the following command, enter the API key of OpenAI
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

- Then, interact with the model by calling an exposed interface through the .invoke method, this will translate the message "hi" into Italian.

```
from langchain_core.messages import HumanMessage, SystemMessage

messages = [
    SystemMessage("Translate the following from English into Italian"),
    HumanMessage("hi!"),
]

model.invoke(messages)
```

![imagen](https://github.com/user-attachments/assets/391d884c-bfd3-40ce-832b-6dfb8d4428cf)

- Also, you can interact through the OpenAI format

```
model.invoke("Hello")

model.invoke([{"role": "user", "content": "Hello"}])

model.invoke([HumanMessage("Hello")])
```

![imagen](https://github.com/user-attachments/assets/ba468622-7b90-460c-8b70-fe074fb2ca7f)

## Prompt Templates

- The application will take raw user input and turn into messages that will be ready to be passed to the language model.
For this two variables, will be needed:
  - language: The language to translate the text
  - text: The text to be translated

```
from langchain_core.prompts import ChatPromptTemplate

system_template = "Translate the following from English into {language}"

prompt_template = ChatPromptTemplate.from_messages(
    [("system", system_template), ("user", "{text}")]
)
```

This prompt will ask to translate from English to the asked language and the text that has been passed to it.

- Now we can pass a dictionary to change the parameters of the message and the language

```
prompt = prompt_template.invoke({"language": "Italian", "text": "hi!"})

prompt
```

![imagen](https://github.com/user-attachments/assets/4faa714c-b5b2-4e94-a307-0cec64b8f2e3)


- Finally, we can see the answer of the prompt by invoking the method and obtaining the content of the answer

```
response = model.invoke(prompt)
print(response.content)
```

![imagen](https://github.com/user-attachments/assets/26c682a4-b979-4ba0-93c2-7dbe1270a5e7)

- Another language

![imagen](https://github.com/user-attachments/assets/6acf72d9-777d-448b-8dee-f86d8a91c5e2)

## Author

Samuel Rojas - SamuRoj
