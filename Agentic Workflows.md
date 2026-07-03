# 🧭 Where to Start with Agentic Workflows

A structured learning path for getting into agentic AI development, building on my existing Copilot Studio experience.

**Owner:** Tommy Bui  
**Last Updated:** July 2026  
**Prior Experience:** 🟢 Strong (Copilot Studio governance & pilot rollout at PCUK)

---

## 🎯 Step 0: Anchor Your Mental Model (1–2 hrs)

Before touching code, get crystal clear on **what an agent actually is**:

> **Agent = LLM + Tools + Memory + Planning Loop**

- **LLM** = the brain (reasoning)
- **Tools** = APIs/functions it can call (actions)
- **Memory** = short-term (conversation) + long-term (vector DB)
- **Planning loop** = decides *what to do next* until goal is met

📺 **Watch:** Andrew Ng's *"Agentic Design Patterns"* (free, ~30 min on DeepLearning.AI) — best conceptual overview out there.

> 💡 I already understand this intuitively from Copilot Studio (topics = planning, actions = tools, knowledge sources = memory). Now I'll see it in raw code.

---

## 🪜 Step 1: Build a "Hello World" Agent (Week 1)

Pick **one framework** and build the simplest possible agent — don't shop around yet.

**Recommended: LangChain (Python)** — because:
- Already learning Python ✅
- Massive community, tons of tutorials
- Easy to swap in Azure OpenAI (already have access)

### Mini Project
```
A weather agent that:
1. Takes a city name
2. Calls a weather API (tool)
3. Returns a natural-language summary
```

**Learn:** tool calling, prompt design, and the agent loop — in ~50 lines of code.

---

## 🪜 Step 2: Add Memory + Multi-Step Reasoning (Week 2)

Upgrade to a **ReAct agent** (Reason + Act pattern).

### Mini Project
```
A research assistant that:
1. Takes a question
2. Searches the web (tool)
3. Reads results, decides if it needs more info
4. Loops until confident, then summarises
```

**Learn:** ReAct pattern, chain-of-thought prompting, when agents get stuck (and how to fix it).

---

## 🪜 Step 3: Multi-Agent Workflow (Week 3)

Graduate to **LangGraph** (LangChain's graph-based orchestration).

### Mini Project — plays to my strengths
```
Recreate my Copilot Studio "Comms Handbook Bot" in code:
- Planner agent: understands user question
- Retriever agent: searches handbook (RAG)
- Writer agent: formats answer for comms tone
```

> 🏆 **Killer portfolio piece** — directly compare it to the low-code Copilot Studio version, showing understanding of *both* worlds.

---

## 🪜 Step 4: Production Concerns (Week 4)

The stuff that separates hobbyists from engineers:

- **Observability** — LangSmith or Langfuse (trace every LLM call)
- **Evaluation** — how do you know your agent is *good*?
- **Guardrails** — prevent hallucinations, prompt injection
- **Cost tracking** — token usage, model routing (cheap vs. smart)

> 💡 I already think this way from Copilot Studio governance (credit tracking, publishing controls) — now apply it to code.

---

## 🛠️ Recommended Stack

| Layer | Tool | Why |
|-------|------|-----|
| Language | **Python** | Already learning it |
| Framework | **LangChain + LangGraph** | Biggest ecosystem, best docs |
| LLM | **Azure OpenAI** (GPT-4o) | Already have access via work |
| Vector DB | **Chroma** (local) → **Azure AI Search** | Start simple, scale to Azure |
| Observability | **LangSmith** (free tier) | See exactly what the agent does |
| Deployment | **Vercel** or **Azure Container Apps** | Ties into cloud roadmap |

---

## 📚 Best Free Resources (in order)

1. 🎥 **DeepLearning.AI — "AI Agents in LangGraph"** (short course, free) ← *start here*
2. 📖 **LangChain docs → "Agents" section** (official quickstart)
3. 🎥 **Harrison Chase (LangChain CEO) YouTube talks** — great architectural insight
4. 📖 **Anthropic's "Building Effective Agents"** blog post — gold standard on patterns
5. 🎥 **Microsoft Learn — "Build AI Agents with Semantic Kernel"** ← useful since I'm in the MS ecosystem

---

## ⚡ First Concrete Action

```bash
# 1. Spin up a new folder in the repo
mkdir agentic-workflows && cd agentic-workflows

# 2. Set up env
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows
pip install langchain langchain-openai python-dotenv

# 3. Create hello_agent.py and get GPT-4o to answer one question with one tool
```

That's it — I've started. 🚀

---

## 💡 Framing for Day Job

> *"I'm learning how agents work under the hood so I can make better governance decisions and know when to build code-first vs. low-code."*

This makes the learning **directly relevant** to my PCUK role — great for performance reviews, chats with James/Lucy, and future progression.

---

## ✅ Progress Checklist

- [ ] Step 0 — Watch Andrew Ng's Agentic Design Patterns
- [ ] Step 1 — Build "Hello World" weather agent (LangChain)
- [ ] Step 2 — Build ReAct research assistant
- [ ] Step 3 — Recreate Comms Handbook Bot in LangGraph
- [ ] Step 4 — Add observability, evals, guardrails, cost tracking

---

## 🔗 Related Pages
- ./README.md
- ./FOUNDATIONS.md
- ./LOG.md *(coming soon)*
