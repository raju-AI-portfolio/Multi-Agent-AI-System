# Multi-Agent-AI-System
This project demonstrates the design and implementation of a "Multi-Agent AI System" using n8n workflows to automate real-world tasks.

---

## 📌 Project Overview

This project demonstrates the design and implementation of a **Multi-Agent AI System** using **n8n workflows** to automate real-world tasks.

The system acts as an intelligent personal assistant capable of:

- 🌤 Retrieving real-time weather forecasts  
- 💱 Fetching live Forex exchange rates  
- 📅 Managing Google Calendar events  
- 🗂 Handling general administrative tasks  
- 🧠 Orchestrating multiple AI agents through a central Manager Agent  
- 📝 Generating a consolidated, user-friendly response  

---

## 🎯 Objective

To build a multi-agent workflow in n8n that:

- Integrates multiple services (Weather API, Forex API, Google Calendar)
- Uses AI agents for task delegation
- Automates compound queries
- Demonstrates workflow orchestration and agent collaboration
- Logs and consolidates outputs using LLM summarization

---

## 🏗 System Architecture

### 🔐 1. Gatekeeper Agent
- Filters unsafe queries
- Classifies messages as `SAFE` or `UNSAFE`

### 🎛 2. Manager Agent
- Understands user intent
- Routes tasks to:
  - Weather Agent
  - Forex Agent
  - Google Calendar Agent
  - Secretary Agent

### 🌦 3. Weather Agent
- Uses OpenWeatherMap API
- Retrieves real-time weather forecast

### 💱 4. Forex Agent
- Uses ExchangeRate API
- Fetches currency exchange rates
- Supports:
  - Real-time conversion
  - Market insight
  - Percentage change

### 📅 5. Google Calendar Agent
- Creates events
- Retrieves events
- Deletes events
- Postpones meetings

### 🗂 6. Secretary Agent
- Handles general queries
- Notes, reminders, miscellaneous tasks

### 🧠 7. LLM Consolidator
- Merges outputs from all agents
- Generates a structured final response

---

## 🔄 Workflow Design (n8n)

### Step 1: Chat Trigger
Captures user input from chat interface.

### Step 2: Gatekeeper Agent
Classifies input as SAFE or UNSAFE.

### Step 3: If Node
- SAFE → Continue workflow
- UNSAFE → Return warning message

### Step 4: Manager Agent
Uses Structured Output Parser:

```json
{
  "assigned_agents": [
    "Google Calendar Agent",
    "Forex Agent",
    "Weather Agent",
    "Secretary Agent"
  ]
}
```

### Step 5: Split Out Node
Splits assigned agents for routing.

### Step 6: Switch Node
Routes queries to appropriate agent.

### Step 7: Specialist Agents
- Weather Agent → OpenWeatherMap Tool
- Forex Agent → HTTP Request Tool
- Google Calendar Agent → Calendar integration
- Secretary Agent → LLM response

### Step 8: Merge Node
Mode: Append  
Inputs: Weather, Forex, Calendar, Secretary  

### Step 9: Python Code Node

```python
# Convert all input items to Python-native dicts
input_items = _input.all().to_py()

# Extract outputs
texts = [str(item["json"]["output"]) for item in input_items]

# Concatenate outputs
full_text = "\n\n".join(texts)

# Return consolidated output
return [{"json": {"fullText": full_text}}]
```

### Step 10: JSON to String Node
Converts merged JSON into string format.

### Step 11: Summary Agent
Summarizes:

```
User Question:
{{ chatInput }}

Agent Outputs:
{{ fullText }}

Generate a well-structured final response.
```

---

## 🧪 Example Test Queries

- Schedule a meeting tomorrow at 10 AM.
- What meetings are scheduled for tomorrow?
- Postpone tomorrow’s meetings to next day.
- What is the current USD to INR rate?
- How much INR will I get for 500 USD?
- Will it rain in Bangalore today?
- Which was built first - Taj Mahal or Eiffel Tower?

---

## 🛠 Technologies Used

- **n8n**
- **OpenAI / Mistral LLM**
- **OpenWeatherMap API**
- **ExchangeRate API**
- **Google Calendar API**
- **Python (n8n Code Node)**

---

## 📊 Business Use Case

Designed for professionals like:
- International Sales Executives
- Consultants
- Business Managers

Example query:

> “Schedule a meeting with my London client tomorrow and tell me the expected weather and GBP-INR rate.”

System automatically:
- Schedules meeting
- Fetches weather forecast
- Retrieves forex rate
- Returns a unified response

---

## 🚀 How to Run

1. Launch n8n (Docker or GitHub Codespaces)
2. Create new workflow
3. Follow the step-by-step node configuration
4. Add API keys:
   - OpenWeatherMap
   - ExchangeRate API
   - Google Calendar credentials
5. Activate workflow
6. Test via Chat interface

---

## 📌 Key Learnings

- Multi-agent orchestration
- Intent classification
- Structured Output Parsing
- Workflow automation
- API integration
- LLM-based response consolidation

---

## 📈 Outcome

This project demonstrates:

✅ Autonomous workflow orchestration  
✅ Multi-agent collaboration  
✅ Real-time API integration  
✅ Intelligent personal assistant system  
✅ No-code + AI hybrid automation  

---

## 👨‍💻 Author

Raju Kumar  

---

