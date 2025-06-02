# Azure AI Foundry Samples

This repository contains sample code for working with Azure AI Foundry (Azure OpenAI) services using two different authentication methods.

## Files Overview

- **`run_model.py`** - Uses Azure AD authentication (AzureCliCredential) with hardcoded endpoint
- **`run_model_apikey.py`** - Uses API key authentication with environment variables
- **`requirements.txt`** - Python dependencies
- **`.env`** - Environment variables configuration
- **`INSTRUCTIONS.md`** - Additional instructions
- **`.gitignore`** - Git ignore file for Python projects

## Prerequisites

- Python 3.8 or higher
- Azure subscription with Azure OpenAI access
- For Azure AD authentication: Azure CLI installed and appropriate RBAC permissions
- For API key authentication: Valid Azure OpenAI API key

## Quick Setup

### 1. Create and Activate Virtual Environment

**Windows:**
```bash
python -m venv .venv
.venv\Scripts\activate
```

**macOS/Linux:**
```bash
python -m venv .venv
source .venv/bin/activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

**Dependencies included:**
- `openai` - Azure OpenAI Python SDK
- `azure-identity` - Azure authentication library
- `python-dotenv` - Environment variable management
- `ansible-core` - Infrastructure automation (if needed)

### 3. Configure Environment Variables

Create a `.env` file in the root directory with your Azure OpenAI configuration:

```env
# Azure OpenAI Configuration
AZURE_EXISTING_AIPROJECT_ENDPOINT="https://your-resource-name.cognitiveservices.azure.com/"
AZURE_OPENAI_API_KEY="your-api-key-here"
DEPLOYMENT_NAME="gpt-4.1-mini"

# Azure Environment (Optional)
AZURE_ENV_NAME="your-env-name"
AZURE_LOCATION="your-location"
AZURE_SUBSCRIPTION_ID="your-subscription-id"
AZURE_EXISTING_AIPROJECT_RESOURCE_ID="your-resource-id"
```

**Note:** The current `.env` uses specific variable names. Ensure your `.env` file matches the format above.

## Authentication Methods

This repository provides two different authentication approaches:

### 1. API Key Authentication (`run_model_apikey.py`) - **Recommended**

This file uses API key authentication and reads configuration from environment variables:

```bash
python run_model_apikey.py
```

**Features:**
- Reads endpoint from `AZURE_EXISTING_AIPROJECT_ENDPOINT` environment variable
- Reads deployment name from `DEPLOYMENT_NAME` environment variable  
- Reads API key from `AZURE_OPENAI_API_KEY` environment variable
- Includes advanced message formatting with system and user roles
- More reliable than Azure AD authentication

**Requirements:**
- Configure your `.env` file with the required variables (see Configuration section)
- Ensure `AZURE_OPENAI_API_KEY` is set

### 2. Azure AD Authentication (`run_model.py`)

This file uses Azure CLI authentication with hardcoded endpoint values:

```bash
python run_model.py
```

**Features:**
- Uses `AzureCliCredential` for authentication
- Hardcoded endpoint: `https://icttestaifoundry.cognitiveservices.azure.com/`
- Hardcoded deployment: `gpt-4.1-mini`
- Simple message structure

**Requirements:**
- Azure CLI must be installed (`az --version` to verify)
- Must be logged in with `az login`
- Appropriate RBAC permissions (see below)

#### Installing Azure CLI

If you get "Azure CLI not found on path" error when running `run_model.py`:

**Windows (Recommended):**
```bash
winget install Microsoft.AzureCLI
```

**macOS:**
```bash
brew install azure-cli
```

**Linux:**
```bash
curl -sL https://aka.ms/InstallAzureCli | sudo bash
```

**After installation:**
1. Close and reopen your terminal
2. Verify: `az --version`
3. Login: `az login`

#### Required RBAC Roles (for Azure AD Authentication)

To use Azure AD authentication with `run_model.py`, assign one of these roles to your Azure identity:

- **Cognitive Services OpenAI User** (Recommended)
  - Role ID: `5e0bd9bd-7b93-4f28-af87-19fc36ad61bd`
  - Allows using Azure OpenAI models through an endpoint
  
- **Cognitive Services OpenAI Contributor**
  - Role ID: `a001fd3d-188f-4b5d-821b-7da978bf7442`
  - Full access to manage Azure OpenAI resources and use models

## Running the Applications

### Method 1: API Key Authentication (Recommended)
```bash
# Ensure your .env file is configured with all required variables
python run_model_apikey.py
```

**Expected output:** The script will display authentication method and then show a response about Paris travel recommendations.

### Method 2: Azure AD Authentication
```bash
# Ensure Azure CLI is installed and you're logged in
az login
python run_model.py
```

**Expected output:** The script will display the response about Paris travel recommendations.

Both scripts send the same prompt: "I am going to Paris, what should I see?" and display the AI assistant's response.

## Customizing the Code

### For `run_model_apikey.py`:
- Edit the `chat_prompt` variable to modify the conversation
- The script uses structured message format with system and user roles
- Configuration is read from environment variables

### For `run_model.py`:
- Edit the `messages` parameter in the `client.chat.completions.create()` call
- Endpoint and deployment are currently hardcoded
- Simple message structure for basic use cases

## Troubleshooting

### Quick Fixes for Common Issues

| Error | Solution |
|-------|----------|
| `Azure CLI not found on path` | Install Azure CLI: `winget install Microsoft.AzureCLI` (Windows) or use `python run_model_apikey.py` |
| `AZURE_OPENAI_API_KEY environment variable is required` | Add API key to your `.env` file |
| `PermissionDenied` (401) with Azure AD | Assign "Cognitive Services OpenAI User" role |
| `CredentialUnavailableError` | Run `az login` or use API key method instead |
| Package installation failures | Activate virtual environment first |

### Azure CLI Issues

#### "Azure CLI not found on path" Error
This error occurs when running `run_model.py` without Azure CLI installed:

**Installation Options:**

**Windows:**
```bash
winget install Microsoft.AzureCLI
```

**macOS:**
```bash
brew install azure-cli
```

**Linux:**
```bash
curl -sL https://aka.ms/InstallAzureCli | sudo bash
```

**Alternative Solution:**
Use the API key version instead:
```bash
python run_model_apikey.py
```

### Authentication Issues

#### API Key Authentication (`run_model_apikey.py`)
- **Missing API Key**: Ensure `AZURE_OPENAI_API_KEY` is set in your `.env` file
- **Invalid API Key**: Verify the key is correct and hasn't expired  
- **Wrong Endpoint**: Check `AZURE_EXISTING_AIPROJECT_ENDPOINT` format in `.env`
- **Environment File**: Ensure `.env` file is in the root directory

#### Azure AD Authentication (`run_model.py`)
- **Not Logged In**: Run `az login` to authenticate with Azure CLI
- **Insufficient Permissions**: Verify RBAC role assignments in Azure portal
- **Wrong Tenant**: Use `az login --tenant your-tenant-id` if needed
- **Subscription Access**: Ensure you have access to the subscription

### Environment Variables

Your `.env` file should contain these variables:

```env
# Required for API key authentication
AZURE_EXISTING_AIPROJECT_ENDPOINT="https://your-resource-name.cognitiveservices.azure.com/"
AZURE_OPENAI_API_KEY="your-api-key-here"
DEPLOYMENT_NAME="gpt-4.1-mini"

# Optional Azure configuration
AZURE_ENV_NAME="your-env-name"
AZURE_LOCATION="your-location"  
AZURE_SUBSCRIPTION_ID="your-subscription-id"
AZURE_EXISTING_AIPROJECT_RESOURCE_ID="your-resource-id"
```

**Note:** The `run_model.py` file doesn't use environment variables - it has hardcoded values.
If you get `CredentialUnavailableError: Azure CLI not found on path`:

**Quick Fix - Use API Key Authentication Instead:**
```bash
# Use the API key version which doesn't require Azure CLI
python run_model_apikey.py
```

**Or Install Azure CLI:**
```bash
# Use the automated installer script
python install_azure_cli.py
```

**Manual Azure CLI Installation:**

**Windows:**
```bash
# Using winget (recommended)
winget install -e --id Microsoft.AzureCLI

# Or download MSI installer from:
# https://aka.ms/installazurecliwindows
```

**macOS:**
```bash
# Using Homebrew
brew install azure-cli
```

**Linux (Ubuntu/Debian):**
```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

**After installation:**
```bash
# Restart your terminal, then login
az login

# Verify installation
az --version
```

### Quick Fixes for Common Issues

| Error | Solution |
|-------|----------|
| `Azure CLI not found on path` | Install Azure CLI: `python install_azure_cli.py` or use `python run_model_apikey.py` |
| `PermissionDenied` (401) with Azure AD | Assign "Cognitive Services OpenAI User" role - see detailed steps below |
| `AuthenticationError` with API Key | Check your `.env` file has correct `AZURE_OPENAI_API_KEY` |
| `CredentialUnavailableError` with Azure CLI | Run `az login` - required for run_model.py |
| Virtual environment issues | Run `python setup_venv.py` for automated setup |
| Package installation failures | Activate venv first, then `pip install --upgrade pip` |

### Virtual Environment Issues

#### Virtual Environment Not Found
If you get an error that the virtual environment doesn't exist:
```bash
# Delete any existing .venv folder and recreate
rm -rf .venv  # (Linux/macOS) or rmdir /s .venv (Windows)
python -m venv .venv
```

#### Activation Issues on Windows
If activation fails on Windows:
```bash
# Try using the full path
C:\path\to\your\project\.venv\Scripts\activate.bat

# Or use PowerShell
.venv\Scripts\Activate.ps1
```

#### Package Installation Issues
If pip install fails:
```bash
# Make sure you're in the activated virtual environment
# You should see (.venv) in your command prompt

# Try upgrading pip first
python -m pip install --upgrade pip

# Install packages one by one to identify issues
pip install openai
pip install azure-identity
pip install python-dotenv
pip install ansible-core
```

#### Verify Virtual Environment is Active
Check that your virtual environment is properly activated:
```bash
# Check Python path (should point to .venv)
which python   # Linux/macOS
where python   # Windows

# Check installed packages
pip list
```

### Authentication Issues

#### API Key Authentication (`run_model_apikey.py`)
- **Missing API Key**: Ensure `AZURE_OPENAI_API_KEY` is set in your `.env` file
- **Invalid API Key**: Verify the key is correct and hasn't expired
- **Environment File**: Ensure `.env` file is in the root directory

## Additional Resources

- [Azure OpenAI Documentation](https://learn.microsoft.com/azure/ai-services/openai/)
- [Azure Identity Documentation](https://learn.microsoft.com/python/api/azure-identity/)
- [Azure CLI Installation Guide](https://learn.microsoft.com/cli/azure/install-azure-cli)
- [Azure RBAC Documentation](https://learn.microsoft.com/azure/role-based-access-control/overview)

## Project Structure Summary

```
├── run_model.py                    # Azure AD auth (hardcoded values)
├── run_model_apikey.py            # API key auth (env variables)
├── requirements.txt               # Python dependencies
├── .env                          # Environment configuration
├── README.md                     # This file
├── INSTRUCTIONS.md               # Additional instructions
└── .gitignore                    # Git ignore file
```

## Next Steps

1. **Start with API key authentication** - it's more reliable and easier to set up
2. **Configure your `.env` file** with your Azure OpenAI credentials  
3. **Run the sample**: `python run_model_apikey.py`
4. **Customize the prompts** to fit your use case
5. **Consider Azure AD authentication** for production scenarios with proper RBAC
- Role assignments can take 5-10 minutes to take effect
- Test the connection:
```bash
python run_model.py
```

#### Alternative: Use API Key Authentication
If you need immediate access, switch to API key authentication:
```bash
python run_model_apikey.py
```

### Dependencies and Environment

- **openai~=1.60.2** - Azure OpenAI Python SDK
- **azure-identity~=1.17.1** - Azure authentication library  
- **python-dotenv~=1.0.1** - Environment variable management
- **ansible-core~=2.17.0** - (If needed for automation tasks)

### Environment Variables Reference

Create a `.env` file with the following variables:

```env
# Required for run_model_apikey.py
AZURE_OPENAI_API_KEY=your-api-key-here
ENDPOINT_URL=https://your-resource-name.openai.azure.com/
DEPLOYMENT_NAME=your-deployment-name

# Optional: Override default values in run_model.py
# (Currently hardcoded in the script)
```

## Additional Resources

- [Azure OpenAI Documentation](https://learn.microsoft.com/azure/ai-services/openai/)
- [DefaultAzureCredential Documentation](https://learn.microsoft.com/python/api/azure-identity/azure.identity.defaultazurecredential)
- [Azure RBAC Documentation](https://learn.microsoft.com/azure/role-based-access-control/overview)
