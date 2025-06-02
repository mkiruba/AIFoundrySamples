# Azure AI Foundry Samples

This repository contains sample code for working with Azure AI Foundry (Azure OpenAI) services using two different authentication methods.

## Files Overview

- **`run_model.py`** - Uses Azure AD authentication (DefaultAzureCredential)
- **`run_model_apikey.py`** - Uses API key authentication
- **`requirements.txt`** - Python dependencies
- **`setup_venv.py`** - Automated virtual environment setup script
- **`.env`** - Environment variables configuration (create this file)

## Prerequisites

- Python 3.8 or higher
- Azure subscription with Azure OpenAI access
- Appropriate RBAC permissions if using Azure AD authentication (see Authentication section)
- Azure CLI installed (for Azure AD authentication)

## Quick Setup (Automated)

For a quick automated setup, you can use the provided Python script:

```bash
# Run the automated setup script
python setup_venv.py
```

This script will:
- Create a virtual environment
- Activate it
- Install all dependencies
- Provide instructions for next steps

## Manual Setup Instructions

### 1. Create a Virtual Environment

```bash
# Create a new virtual environment
python -m venv .venv
```

### 2. Activate the Virtual Environment

**On Windows:**
```bash
.venv\Scripts\activate
```

**On macOS/Linux:**
```bash
source .venv/bin/activate
```

### 3. Install Dependencies

After activating the virtual environment, install the required packages:

```bash
# Upgrade pip first
python -m pip install --upgrade pip

# Install all required dependencies
pip install -r requirements.txt
```

### 4. Verify Installation

You can verify that the packages are installed correctly:

```bash
pip list
```

### Alternative: Using the install.sh Script (Linux/macOS)

You can use the provided shell script for automated setup:

```bash
# Make the script executable
chmod +x install.sh

# Run the script
./install.sh
```

## Configuration

Configure the application using environment variables. Create a `.env` file in the root directory:

**For API Key Authentication (run_model_apikey.py):**
```env
# Azure OpenAI endpoint URL
ENDPOINT_URL=https://your-resource-name.openai.azure.com/

# The name of your model deployment
DEPLOYMENT_NAME=your-deployment-name

# API key (required for run_model_apikey.py)
AZURE_OPENAI_API_KEY=your-api-key
```

**For Azure AD Authentication (run_model.py):**
The `run_model.py` file uses hardcoded values for demonstration. For production use, you should modify it to use environment variables:
```env
# Azure OpenAI endpoint URL
ENDPOINT_URL=https://your-resource-name.openai.azure.com/

# The name of your model deployment
DEPLOYMENT_NAME=your-deployment-name
```

## Authentication Methods

This repository provides two different authentication approaches:

### 1. API Key Authentication (`run_model_apikey.py`)

This file uses traditional API key authentication and includes environment variable support:

```bash
python run_model_apikey.py
```

**Requirements:**
- Set the `AZURE_OPENAI_API_KEY` environment variable in your `.env` file
- Configure `ENDPOINT_URL` and `DEPLOYMENT_NAME` in your `.env` file

### 2. Azure AD Authentication (`run_model.py`)

This file uses Azure CLI authentication via `AzureCliCredential`:

```bash
python run_model.py
```

**Note:** The current version has hardcoded endpoint and deployment values for demonstration. For production use, modify the script to use environment variables.

**AzureCliCredential** requires you to be authenticated via Azure CLI:
- You must run `az login` before using this script
- Uses only Azure CLI credentials (more direct than DefaultAzureCredential)
- Provides clearer error messages when authentication fails

#### Required RBAC Roles (for Azure AD Authentication)

To use Azure AD authentication with `run_model.py`, you'll need one of the following roles assigned to your Azure identity:

- **Cognitive Services OpenAI User** (Role ID: `5e0bd9bd-7b93-4f28-af87-19fc36ad61bd`)
  - Allows using Azure OpenAI models through an endpoint
  - **Recommended** for most use cases
  
- **Cognitive Services OpenAI Contributor** (Role ID: `a001fd3d-188f-4b5d-821b-7da978bf7442`)
  - Provides full access to manage Azure OpenAI resources and use the models

- **Cognitive Services User** (Role ID: `a97b65f3-24c7-4388-baec-2e87135dc908`)
  - Allows reading and listing keys but not direct resource access

#### Setting up Azure CLI Authentication

For Azure AD authentication, ensure you're logged in via Azure CLI (required for `run_model.py`):

```bash
# Login to Azure CLI (required)
az login

# Verify your account
az account show

# Set specific subscription if needed
az account set --subscription "your-subscription-id"
```

## Running the Applications

### Using API Key Authentication
```bash
# Make sure your .env file is configured with AZURE_OPENAI_API_KEY
python run_model_apikey.py
```

### Using Azure AD Authentication  
```bash
# Make sure you're logged in with Azure CLI: az login
python run_model.py
```

Both scripts will send a sample prompt about Paris travel recommendations to your Azure OpenAI deployment and display the response.

## Customizing the Code

- **For `run_model_apikey.py`**: Edit the `chat_prompt` variable to customize the conversation
- **For `run_model.py`**: Edit the `messages` parameter in the `client.chat.completions.create()` call
- **Endpoints and deployments**: Modify the endpoint and deployment variables in the respective files

## Troubleshooting

### Quick Fixes for Common Issues

| Error | Solution |
|-------|----------|
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

#### Azure AD Authentication (`run_model.py`)
- **Not Logged In**: Run `az login` to authenticate with Azure CLI (required)
- **Insufficient Permissions**: Verify RBAC role assignments (see Required RBAC Roles section)
- **Wrong Tenant**: Use `az login --tenant your-tenant-id` if needed
- **Subscription Access**: Ensure you have access to the subscription containing the OpenAI resource
- **Authentication Test**: Run `az account get-access-token --resource https://cognitiveservices.azure.com` to verify token access

#### Common 401 Authentication Errors
If you see a 401 error:

**For API Key method:**
1. Check your API key is correct in the `.env` file
2. Verify your endpoint URL format: `https://your-resource-name.openai.azure.com/`
3. Ensure your deployment name matches exactly (case-sensitive)

**For Azure AD method:**
1. Run `az login` and verify authentication with `az account show`
2. Check RBAC role assignments in the Azure portal
3. Verify the endpoint URL and deployment name in the code
4. Try running `az account get-access-token --resource https://cognitiveservices.azure.com` to test token acquisition

### Endpoint/Deployment Issues

- Verify that your endpoint URL is correct (format: `https://your-resource-name.openai.azure.com/`)
- Confirm that the deployment name exists and is active
- Check that your subscription has access to the specified model
- Ensure the API version is supported by your Azure OpenAI resource

### Specific Error: PermissionDenied (401)

If you see this error:
```
openai.AuthenticationError: Error code: 401 - {'error': {'code': 'PermissionDenied', 'message': 'The principal `{user-id}` lacks the required data action `Microsoft.CognitiveServices/accounts/OpenAI/deployments/chat/completions/action`'}}
```

This means your Azure identity lacks the proper RBAC role. **Follow these steps to fix it:**

#### Step 1: Identify Your Current Identity
```bash
# Check which account you're using
az account show --query user.name -o tsv
```

#### Step 2: Assign the Correct RBAC Role
You need to assign the **Cognitive Services OpenAI User** role to your identity:

**Option A: Using Azure Portal**
1. Go to your Azure OpenAI resource in the Azure Portal
2. Click on "Access control (IAM)" in the left menu
3. Click "+ Add" → "Add role assignment"
4. Search for and select "Cognitive Services OpenAI User"
5. Click "Next"
6. Select "User, group, or service principal"
7. Click "+ Select members"
8. Search for and select your user account
9. Click "Select" → "Review + assign" → "Assign"

**Option B: Using Azure CLI**
```bash
# Get your Azure OpenAI resource ID
az cognitiveservices account show --name "your-openai-resource-name" --resource-group "your-resource-group" --query id -o tsv

# Assign the role (replace the values below)
az role assignment create \
  --role "Cognitive Services OpenAI User" \
  --assignee-object-id "$(az ad signed-in-user show --query id -o tsv)" \
  --scope "/subscriptions/YOUR_SUBSCRIPTION_ID/resourceGroups/YOUR_RESOURCE_GROUP/providers/Microsoft.CognitiveServices/accounts/YOUR_OPENAI_RESOURCE_NAME"
```

#### Step 3: Wait and Test
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
