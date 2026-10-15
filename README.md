# AI Trip Planner

An autonomous AI trip planner that builds dynamic, multi-day itineraries using agentic workflows and live travel data. The project combines LangGraph-based orchestration with external tools for weather, activities, attractions, transportation, expenses, and currency conversion to generate travel plans that are practical and detailed.

## Overview

AI Trip Planner helps users ask for destination-based travel suggestions in natural language and turns them into structured itineraries with:

- Day-by-day trip plans
- Popular and offbeat places to visit
- Hotel and food recommendations
- Activity and transport suggestions
- Budget and expense estimates
- Real-time weather updates
- Currency conversion support

The app uses a FastAPI backend and a Streamlit frontend, with a LangGraph agent coordinating specialized tools.

## Features

- AI-powered multi-agent travel planning workflow
- Real-time place discovery via Google Places and Tavily fallback
- Weather insights for destinations
- Expense calculation and travel budget breakdown
- Currency conversion support
- End-to-end travel planning through a backend API and web UI
- Graph-based orchestration for tool calling and reasoning

## Tech Stack

- Python 3.10+
- FastAPI
- Streamlit
- LangGraph
- LangChain
- Groq / OpenAI compatible model providers
- Google Places API
- Tavily API
- OpenWeatherMap
- External travel and exchange-rate APIs

## Project Structure

```text
AI-Trip-Planner/
├── agent/
│   ├── __init__.py
│   └── agentic_workflow.py          # LangGraph workflow and tool orchestration
├── config/
│   ├── __init__.py
│   └── config.yaml                 # LLM provider configuration
├── exception/
├── logger/
├── notebook/
├── prompt_library/
│   └── prompt.py                   # System prompt for the travel agent
├── tools/
│   ├── arthamatic_op_tool.py       # Arithmetic helpers
│   ├── currency_conversion_tool.py # Currency conversion utility
│   ├── expense_calculator_tool.py  # Budget estimation logic
│   ├── place_search_tool.py        # Attraction / restaurant / activity tools
│   ├── weather_info_tool.py        # Weather data tool
│   └── __init__.py
├── utils/
├── .env.name                       # Environment variable template
├── main.py                         # FastAPI backend
├── streamlit_app.py                # Streamlit frontend
├── my_graph.png                    # Example generated workflow graph
├── requirements.txt
├── pyproject.toml
├── setup.py
├── README.md
└── uv.lock
```

## Architecture

The app follows a tool-using agent pattern:

1. User enters a travel query via the Streamlit app.
2. The app sends the request to the FastAPI backend.
3. The backend creates a LangGraph workflow using `GraphBuilder`.
4. The agent invokes travel-related tools (weather, places, currency, expenses).
5. The model synthesizes the final response into a detailed trip plan.

The workflow logic is defined in `agent/agentic_workflow.py`, and the main API entry point is `main.py`.

## Prerequisites

Before running the project, make sure you have:

- Python 3.10+
- pip or uv
- API keys for the services you want to use

Required environment variables are listed in `.env.name`:

```env
OPENAI_API_KEY=""
GROQ_API_KEY=""
GOOGLE_API_KEY=""
GPLACES_API_KEY=""
FOURSQUARE_API_KEY=""
TAVILAY_API_KEY=""
OPENWEATHERMAP_API_KEY=""
EXCHANGE_RATE_API_KEY=""
```

Create a `.env` file in the project root and populate the keys you plan to use.

## Installation

### Option 1: Using pip

```bash
git clone https://github.com/CVinit13/AI-Trip-Planner.git
cd AI-Trip-Planner
python -m venv .venv
source .venv/bin/activate   # On Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### Option 2: Using uv

```bash
git clone https://github.com/CVinit13/AI-Trip-Planner.git
cd AI-Trip-Planner
uv venv .venv
source .venv/bin/activate
uv pip install -r requirements.txt
```

## Configuration

The default LLM settings are in `config/config.yaml`:

```yaml
llm:
  openai:
    provider: "openai"
    model_name: "o4-mini"
  groq:
    provider: "groq"
    model_name: "deepseek-r1-distill-llama-70b"
```

You can switch between providers by modifying this file or by passing a different model provider in the backend code.

## Running the Application

### Start the backend

```bash
uvicorn main:app --reload --port 8000
```

### Start the frontend

```bash
streamlit run streamlit_app.py
```

Then open the Streamlit UI in your browser and ask something like:

- Plan a 5-day trip to Goa
- Suggest a 3-day itinerary for Kyoto
- Build a budget trip for Bali under $500
- Create an offbeat tour plan for Jaipur

## API Example

Send a query to the API:

```bash
curl -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{"question": "Plan a 4-day trip to Bali with budget details."}'
```

Example response:

```json
{
  "answer": "A detailed travel plan with itinerary, costs, weather, attractions, eateries, and transport options..."
}
```

## Notes

- This project is designed for local experimentation and development.
- Some tools depend on external vendor APIs, so you will need valid keys configured in your environment.
- The app is best suited for conversational trip planning rather than production-grade deployment without additional hardening.

## Future Improvements

- Add authentication and user accounts
- Support saved trip plans
- Add map integration
- Improve itinerary caching and personalization
- Add deployment configuration for production environments

## License

This project does not currently include an explicit license file. Please check the repository state before using it in a production or commercial context.

## Contributing

Contributions are welcome. If you want to improve the project, open a pull request with a clear summary of the enhancement and test coverage where applicable.

## Author

Vinit Chaudhari

