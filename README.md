# AI Agent Content Creation System

## Overview
This project demonstrates a multi-agent system built using crewAI that creates, edits, and publishes content automatically. The system consists of multiple AI agents working together to plan, write, edit, and publish articles to Dev.to.

This code is part of the Multi-Agent Systems course from DeepLearning.AI platform, with additional custom functionality for publishing to Dev.to.
https://www.deeplearning.ai/short-courses/multi-ai-agent-systems-with-crewai/


## Features
- **Content Planning**: An agent dedicated to researching and planning article structure
- **Content Writing**: An agent that transforms plans into well-written articles
- **Content Editing**: An agent that reviews and improves the written content
- **Content Publishing**: A custom agent that publishes the final article to Dev.to

## System Architecture

### Agents
The system uses four specialized agents:

1. **Content Planner** - Researches topics and creates an article outline
2. **Content Writer** - Writes the actual article based on the plan
3. **Editor** - Reviews and improves the article for quality and style
4. **Dev.to Publisher** - Optimizes and publishes the article to Dev.to

### Tasks
Each agent has dedicated tasks:

1. **Plan** - Create a comprehensive content plan with outline, SEO keywords, etc.
2. **Write** - Craft a compelling blog post based on the plan
3. **Edit** - Proofread and polish the post
4. **Publish** - Format and publish the post to Dev.to as a draft

## Custom Dev.to Integration
The Dev.to publishing functionality is a custom addition to the base course content. This integration:

- Automatically formats articles for the Dev.to platform
- Selects appropriate tags (up to 4)
- Publishes content as drafts for review
- Returns the URL of the published draft

## Requirements
- Python 3.7+
- crewAI 0.28.8+
- crewai_tools 0.1.6+
- gemini model 2.0 flash
- langchain_community 0.0.29+
- requests

## Setup
1. Install the required dependencies:
```bash
pip install crewai==0.28.8 crewai_tools==0.1.6 langchain_community==0.0.29 requests 
pip install 'crewai[tools]'
pip install langchain-google-genai
```

2. Set up your API keys:
```python
# Configure your API keys
os.environ["GEMINI_API_KEY"] = "your-gemini-api-key"
os.environ["DEVTO_API_KEY"] = "your-dev-to-api-key"
```

## Usage
To run the system, define a topic and execute the crew:

```python
# Create the crew with all agents and tasks
crew = Crew(
    agents=[planner, writer, editor, publisher],
    tasks=[plan, write, edit, publish],
    verbose=True
)

# Execute the crew with your desired topic
result = crew.kickoff(inputs={"topic": "AI agents"})
```

## Custom Dev.to Publishing Tool
The custom publishing tool is implemented using crewAI's BaseTool class and provides a clean interface for the publisher agent to interact with the Dev.to API. The tool handles:

- Article formatting
- Tag selection (limited to 4 as per Dev.to requirements)
- Draft or published status
- Series assignment
- Error handling for API responses

## Agent Workflow Diagram

```
┌───────────────┐     ┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│               │     │               │     │               │     │               │
│  Content      │     │  Content      │     │    Editor     │     │   Dev.to      │
│  Planner      │────▶│  Writer       │────▶│               │────▶│   Publisher   │
│               │     │               │     │               │     │               │
└───────────────┘     └───────────────┘     └───────────────┘     └───────────────┘
       │                     │                     │                     │
       ▼                     ▼                     ▼                     ▼
┌───────────────┐     ┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│  Content      │     │  Draft        │     │  Polished     │     │  Published    │
│  Plan         │     │  Article      │     │  Article      │     │  Article      │
│               │     │               │     │               │     │               │
└───────────────┘     └───────────────┘     └───────────────┘     └───────────────┘
```

## Acknowledgements
- This project is based on the crewAI framework from DeepLearning.AI's Multi-Agent Systems course
- The Dev.to publishing functionality is a custom extension to the base course material
