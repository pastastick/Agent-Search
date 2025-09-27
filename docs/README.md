# LangChain Chat with Search (Streamlit)

A simple Streamlit app that lets you chat with an agent capable of searching the web and academic sources (DuckDuckGo, Wikipedia, arXiv) using LangChain tools and a Groq LLM backend.

## Features

- Chat UI powered by Streamlit `st.chat_message` and `st.chat_input`
- Tools: DuckDuckGo search, Wikipedia, arXiv
- Agent: ZERO_SHOT_REACT_DESCRIPTION via LangChain
- Streaming responses via `StreamlitCallbackHandler`

## Quickstart

### 1) Prerequisites
- Python 3.10+
- A Groq API key

### 2) Install
```bash
pip install -r requirements.txt
```

### 3) Set environment
- You can pass the API key in the UI, or set env var:
```bash
export GROQ_API_KEY=your_key_here
```

### 4) Run the app
```bash
streamlit run app.py
```

Open the printed local URL in your browser.

## Documentation
- See `docs/USAGE.md` for usage tips
- See `docs/API_REFERENCE.md` for public APIs/functions/components
- See `docs/EXTENDING.md` for how to add tools or change models