# Multi-Agent Service Matcher

**Track:** Enterprise Agents

**One-Sentence Summary:** A sequential multi-agent system that routes user queries to the right service in a knowledge marketplace by interpreting needs, matching them to a catalog, and polishing the response.

---

## Problem Statement

In service and knowledge marketplaces, users often struggle to find the right offering because they don't know the specific terminology used in catalogs. A user might say "my code is broken," while the service is listed as "Python Debug Helper." This semantic gap leads to frustration and inefficient discovery.

## Solution Overview

This project implements an intelligent **Multi-Agent Service Matcher** that acts as a broker between users and services. It uses a sequential pipeline of three specialized agents to bridge the gap between natural language and structured data:

1.  **Interpreter Agent**: Clarifies and normalizes the user's raw, potentially messy request.
2.  **Matcher Agent**: Uses semantic reasoning and a `get_services()` tool to find the best fit from the catalog.
3.  **Polisher Agent**: Formats the technical match into a warm, user-friendly response.

## Key Concepts Applied

This submission demonstrates the following key concepts from the Agents Intensive course:

1.  **Multi-Agent Systems (Sequential)**: A linear pipeline where the output of one agent becomes the input for the next.
2.  **Tools/API Usage**: The Matcher agent utilizes a function calling tool (`get_services()`) to retrieve real-time data.
3.  **Session State (Memory)**: A shared dictionary maintains context (user request, clarified intent, match result) across the agent workflow.
4.  **LLM Reasoning**: The system relies on reasoning to justify *why* a service matches, rather than simple keyword search.

## System Architecture

```
       User Request
           |
           v
┌─────────────────────────┐
│  AGENT 1: INTERPRETER   │  Role: Clarify and normalize the user's request
│  Input: Raw user text   │
│  Output: Clear summary  │
└──────────┬──────────────┘
           │ (writes to session)
           v
┌─────────────────────────┐
│  AGENT 2: MATCHER       │  Role: Find the best matching service
│  Input: Summary + Tool  │  Tool: get_services() → returns service list
│  Output: Best match     │
└──────────┬──────────────┘
           │ (writes to session)
           v
┌─────────────────────────┐
│  AGENT 3: POLISHER      │  Role: Format user-friendly final answer
│  Input: Matched service │
│  Output: Final response │
└──────────┬──────────────┘
           │
           v
      Final Answer
```

## Setup and Usage

### Prerequisites
- Python 3.8+
- Google Cloud Project with Vertex AI API enabled
- `google-genai` library

### Installation

1.  Clone the repository.
2.  Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```
3.  Create a `.env` file in the root directory with your Google Cloud details:
    ```env
    GOOGLE_CLOUD_PROJECT=your-project-id
    GOOGLE_CLOUD_LOCATION=us-central1
    ```
4.  Authenticate with Google Cloud:
    ```bash
    gcloud auth application-default login
    ```

### Running the Agent

The notebook `multi-agent-service-matcher-formal.ipynb` contains the complete implementation. You can run the pipeline using the `match_service` helper function:

```python
# Example usage
from notebook_module import match_service

response = match_service("my python code keeps crashing")
print(response)
```

## Example Scenarios

The system handles various domains by reasoning about the user's intent:

| User Request | Matched Service | Reasoning |
| :--- | :--- | :--- |
| "my python loop keeps skipping stuff" | **Python Debug Helper** | Identifies code debugging need. |
| "help with sat math word problems" | **SAT Math Tutor** | Matches specific academic subject. |
| "questions about filing taxes" | **Tax Filing Advisor** | Connects to government form assistance. |
| "improve freestyle breathing" | **Swimming Technique Review** | Links sports technique to coaching. |
