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
