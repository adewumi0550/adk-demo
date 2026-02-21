# Google ADK Agent Demo

Welcome to this example project demonstrating how to build a multi-agent system using the [Google Agent Development Kit (ADK)](https://google.github.io/adk-docs/). 

This repository contains a simple `root_agent` that functions as a helpful assistant, powered by Google's Gemini models.

## 🚀 Installation & Setup

### 1. Prerequisites
- **Python 3.10+**: Download and install it from [python.org](https://www.python.org/downloads/).
- **pip**: Ensure `pip` is installed (it usually comes with Python by default).

### 2. Setup Virtual Environment (Recommended)
It is highly recommended to use a virtual environment so dependencies don't conflict with your OS.
```bash
python3 -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
```

### 3. Install Google ADK
Install the ADK CLI and runtimes via `pip`:
```bash
pip install google-adk
```

### 2. Getting your AI Studio API Key
To run the agent, you need an API key from Google AI Studio. 
1. Head over to [Google AI Studio](https://aistudio.google.com/).
2. Click on **Get API key**.
3. Create an API key in a new or existing project.

### 3. Configuring the `.env` File
Create a `.env` file in the root of your project directory to store your API credentials.

For standard AI Studio usage, your `.env` should look like this:

```env
GOOGLE_API_KEY=your_ai_studio_api_key_here
GOOGLE_GENAI_USE_VERTEXAI=FALSE
```

The application automatically loads these environment variables at runtime!

---

## 🏃 Running the Agent

You can interact with your agent using the built-in ADK CLI tools. 

To start the interactive command-line interface, run:
```bash
adk run .
```

To start the local ADK Web server interface (which provides a nice UI at `http://localhost:8000`), run:
```bash
adk web
```
*(Note: Press `Ctrl+C` to stop the web server).*

---

## 🛠️ Customizing the Model (Upgrading to Gemini Pro)

By default, this agent is configured to use `gemini-2.5-flash` for fast responses. If your tasks require complex reasoning or large context windows, you can easily upgrade to **Gemini Pro**!

To customize the model, open `agent.py` and change the `model` parameter:

```python
from google.adk.agents.llm_agent import Agent

root_agent = Agent(
    model='gemini-2.5-pro', # <--- Change this from gemini-2.5-flash
    name='root_agent',
    description='A helpful assistant for user questions.',
    instruction='Answer user questions to the best of your knowledge',
)
```
You can dynamically configure the agent's instructions, available tools, and more inside this file.

---

## ☁️ Enterprise Deployment: Vertex AI vs AI Studio

While Google AI Studio is fantastic for prototyping and free-tier access, enterprise applications often require the robust security, compliance, and quota management provided by **Google Cloud Vertex AI**.

If you decide to switch to Vertex AI, here is why and how you do it:

### Why use Vertex AI?
- **IAM Security:** Fine-grained access control using Google Cloud IAM.
- **Data Privacy:** Your data isn't used to train Google's foundation models.
- **Enterprise SLA & Quotas:** Guaranteed uptime and higher rate limits.

### Configuration for Vertex AI
To switch your agent to Vertex AI, you don't need to change any Python code! Simply update your `.env` file:

```env
# Comment out the AI Studio Key
# GOOGLE_API_KEY=your_api_key

# Enable Vertex AI
GOOGLE_GENAI_USE_VERTEXAI=TRUE
GOOGLE_CLOUD_PROJECT=your-gcp-project-id
GOOGLE_CLOUD_LOCATION=us-central1
```

**⚠️ Important Permission Note for Vertex AI:**
To run the agent locally with Vertex AI, you MUST authenticate using your Google Cloud credentials. 

1. Install the [Google Cloud CLI (`gcloud`)](https://cloud.google.com/sdk/docs/install).
2. Authenticate your terminal:
   ```bash
   gcloud auth application-default login
   ```
3. Ensure your Google account has the **Vertex AI User** (`roles/aiplatform.user`) role in the specified Google Cloud Project, which grants the necessary `aiplatform.endpoints.predict` permission.

---

## 📚 Further Learning & Resources

Looking to build more complex agents? Learn about creating conversational workflows, function calling, custom tools, and more through the official tutorial:

[Build Intelligent Agents with the Google Agent Development Kit (Codelab)](https://codelabs.developers.google.com/devsite/codelabs/build-agents-with-adk-foundation#0)
