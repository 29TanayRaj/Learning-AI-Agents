# LangGraph Agent with Groq Integration

A powerful ReAct agent implementation using LangGraph and Groq that processes user queries using an advanced LLM and executes various tools based on need.

## Overview

This project demonstrates how to build an intelligent agent that:
- Uses a Groq-hosted language model (DeepSeek r1 Distill LLama 70B)
- Implements a ReAct pattern (Reasoning, Acting) for complex problem-solving
- Integrates multiple tools for calculations and web searches
- Leverages LangGraph for orchestrating the agent's decision flow

## Features

- **Advanced LLM Integration**: Uses DeepSeek r1 Distill LLama 70B via Groq's API
- **Tool Integration**: 
  - Basic math operations (add, multiply, divide, subtract)
  - Web search via DuckDuckGo
  - Date/time retrieval
- **LangGraph Workflow**: Orchestrates the decision process between reasoning and tool execution
- **Environment Configuration**: Uses dotenv for secure API key management

## Prerequisites

- Python 3.8+
- Groq API key

## Installation

1. Clone the repository
2. Install required packages:
   ```bash
   pip install langchain-groq langchain-core langgraph ipython python-dotenv requests
   ```
3. Create a `.env` file in the project root and add your Groq API key:
   ```
   API_KEY=your_groq_api_key_here
   ```

## Usage

Import the necessary libraries and initialize the agent:

```python
from your_module import react_graph
from langchain_core.messages import HumanMessage

# Create a query
messages = [HumanMessage(content="your question here")]

# Invoke the agent
response = react_graph.invoke({"messages": messages})

# Print results
for m in response['messages']:
    m.pretty_print()
```

## How It Works

The agent operates using a structured workflow:

1. **Initial Query Processing**: User's question is sent to the LLM for reasoning
2. **Tool Decision**: LLM decides if a tool should be used to answer the query
3. **Tool Execution**: If needed, the appropriate tool is executed
4. **Final Response**: LLM provides a final answer based on tool outputs or its own knowledge

### Graph Structure

The LangGraph implementation creates a decision flow with the following components:

- **Nodes**:
  - `reasoner`: Processes inputs and decides on next steps
  - `tools`: Executes selected tools when needed

- **Edges**:
  - START → reasoner
  - reasoner → tools (conditional)
  - tools → reasoner
  - reasoner → END (when no tool is needed)

## Available Tools

### Math Operations
- `add(a, b)`: Adds two integers
- `multiply(a, b)`: Multiplies two integers
- `divide(a, b)`: Divides two integers
- `substract(a, b)`: Subtracts second integer from first

### Information Retrieval
- `get_current_datetime()`: Returns current date and time
- `search(query)`: Performs web search via DuckDuckGo

## Example

```python
messages = [HumanMessage(content="what is the first movie russel crowe won an oscar for")]
messages = react_graph.invoke({"messages": messages})
for m in messages['messages']:
    m.pretty_print()
```

Output:
```
================================ Human Message =================================
what is the first movie russel crowe won an oscar for

================================== Ai Message ==================================
Tool Calls:
  duckduckgo_search (call_6rcm)
 Call ID: call_6rcm
  Args:
    query: Russell Crowe first Oscar win movie
    
================================= Tool Message =================================
Name: duckduckgo_search
[Search results about Russell Crowe's Oscar win]

================================== Ai Message ==================================
Russell Crowe won his first Oscar for his role in "Gladiator" at the 73rd Academy Awards in 2001.
```

## Visualization

The project includes visualization of the agent's decision graph using IPython's display capabilities and LangGraph's built-in visualization tools.

## License
MIT License

