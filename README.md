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

### OUTPUT:

### RESULT:
Thus, a "Chat with LLM" application was successfully developed and deployed using the Gradio Blocks framework, enabling interactive conversation with an LLM while maintaining chat history and providing advanced options such as temperature control and streaming responses.
