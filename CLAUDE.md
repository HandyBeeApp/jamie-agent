# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Jamie Clerk** is an AI assistant service for small business owners, built with Google's Agent Development Kit (ADK). Jamie learns from business data and feedback to automatically reply to customer enquiries.

**Current Capabilities:**
- Answers customer enquiries using business-specific data
- Retrieves information from PostgreSQL database containing customer data
- Uses RAG (Retrieval-Augmented Generation) for context-aware responses

**Future Roadmap:**
- Additional tools will be added as Jamie's capabilities expand
- Learning from feedback to improve response quality over time

**Technical Foundation:**
- Built on GCP Agent Starter Pack template (version `0.16.0`)
- Initially configured with Vertex AI RAG Engine example
- Being adapted to use PostgreSQL for business data storage

## Environment Setup

### Prerequisites
- **uv**: Python package manager for all dependency management
- **Google Cloud SDK**: For GCP services
- **Terraform**: For infrastructure deployment
- **Poetry**: Currently used (note: project uses both uv and Poetry)

### Authentication
```bash
gcloud auth application-default login
gcloud auth application-default set-quota-project YOUR_PROJECT_ID
gcloud config set project PROJECT_ID
gcloud services enable aiplatform.googleapis.com
gcloud services enable discoveryengine.googleapis.com
```

### Environment Variables
Create `.env` file from `.env.example` and configure:
- `RAG_CORPUS`: Vertex AI RAG corpus resource name (format: `projects/123/locations/europe-west4/ragCorpora/456`)
  - **Note**: This is from the template; will be replaced with PostgreSQL connection for business data
- Google Cloud project settings
- PostgreSQL database credentials (for business customer data)

## Common Commands

### Development
```bash
make install              # Install dependencies using uv
make playground           # Launch Streamlit UI (select 'rag' folder in UI)
uv run adk web .          # Alternative playground command
```

### Testing
```bash
make test                 # Run unit and integration tests
make lint                 # Run code quality checks (codespell, ruff, mypy)
uv run pytest tests/unit  # Run unit tests only
uv run pytest tests/integration  # Run integration tests only
```

### Deployment
```bash
make backend              # Deploy agent to Agent Engine
make setup-dev-env        # Provision dev environment with Terraform
```

### Running Python Scripts
Always use `uv` to execute Python commands:
```bash
uv run python script.py
```

## Project Architecture

### Core Agent Structure
- **[rag/agent.py](rag/agent.py)**: Main agent definition with `root_agent` (LlmAgent)
  - **Agent Name**: `ask_rag_agent` (will be renamed to reflect Jamie Clerk identity)
  - **Model**: `gemini-2.5-flash-lite`
  - **Current Tool**: `VertexAiRagRetrieval` (template example - being replaced)
  - **Target Tool**: PostgreSQL retrieval for business customer data
- **[rag/prompts.py](rag/prompts.py)**: Instruction prompts guiding agent behavior
  - Current: Generic documentation assistant instructions
  - Target: Jamie Clerk persona for handling customer enquiries
- **[rag/agent_engine_app.py](rag/agent_engine_app.py)**: Agent Engine application logic for deployment

### Jamie Clerk Architecture (In Development)
1. **Business Data Source**: PostgreSQL database with customer data
   - Customer enquiry history
   - Business-specific information
   - Feedback data for learning
2. **Enquiry Handling**: Jamie retrieves relevant context from PostgreSQL to answer customer questions
3. **Response Learning**: System will incorporate feedback to improve responses over time
4. **Tool Expansion**: Additional capabilities will be added as requirements emerge

### Current Template Patterns (To Be Adapted)
1. **RAG Retrieval**: Uses `VertexAiRagRetrieval` (will be replaced with PostgreSQL tool)
2. **Citation System**: Provides citations at end of answers (may be adapted for customer context)
3. **Selective Tool Use**: Only uses retrieval for specific questions, not casual conversation

### Directory Structure
```
jamie-agent/
├── rag/                      # Core agent code
│   ├── agent.py              # Main agent definition
│   ├── agent_engine_app.py   # Agent Engine app
│   ├── prompts.py            # Agent instructions
│   ├── utils/                # Utilities (tracing, deployment, GCS)
│   └── shared_libraries/     # RAG corpus preparation
├── deployment/               # Terraform infrastructure
├── tests/                    # Unit, integration, and load tests
├── notebooks/                # Jupyter notebooks for prototyping
└── Makefile                  # Common commands
```

## Agent Configuration Details

### RAG Retrieval Parameters
- `similarity_top_k`: 10 (number of chunks to retrieve)
- `vector_distance_threshold`: 0.6 (minimum similarity score)

### Model Selection
When using Gemini models, prefer the **2.5 family** for optimal performance:
- `gemini-2.5-pro` for complex reasoning
- `gemini-2.5-flash` for fast responses
- Current: `gemini-2.5-flash-lite` (lightweight variant)

## Development Workflow

1. **Prototype**: Use notebooks in `notebooks/` for experimentation
2. **Integrate**: Edit `rag/agent.py` to modify agent logic
3. **Test Locally**: Use `make playground` or create programmatic test scripts
4. **Deploy Dev**: Run `make setup-dev-env` then `make backend`
5. **Production CI/CD**: Use `uvx agent-starter-pack setup-cicd` for automated pipeline

## Testing Guidelines

### Programmatic Testing (Recommended)
Create test scripts that invoke the agent programmatically:
```python
import asyncio
from google.adk.runners import Runner
from google.adk.sessions import InMemorySessionService
from rag.agent import root_agent
from google.genai import types as genai_types

async def main():
    session_service = InMemorySessionService()
    await session_service.create_session(
        app_name="rag", user_id="test_user", session_id="test_session"
    )
    runner = Runner(agent=root_agent, app_name="rag", session_service=session_service)

    async for event in runner.run_async(
        user_id="test_user",
        session_id="test_session",
        new_message=genai_types.Content(
            role="user",
            parts=[genai_types.Part.from_text(text="Your question here")]
        ),
    ):
        if event.is_final_response():
            print(event.content.parts[0].text)

if __name__ == "__main__":
    asyncio.run(main())
```

Run with: `uv run python your_test.py`

## Monitoring & Observability

- Uses OpenTelemetry for events to Google Cloud Trace and Logging
- Custom tracer in [rag/utils/tracing.py](rag/utils/tracing.py) handles large payloads via GCS
- Log Router sinks data to BigQuery (provisioned by Terraform)
- Looker Studio dashboard template available for visualization

## Deployment Notes

### Agent Engine Deployment
The `make backend` command:
1. Exports dependencies to `.requirements.txt` using `uv export`
2. Runs [rag/agent_engine_app.py](rag/agent_engine_app.py) for deployment

### Infrastructure
- Terraform configs in `deployment/` directory
- Recommended: Use `uvx agent-starter-pack setup-cicd` for automated setup
- Manual deployment: See [deployment/README.md](deployment/README.md)

## Important Context Files

- **[GEMINI.md](GEMINI.md)**: Comprehensive ADK reference guide and Agent Starter Pack documentation
- **[README.md](README.md)**: Project overview and quick start
- **[README_PROJECT_SETUP.md](README_PROJECT_SETUP.md)**: Detailed setup instructions
- **[README_PROJECT_RUN.md](README_PROJECT_RUN.md)**: Runtime instructions
