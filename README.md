## Development and Deployment of a 'Chat with LLM' Application Using the Gradio Blocks Framework

### AIM:
To design and deploy a "Chat with LLM" application by leveraging the Gradio Blocks UI framework to create an interactive interface for seamless user interaction with a large language model.

### PROBLEM STATEMENT:
To develop an interactive web-based chatbot application that accepts user prompts, sends them to a Large Language Model, maintains the conversation history, and displays the generated responses using Gradio.

### DESIGN STEPS:

#### STEP 1:
- Load the required libraries and configure the Hugging Face API.

- Import the required Python libraries, load the Hugging Face API key, and create a client to communicate with the LLM inference endpoint. The given notebook uses the text_generation library and a Falcon LLM endpoint.
  
#### STEP 2:
- Create the chatbot interface using Gradio Blocks.

- Create a gr.Chatbot() to display the conversation, a textbox to accept the user's prompt, and a Submit button. The chat history is maintained and passed to the response function.

#### STEP 3:
- Connect the chatbot with the LLM and deploy the application.

- Format the previous conversation into a prompt, send it to the LLM, receive the generated response, append it to the chat history, and launch the Gradio application. The notebook also adds a system message, temperature control, and streaming response.

### PROGRAM:
```
Name : P PARTHIBAN
Register number : 212223230145
```
```python
import os
import io
import IPython.display
from PIL import Image
import base64 
import requests 
requests.adapters.DEFAULT_TIMEOUT = 60

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']
```
```python
# Helper function
import requests, json
from text_generation import Client

#FalcomLM-instruct endpoint on the text_generation library
client = Client(os.environ['HF_API_FALCOM_BASE'], headers={"Authorization": f"Basic {hf_api_key}"}, timeout=120)
```
```python
import gradio as gr
def format_chat_prompt(message, chat_history, instruction):
    prompt = f"System:{instruction}"
    for turn in chat_history:
        user_message, bot_message = turn
        prompt = f"{prompt}\nUser: {user_message}\nAssistant: {bot_message}"
    prompt = f"{prompt}\nUser: {message}\nAssistant:"
    return prompt
```
```python
def respond(message, chat_history, instruction, temperature=0.7):
    prompt = format_chat_prompt(message, chat_history, instruction)
    chat_history = chat_history + [[message, ""]]
    stream = client.generate_stream(prompt,
                                      max_new_tokens=1024,
                                      stop_sequences=["\nUser:", "<|endoftext|>"],
                                      temperature=temperature)
                                      #stop_sequences to not generate the user answer
    acc_text = ""
    #Streaming the tokens
    for idx, response in enumerate(stream):
            text_token = response.token.text

            if response.details:
                return

            if idx == 0 and text_token.startswith(" "):
                text_token = text_token[1:]

            acc_text += text_token
            last_turn = list(chat_history.pop(-1))
            last_turn[-1] += acc_text
            chat_history = chat_history + [last_turn]
            yield "", chat_history
            acc_text = ""
```
```python
with gr.Blocks() as demo:
    chatbot = gr.Chatbot(height=240) #just to fit the notebook
    msg = gr.Textbox(label="Prompt")
    with gr.Accordion(label="Advanced options",open=False):
        system = gr.Textbox(label="System message", lines=2, value="A conversation between a user and an LLM-based AI assistant. The assistant gives helpful and honest answers.")
        temperature = gr.Slider(label="temperature", minimum=0.1, maximum=1, value=0.7, step=0.1)
    btn = gr.Button("Submit")
    clear = gr.ClearButton(components=[msg, chatbot], value="Clear console")

    btn.click(respond, inputs=[msg, chatbot, system], outputs=[msg, chatbot])
    msg.submit(respond, inputs=[msg, chatbot, system], outputs=[msg, chatbot]) #Press enter to submit

gr.close_all()
demo.queue().launch(share=True, server_port=int(os.environ['PORT4']))    
```
```python
gr.close_all()
```

### OUTPUT:

<img width="917" height="579" alt="image" src="https://github.com/user-attachments/assets/40adfbe9-d46e-450d-bea0-724d6166a528" />

### RESULT:
Thus, a "Chat with LLM" application was successfully developed and deployed using the Gradio Blocks framework, enabling interactive conversation with an LLM while maintaining chat history and providing advanced options such as temperature control and streaming responses.
