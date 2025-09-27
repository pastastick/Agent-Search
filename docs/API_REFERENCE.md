# API Reference

This project exposes a Streamlit chat application defined in `app.py`. The public interface is the app itself; however, the following constructs and patterns constitute the "public API" for extension and usage.

## Streamlit Components

- `st.title(text)`
  - Sets the app title.
  - Example:
    ```python
    st.title("🔎 LangChain - Chat with search")
    ```

- `st.sidebar.text_input(label, type="password")`
  - Collects the Groq API key from user.
  - Returns a string or empty string.

- `st.chat_message(role)` and `.write(content)`
  - Renders chat messages. `role` is one of `"user"`, `"assistant"`.

- `st.chat_input(placeholder=...)`
  - Input box for the next user message.
  - Returns the submitted string when user sends, otherwise `None`.

## Session State

- `st.session_state["messages"]`
  - A list of dicts with keys: `role`, `content`.
  - Used to persist the chat history.
  - Example structure:
    ```python
    {
      "role": "assistant",
      "content": "Hello!"
    }
    ```

## LangChain LLM

- `ChatGroq(groq_api_key: str, model_name: str, streaming: bool)`
  - LLM client used by the agent.
  - Key parameters:
    - `groq_api_key`: required for authentication
    - `model_name`: e.g. `"Llama3-8b-8192"`
    - `streaming`: `True` to stream tokens

## Tools

- `DuckDuckGoSearchRun(name="Search")`
- `ArxivQueryRun(api_wrapper=ArxivAPIWrapper(...))`
- `WikipediaQueryRun(api_wrapper=WikipediaAPIWrapper(...))`

These tool instances are collected as:
```python
tools = [search, arxiv, wiki]
```

## Agent

- `initialize_agent(tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, handling_parsing_errors=True)`
  - Creates a LangChain agent that can call the tools.

- `StreamlitCallbackHandler(container, expand_new_thoughts=False)`
  - Streams intermediate agent thoughts and actions to the UI.

## Chat Flow (Public Behavior)

1. Read or initialize `st.session_state["messages"]`.
2. Display all previous messages with `st.chat_message`.
3. On `prompt := st.chat_input(...)`, append the user message.
4. Create `ChatGroq` and tool list; initialize the agent.
5. Run the agent with the conversation history, render the response, and append it to history.

### Example Usage

```python
from langchain_groq import ChatGroq
from langchain_community.utilities import ArxivAPIWrapper, WikipediaAPIWrapper
from langchain_community.tools import ArxivQueryRun, WikipediaQueryRun, DuckDuckGoSearchRun
from langchain.agents import initialize_agent, AgentType

llm = ChatGroq(groq_api_key=os.environ.get("GROQ_API_KEY"), model_name="Llama3-8b-8192", streaming=True)
search = DuckDuckGoSearchRun(name="Search")
arxiv = ArxivQueryRun(api_wrapper=ArxivAPIWrapper(top_k_results=1, doc_content_chars_max=200))
wiki = WikipediaQueryRun(api_wrapper=WikipediaAPIWrapper(top_k_results=1, doc_content_chars_max=200))

agent = initialize_agent([search, arxiv, wiki], llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, handling_parsing_errors=True)
response = agent.run([{"role": "user", "content": "What is machine learning?"}])
print(response)
```