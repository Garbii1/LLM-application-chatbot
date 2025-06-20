# 🤖 My Simple Chatbot with Python, Hugging Face, and Flask

This is a personal project where I explored building a chatbot using open-source Large Language Models (LLMs) from Hugging Face. My goal was to learn how to create both a command-line chatbot and a web-based chatbot application, and to document my process and key takeaways here.

---

## 🚀 Project Overview

- Command-Line Chatbot (Python)
- Web Application Integration (Flask + JavaScript)

---

## 🎯 What I Learned

- The components of a chatbot: Transformer, LLM, Tokenizer
- How to use an open-source LLM from Hugging Face Hub
- Programming a simple chatbot in Python
- Setting up a Flask backend server
- Integrating the chatbot into a web application

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

### 1.3. Run the Chatbot

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

### 2.3. Configure the Front-End

Open `LLM_application_chatbot/static/script.js` and set the API endpoint URL:

```javascript
const CHATBOT_ENDPOINT = 'http://127.0.0.1:5000/chatbot';
```

> _If using a cloud IDE, use the public URL provided by your environment._

### 2.4. Running the Web Application

Navigate to the project directory and start the Flask server:

```bash
cd LLM_application_chatbot/
python3 app.py
```

Open your browser and go to [http://127.0.0.1:5000](http://127.0.0.1:5000) to chat with your bot!

---

## 🧪 Testing the Backend (Optional)

You can test the `/chatbot` endpoint directly with `curl` as described in the setup instructions.

---

## 📚 Resources

- [Hugging Face Transformers Documentation](https://huggingface.co/docs/transformers/index)
- [Flask Documentation](https://flask.palletsprojects.com/)

---