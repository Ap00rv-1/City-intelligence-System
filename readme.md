# City Intelligence Agent

A command-line AI assistant that answers questions about any city using **live data**. It is built with LangChain's `create_agent` and uses a Mistral AI model to decide when to call external tools for current weather and the latest local news.

## How it works

The LLM is given two tools. For each question, it chooses which tool(s) to call, reads the results, and writes a final answer.

| Tool | Purpose | Data source |
|------|---------|-------------|
| `get_weather(city)` | Current weather: conditions and temperature in °C | [OpenWeatherMap API](https://openweathermap.org/api) |
| `get_news(city)` | Latest news: top 3 results with title, link and a short snippet | [Tavily Search API](https://tavily.com/) |

```
User question
     |
     v
create_agent (Mistral LLM + system prompt)
     |
     +--> decides which tool(s) to call
              |-- get_weather --> OpenWeatherMap
              |-- get_news    --> Tavily
     |
     v
Final answer in the terminal
```

## Tech stack

- Python
- LangChain (`create_agent`, `@tool`)
- Mistral AI (`mistral-small-2506`) via `langchain-mistralai`
- OpenWeatherMap API, Tavily API
- `python-dotenv` for API key management, `rich` for terminal output

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/Ap00rv-1/City-intelligence-System
```

### 2. Install dependencies

```bash
pip install langchain langchain-mistralai python-dotenv requests tavily-python rich
```

### 3. Add your API keys

Create a `.env` file in the project root:

```env
MISTRAL_API_KEY=your_mistral_key
OPENWEATHER_API_KEY=your_openweather_key
TAVILY_API_KEY=your_tavily_key
```

Keys can be obtained from the Mistral AI console, OpenWeatherMap and Tavily. Never commit your `.env` file; add it to `.gitignore`.

### 4. Run the agent

```bash
python Agents_updated.py
```

## Usage

```
City Agent | Type exit to leave
You : What's the weather in Bhopal?
You : Any latest news from Indore?
You : Weather and news for Pune
You : exit
```

The agent can call one tool or both, depending on what you ask. A question like "Weather and news for Pune" triggers both tools in one go.

## Project structure

```
.
├── <your_script_name>.py   # agent, tools and chat loop
├── .env                    # API keys (not committed)
└── README.md
```

## Limitations and ideas for improvement

- **India-only weather:** the weather tool appends `,IN` to the query, so it currently searches for cities in India. Removing the country code or adding a `country` parameter would make it global.
- **No conversation memory:** each message is sent to the agent on its own, so follow-up questions like "and tomorrow?" will not remember the earlier city. Passing the message history on each call, or adding a checkpointer, would fix this.
- **Current weather only:** no forecast support yet.
- **Basic error handling:** the tools return an error message on a failed weather lookup but do not retry or handle network timeouts.
- **Possible extensions:** forecasts, more tools (air quality, events, traffic), a web UI with Streamlit, and an approval step before tools run.

## Author

**Apoorv Singh**, IIT Indore
GitHub: [Ap00rv-1](https://github.com/Ap00rv-1)