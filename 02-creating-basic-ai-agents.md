# Creating Basic AI Agents

This guide walks you through building simple AI agents from scratch, illustrating core concepts like perception, decision-making, and action. We cover both a reactive agent and a deliberative (task-planning) agent using Python.

---

## 1. Prerequisites

* **Python 3.8+**
* **Libraries:**

  * `openai` (for LLM-based agents)
  * `requests` (for API calls)
  * `networkx` (optional, for planning graphs)
* **API Keys:** An OpenAI API key if you plan to use an LLM for decision-making.

Install dependencies:

```bash
pip install openai requests networkx
```

---

## 2. Reactive Agent Example

A *reactive agent* responds directly to input without maintaining an internal state. Below is a rule-based chatbot that echoes keywords.

```python
# reactive_agent.py
import re

def reactive_agent(user_input: str) -> str:
    # Simple keyword-based responses
    if re.search(r"\bhello\b", user_input, re.I):
        return "Hi there! How can I help you today?"
    if re.search(r"\bweather\b", user_input, re.I):
        return "I don't have live weather data, but it's always sunny in the cloud!"
    return "Sorry, I didn't understand that."

if __name__ == '__main__':
    while True:
        inp = input("You: ")
        print("Agent:", reactive_agent(inp))
```

**How it works:**

1. **Perception:** Takes raw text input.
2. **Decision:** Matches against regex rules.
3. **Action:** Returns a hardcoded response.

---

## 3. Deliberative Task-Planning Agent Example

A *deliberative agent* plans a sequence of steps to achieve a goal. Here, we use a simple graph search to find a path.

```python
# task_planning_agent.py
import networkx as nx

class TaskPlanningAgent:
    def __init__(self, graph: nx.Graph):
        self.graph = graph

    def plan(self, start, goal):
        try:
            path = nx.shortest_path(self.graph, source=start, target=goal)
            return path
        except nx.NetworkXNoPath:
            return []

    def act(self, path):
        for step in path:
            print(f"Moving to {step}...")

if __name__ == '__main__':
    # Define a simple map
    G = nx.Graph()
    edges = [('A','B'), ('B','C'), ('C','D'), ('A','D'), ('B','D')]
    G.add_edges_from(edges)

    agent = TaskPlanningAgent(G)
    goal = input("Enter goal node (A-D): ")
    path = agent.plan('A', goal)
    if path:
        agent.act(path)
    else:
        print("No path found.")
```

**How it works:**

1. **Perception:** Static graph map.
2. **Decision:** Uses Dijkstra’s algorithm for shortest path.
3. **Action:** Iterates moves along the path.

---

## 4. LLM-Powered Agent Example

Combine an LLM for reasoning within a reactive loop.

```python
# llm_agent.py
import openai, os

openai.api_key = os.getenv('OPENAI_API_KEY')

def llm_agent(prompt: str) -> str:
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message.content.strip()

if __name__ == '__main__':
    while True:
        user = input("You: ")
        reply = llm_agent(f"You are a helpful assistant. {user}")
        print("Agent:", reply)
```

**Notes:**

* Easily extendable with tools and memory handling.
* Suitable for more sophisticated decision-making.

---

## 5. Next Steps

* **Add Memory:** Persist conversation history or task state.
* **Integrate Tools:** Call external APIs (search, database, code execution).
* **Implement Hybrid Architecture:** Combine reactive safety checks with deliberative planning.
* **Evaluate & Metrics:** Measure task success, latency, and adaptability.

---
