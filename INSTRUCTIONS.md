# VS Code for the Web - Azure AI Foundry

We've generated a simple development environment for you to play with sample code to create and run the agent that you built in the Azure AI Foundry playground.

The Azure AI Foundry extension provides tools to help you build, test, and deploy AI models and AI Applications directly from VS Code. It offers simplified operations for interacting with your models, agents, and threads without leaving your development environment. Click on the Azure AI Foundry Icon on the left to see more.

Follow the instructions below to get started!

## Setup Your Environment

### 1. Create a Virtual Environment

First, set up a Python virtual environment with all the required dependencies:

```bash
python setup_venv.py
```

### 2. Activate the Virtual Environment

```bash
# On Windows
.venv\Scripts\activate

# On macOS/Linux
source .venv/bin/activate
```

### 3. Configure Authentication

The code supports two authentication methods:

#### Option 1: API Key Authentication (Default)
Ensure your `.env` file contains your Azure OpenAI API key:
```
AZURE_OPENAI_API_KEY=your-api-key-here
ENDPOINT_URL=https://your-resource-name.openai.azure.com/
DEPLOYMENT_NAME=your-deployment-name
```

#### Option 2: Azure AD Authentication
If you prefer to use Azure AD authentication (managed identity, Azure CLI, etc.), you don't need to set `AZURE_OPENAI_API_KEY`. Just make sure your authenticated identity has the "Cognitive Services OpenAI User" or "Cognitive Services OpenAI Contributor" RBAC role assigned.

## Run your model locally

To run the model that you deployed in AI Foundry, and view the output in the terminal run the following command:

```bash
python run_model.py
```

## Continuing on your local desktop

You can keep working locally on VS Code Desktop by clicking "Continue On Desktop..." at the bottom left of this screen. Be sure to take the .env file with you using these steps:

- Right-click the .env file
- Select "Download"
- Move the file from your Downloads folder to the local git repo directory
- For Windows, you will need to rename the file back to .env using right-click "Rename..."
