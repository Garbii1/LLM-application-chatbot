# 🤖 Simple Chatbot with Python, Hugging Face, and Flask

Welcome to your hands-on project for building a chatbot using open-source Large Language Models (LLMs) from Hugging Face! This guide will walk you through both a command-line chatbot and a web-based chatbot application.

---

## 🚀 Project Overview

- **Part 1:** Command-Line Chatbot (Python)
- **Part 2:** Web Application Integration (Flask + JavaScript)

> _Based on the "Create Simple Chatbot with Open Source LLMs using Python and Hugging Face" lab from [cognitiveclass.ai](https://cognitiveclass.ai)._ 

---

## 🎯 Learning Outcomes

- Understand chatbot components: Transformer, LLM, Tokenizer
- Use an open-source LLM from Hugging Face Hub
- Program a simple chatbot in Python
- Set up a Flask backend server
- Integrate the chatbot into a web application

---

# 🟦 Part 1: Building a Command-Line Chatbot

### 1.1. Prerequisites
- Python 3
- `pip` and `virtualenv`

### 1.2. Setup

```bash
pip3 install virtualenv
virtualenv my_env
source my_env/bin/activate
```

Install required libraries:

```bash
python3 -m pip install transformers==4.30.2 torch
```

### 1.3. Code: `chatbot.py`

```python
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

model_name = "facebook/blenderbot-400M-distill"
model = AutoModelForSeq2SeqLM.from_pretrained(model_name)
tokenizer = AutoTokenizer.from_pretrained(model_name)

conversation_history = []

print("Chatbot is ready. Type 'exit' to end the conversation.")

while True:
    history_string = "\n".join(conversation_history)
    input_text = input("> ")
    if input_text.lower() == "exit":
        print("Goodbye!")
        break
    inputs = tokenizer.encode_plus(history_string, input_text, return_tensors="pt")
    outputs = model.generate(**inputs)
    response = tokenizer.decode(outputs[0], skip_special_tokens=True).strip()
    print(response)
    conversation_history.append(input_text)
    conversation_history.append(response)
    if len(conversation_history) > 20:
        conversation_history = conversation_history[-20:]
```

### 1.4. Run the Chatbot

```bash
python3 chatbot.py
```

Type `exit` to end the chat.

---

# 🟩 Part 2: Integrating the Chatbot into a Web Application

### 2.1. Prerequisites
- Complete Part 1 setup
- `git` for cloning the front-end repository

### 2.2. Setup

Install Flask libraries:

```bash
python3 -m pip install flask flask_cors
```

> _Note: The lab specifies `transformers==4.38.2` and `torch==2.2.1` for the web app._

Clone the front-end template:

```bash
git clone https://github.com/ibm-developer-skills-network/LLM_application_chatbot
```

This creates a `LLM_application_chatbot` directory with `static` and `templates` subdirectories.

### 2.3. Code: `app.py`

```python
from flask import Flask, request, render_template
from flask_cors import CORS
import json
from transformers import AutoModelForSeq2SeqLM, AutoTokenizer

app = Flask(__name__)
CORS(app)

model_name = "facebook/blenderbot-400M-distill"
model = AutoModelForSeq2SeqLM.from_pretrained(model_name)
tokenizer = AutoTokenizer.from_pretrained(model_name)
conversation_history = []

@app.route('/', methods=['GET'])
def home():
    return render_template('index.html')

@app.route('/chatbot', methods=['POST'])
def handle_prompt():
    global conversation_history
    data = request.get_data(as_text=True)
    data = json.loads(data)
    input_text = data['prompt']
    history = "\n".join(conversation_history)
    inputs = tokenizer.encode_plus(history, input_text, return_tensors="pt")
    outputs = model.generate(**inputs, max_length=60)
    response = tokenizer.decode(outputs[0], skip_special_tokens=True).strip()
    conversation_history.append(input_text)
    conversation_history.append(response)
    if len(conversation_history) > 20:
        conversation_history = conversation_history[-20:]
    return response

if __name__ == '__main__':
    app.run(host="0.0.0.0", port=5000)
```

### 2.4. Configure the Front-End

Open `LLM_application_chatbot/static/script.js` and set the API endpoint URL:

```javascript
const CHATBOT_ENDPOINT = 'http://127.0.0.1:5000/chatbot';
```

> _If using a cloud IDE, use the public URL provided by your environment._

### 2.5. Running the Web Application

Navigate to the project directory and start the Flask server:

```bash
cd LLM_application_chatbot/
python3 app.py
```

Open your browser and go to [http://127.0.0.1:5000](http://127.0.0.1:5000) to chat with your bot!

---

## 🧪 Testing the Backend (Optional)

You can test the `/chatbot` endpoint directly with `curl`:

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Hello, how are you today?"}' \
  http://127.0.0.1:5000/chatbot
```

---

## 📚 Resources
- [Hugging Face Transformers Documentation](https://huggingface.co/docs/transformers/index)
- [Flask Documentation](https://flask.palletsprojects.com/)
- [Original Lab on Cognitive Class](https://cognitiveclass.ai/)

---

## 📝 License
This project is for educational purposes. See the original lab for licensing details.