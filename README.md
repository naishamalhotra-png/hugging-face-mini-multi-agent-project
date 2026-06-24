# Multi-Agent Research Assistant with SmolAgents

## Overview

This project is a simple multi-agent research system built using the **SmolAgents** framework from Hugging Face. It demonstrates how multiple AI agents can collaborate to perform web research, retrieve information from online sources, and generate evidence-based answers to complex questions.

The system uses a hierarchical agent architecture:

* **Manager Agent** – Responsible for planning, reasoning, and coordinating tasks.
* **Web Search Agent** – Specialized agent responsible for searching the web and extracting content from webpages.
* **Custom Webpage Tool** – Fetches webpage content and converts it into markdown for easier processing by language models.

The project was created as part of the Hugging Face AI Agents learning track and serves as a foundational implementation of a multi-agent workflow.

---

## Features

### Multi-Agent Architecture

The project separates responsibilities across agents:

#### Manager Agent (`CodeAgent`)

* Handles high-level reasoning.
* Breaks down complex tasks.
* Delegates information gathering to specialized agents.
* Synthesizes final answers.

#### Web Search Agent (`ToolCallingAgent`)

* Performs internet searches.
* Visits webpages.
* Extracts relevant information.
* Returns findings to the manager agent.

---

### Custom Webpage Scraping Tool

A custom tool called `visit_webpage()` is implemented using the `@tool` decorator.

Capabilities:

* Downloads webpage content.
* Uses browser-like headers to reduce blocking.
* Converts HTML into Markdown format.
* Cleans excessive whitespace.
* Handles network and request errors gracefully.

Example:

```python
visit_webpage("https://example.com")
```

Output:

```markdown
# Example Domain

This domain is for use in illustrative examples...
```

---

## Technology Stack

### Core Libraries

* Python
* SmolAgents
* Requests
* Markdownify
* Regular Expressions (re)

### AI Model

```python
Qwen/Qwen3-Next-80B-A3B-Thinking
```

Used through:

```python
InferenceClientModel
```

---

## Project Architecture

```text
User Query
    │
    ▼
Manager Agent (CodeAgent)
    │
    ▼
Web Search Agent (ToolCallingAgent)
    │
 ┌──┴──────────────┐
 │                 │
 ▼                 ▼
WebSearchTool   visit_webpage()
 │                 │
 └───────┬─────────┘
         ▼
Information Gathering
         ▼
Manager Agent
         ▼
Final Response
```

---

## Agent Configuration

### Web Search Agent

```python
web_agent = ToolCallingAgent(
    tools=[WebSearchTool(), visit_webpage],
    model=model,
    max_steps=10,
    name="web_search_agent",
    description="Runs web searches for you",
)
```

Responsibilities:

* Search the web.
* Visit webpages.
* Extract source information.
* Return findings.

---

### Manager Agent

```python
manager_agent = CodeAgent(
    tools=[],
    model=model,
    managed_agents=[web_agent],
    additional_authorized_imports=[
        "time",
        "numpy",
        "pandas"
    ]
)
```

Responsibilities:

* Coordinate research.
* Delegate tasks.
* Analyze results.
* Produce final answers.

---

## Example Research Query

```python
answer = manager_agent.run(
    "If LLM training continues to scale up at the current rhythm until 2030, what would be the electric power in GW required to power the biggest training runs by 2030? What would that correspond to, compared to some countries? Please provide a source for any numbers used."
)
```

This demonstrates the system's ability to:

1. Search for relevant data.
2. Gather supporting evidence.
3. Analyze trends.
4. Generate a reasoned response with sources.

---

## Key Learning Objectives

This project explores several important AI agent concepts:

### Tool Usage

Allowing language models to interact with external tools.

### Agent Specialization

Assigning different responsibilities to different agents.

### Agent Delegation

Enabling one agent to call another when specialized knowledge is required.

### Retrieval-Augmented Research

Combining web search with reasoning capabilities.

### Multi-Step Reasoning

Breaking complex questions into smaller tasks and solving them iteratively.

---

## Current Limitations

* Basic webpage extraction without advanced content filtering.
* No persistent memory between runs.
* No caching of search results.
* No structured source verification pipeline.
* Limited to publicly accessible webpages.
* No user interface or deployment layer.

---

## Future Improvements

Potential extensions include:

* Support for additional specialized agents.
* Citation validation and source ranking.
* Persistent memory and knowledge storage.
* PDF and document ingestion.
* Data visualization tools.
* Financial and scientific research agents.
* Local model support.
* Web application interface.

---

## Educational Purpose

This project is intended for learning and experimentation with:

* AI Agents
* Multi-Agent Systems
* Tool Calling
* Autonomous Research Workflows
* SmolAgents Framework
* Large Language Model Orchestration

It serves as a foundational example of how multiple AI agents can collaborate to solve complex research problems using external tools and web-based information retrieval.

