# Usage

## Running the App

1. Ensure dependencies are installed:
   ```bash
   pip install -r requirements.txt
   ```
2. Start the app:
   ```bash
   streamlit run app.py
   ```
3. Enter your Groq API key in the sidebar when prompted, or export `GROQ_API_KEY`.

## Interacting

- Type a question in the chat input. The agent may call search tools.
- Intermediate thoughts and actions appear during streaming.
- The final answer is added as an assistant message.

## Example Prompts

- "Summarize the latest research on transformers from arXiv."
- "Find the Wikipedia page for Ada Lovelace and summarize it."
- "Search for tutorials on reinforcement learning and give me links."

## Environment Variables

- `GROQ_API_KEY`: API key for Groq LLM. If omitted, you can paste it in the UI.

## Troubleshooting

- If you see auth errors: verify `GROQ_API_KEY` and network connectivity.
- If tools return empty results: try broader queries; ensure the services are reachable.
- If Streamlit cannot start: check Python version and that `streamlit` is installed.