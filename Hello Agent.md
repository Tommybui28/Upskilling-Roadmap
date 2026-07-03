# 🤖 Step 1 — Hello World Agent

A simple weather agent built with **LangChain + Azure OpenAI** to learn the core agent loop:

> User question → LLM decides → Tool call → LLM formats answer → Response

**Owner:** Tommy Bui  
**Last Updated:** July 2026  
**Roadmap Step:** 1 of 4 (see ./Agentic%20Workflows.md)

---

## 📁 Folder Structure

````
agentic-workflows/
 ├── hello_agent.py
 ├── requirements.txt
 ├── .env              ← not committed
 └── .gitignore
````

---

## 🐍 `hello_agent.py`

````python
"""
hello_agent.py
---------------
Step 1: Hello World Agent — a weather agent using LangChain + Azure OpenAI.

Goal: Understand the core agent loop:
    User question -> LLM decides -> Tool call -> LLM formats answer -> Response

Author: Tommy Bui
Date:   July 2026
"""

import os
import requests
from dotenv import load_dotenv

from langchain_openai import AzureChatOpenAI
from langchain.agents import AgentExecutor, create_tool_calling_agent
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.tools import tool

# ─────────────────────────────────────────────────────────────
# 1. Load environment variables from .env
# ─────────────────────────────────────────────────────────────
load_dotenv()

# Expected in .env:
#   AZURE_OPENAI_API_KEY=...
#   AZURE_OPENAI_ENDPOINT=https://<your-resource>.openai.azure.com/
#   AZURE_OPENAI_API_VERSION=2024-08-01-preview
#   AZURE_OPENAI_DEPLOYMENT=gpt-4o
#   OPENWEATHER_API_KEY=...    # free tier: https://openweathermap.org/api

# ─────────────────────────────────────────────────────────────
# 2. Define a TOOL the agent can call
# ─────────────────────────────────────────────────────────────
@tool
def get_weather(city: str) -> str:
    """
    Get the current weather for a given city.
    Returns a short description including temperature (°C) and conditions.
    """
    api_key = os.getenv("OPENWEATHER_API_KEY")
    url = "https://api.openweathermap.org/data/2.5/weather"
    params = {"q": city, "appid": api_key, "units": "metric"}

    try:
        response = requests.get(url, params=params, timeout=10)
        response.raise_for_status()
        data = response.json()

        temp = data["main"]["temp"]
        conditions = data["weather"][0]["description"]
        return f"{city}: {temp}°C, {conditions}."
    except requests.exceptions.RequestException as e:
        return f"Error fetching weather for {city}: {e}"


# ─────────────────────────────────────────────────────────────
# 3. Initialise the LLM (Azure OpenAI GPT-4o)
# ─────────────────────────────────────────────────────────────
llm = AzureChatOpenAI(
    azure_deployment=os.getenv("AZURE_OPENAI_DEPLOYMENT"),
    api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
    temperature=0,  # deterministic — good for tool use
)

# ─────────────────────────────────────────────────────────────
# 4. Build the AGENT prompt
# ─────────────────────────────────────────────────────────────
prompt = ChatPromptTemplate.from_messages([
    ("system",
     "You are a helpful weather assistant. "
     "Use the `get_weather` tool whenever the user asks about weather. "
     "Always respond in a friendly, natural sentence."),
    ("human", "{input}"),
    ("placeholder", "{agent_scratchpad}"),  # required for tool-calling agents
])

# ─────────────────────────────────────────────────────────────
# 5. Wire up the agent + executor
# ─────────────────────────────────────────────────────────────
tools = [get_weather]
agent = create_tool_calling_agent(llm, tools, prompt)
agent_executor = AgentExecutor(
    agent=agent,
    tools=tools,
    verbose=True,   # see the agent's reasoning in the console
)

# ─────────────────────────────────────────────────────────────
# 6. Run it!
# ─────────────────────────────────────────────────────────────
if __name__ == "__main__":
    print("🤖 Weather Agent ready. Ask me about the weather (type 'quit' to exit).\n")

    while True:
        user_input = input("You: ").strip()
        if user_input.lower() in {"quit", "exit", "q"}:
            print("👋 Bye!")
            break

        result = agent_executor.invoke({"input": user_input})
        print(f"\n🤖 Agent: {result['output']}\n")
````

---

## 🗝️ `.env` (never commit — add to `.gitignore`)

````
AZURE_OPENAI_API_KEY=your-key-here
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com/
AZURE_OPENAI_API_VERSION=2024-08-01-preview
AZURE_OPENAI_DEPLOYMENT=gpt-4o
OPENWEATHER_API_KEY=your-openweather-key
````

---

## 📦 `requirements.txt`

````
langchain>=0.3.0
langchain-openai>=0.2.0
python-dotenv>=1.0.0
requests>=2.31.0
````

---

## 🚫 `.gitignore`

````
.venv/
.env
__pycache__/
*.pyc
````

---

## ▶️ How to Run

*```*ash
# From the agentic-workflows f*lder
pip install -r requirements.t*t
python hello_agent.py
````

### *ry prompts like:*- "What's the weather in London?"
* "Is it warmer in Paris or Tokyo r*ght now?" — agent should call the *ool **twice**
- "T*ll me a joke" — agent should **not** call the tool (tests judgement)
*---

## 🧠 What to Notice When It *uns

With `verbose=True*, you'll see the agent's inner mon*logue:

````
> Entering new AgentE*ecutor chain...
Invoking: `get_wea*her` with `*'city': 'London'}`
London: 18°C, l*ght rain.
The weather in London is*currently 18°C with light rain. ☔
* Finished chain.
````*
> 🎯 **That's the agent loop in a*tion** — LLM decided to call a too*, got the result, then formatted a*natural response.*Everything else in agentic AI is a*variation of this pattern.

---

#* 🚀 Stretch Goals

- [ ] Add a **s*cond tool** (e.g. `get_forecast`*for 5-day outlook)
- [ ] Add **con*ersation memory** so it remembers *revious cities
- [ ] Swap OpenWeat*er for a***UK-specific API** (Met Office Da*aHub)
- [ ] Log every run to a fil* for later review (prep for Step 4*observability)

---

## ✅ Completi*n Checklist

- [ ] Azure OpenAI ke*s added to `.env`
- [ ] OpenWeathe* API key added to `.env`
- [ ] `pi* install -r requirements.txt` runs*clean
- [ ] Agent answers a weathe* question successfully
- [ ] Agent*correctly ignores non-weather ques*ions
- [ ] At least one stretch go*l completed

---

## 🔗 Related Pa*es
- ./README.md — main roadmap
- */Agentic%20Workflows.md — full 4-w*ek agentic workflow plan
- ./Found*tion%20Skills.md — prior experienc* mapping
