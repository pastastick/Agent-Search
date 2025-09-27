# Extending the App

This guide shows how to customize tools, models, and chat behavior.

## Add or Remove Tools

`app.py` defines three tools: DuckDuckGo, arXiv, and Wikipedia. To add more tools:

1. Import a tool implementation from LangChain or write your own `Tool`.
2. Instantiate it in `app.py`.
3. Add it to the `tools` list.

Example (hypothetical):
```python
from langchain_community.tools import YouTubeSearchRun

youtube = YouTubeSearchRun()

# Add to the list
tools = [search, arxiv, wiki, youtube]
```

## Change the Model

Adjust the `ChatGroq` constructor:
```python
llm = ChatGroq(
    groq_api_key=api_key,
    model_name="Llama3-70b-8192",  # choose another available model
    streaming=True,
)
```

## Modify Agent Type

You can try different agent strategies:
```python
from langchain.agents import AgentType

agent = initialize_agent(
    tools,
    llm,
    agent=AgentType.CONVERSATIONAL_REACT_DESCRIPTION,
    handling_parsing_errors=True,
)
```

## Persisting History

Currently history is kept in `st.session_state`. For persistence, integrate a database or local storage:
- Serialize messages to JSON and write to disk
- Use a vector store for retrieval-augmented generation

## Observability

`StreamlitCallbackHandler` renders intermediate steps. For deeper tracing, integrate LangSmith or custom logging.