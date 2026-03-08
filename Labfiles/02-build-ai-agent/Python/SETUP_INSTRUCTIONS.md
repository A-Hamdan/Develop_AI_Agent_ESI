# Azure AI Agent Setup Instructions

Complete step-by-step guide to run `agent.py` locally using VS Code.

---

## Prerequisites

Before starting, ensure you have the following installed:

| Tool | Download Link |
|------|---------------|
| VS Code | https://code.visualstudio.com/download |
| Python 3.10+ | https://www.python.org/downloads/ |
| Azure CLI | https://learn.microsoft.com/en-us/cli/azure/install-azure-cli |
| Git | https://git-scm.com/downloads |

---

## Step 1: Clone or Download the Repository

### Option A: Using UI (GitHub Website)
1. Open your browser and go to your GitHub repository
2. Click the green **"Code"** button
3. Click **"Download ZIP"**
4. Extract the ZIP file to a folder (e.g., `C:\Projects\ai-agents`)

### Option B: Using CLI (Terminal/Command Prompt)
```bash
# Open Terminal (Mac/Linux) or Command Prompt (Windows)
# Navigate to where you want to save the project
cd C:\Projects

# Clone the repository
git clone <your-repo-url>

# Navigate into the project folder
cd ai-agents
```

---

## Step 2: Open Project in VS Code

### Option A: Using UI
1. Open **VS Code**
2. Click **File** → **Open Folder...** (or press `Ctrl+K Ctrl+O`)
3. Navigate to: `Labfiles/02-build-ai-agent/Python`
4. Click **Select Folder**
5. If prompted "Do you trust the authors?", click **Yes, I trust the authors**

### Option B: Using CLI
```bash
# Navigate to the Python folder
cd Labfiles/02-build-ai-agent/Python

# Open VS Code in current folder
code .
```

---

## Step 3: Open the Terminal in VS Code

### Option A: Using UI
1. In VS Code, click **View** → **Terminal** from the top menu
2. Or press the keyboard shortcut: `` Ctrl + ` `` (backtick key, below Esc)

### Option B: Using Menu Bar
1. Click **Terminal** → **New Terminal** from the top menu

You should see a terminal panel appear at the bottom of VS Code.

---

## Step 4: Create a Virtual Environment

### Option A: Using UI (VS Code Command Palette)
1. Press `Ctrl+Shift+P` to open Command Palette
2. Type: `Python: Create Environment`
3. Select **Venv**
4. Select your Python interpreter (choose Python 3.10 or higher)
5. Check the box for `requirements.txt` when prompted
6. Wait for VS Code to create and activate the environment

### Option B: Using CLI (Terminal)

**Windows (PowerShell):**
```powershell
# Create virtual environment
python -m venv .venv

# Activate it
.venv\Scripts\Activate.ps1

# If you get an execution policy error, run this first:
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

**Windows (Command Prompt):**
```cmd
# Create virtual environment
python -m venv .venv

# Activate it
.venv\Scripts\activate.bat
```

**Mac/Linux:**
```bash
# Create virtual environment
python3 -m venv .venv

# Activate it
source .venv/bin/activate
```

**Verify activation:** You should see `(.venv)` at the beginning of your terminal prompt.

---

## Step 5: Install Dependencies

### Option A: Using UI
1. Press `Ctrl+Shift+P` to open Command Palette
2. Type: `Python: Install Dependencies`
3. Select `requirements.txt`

### Option B: Using CLI
```bash
# Make sure virtual environment is activated (you see .venv in prompt)
pip install -r requirements.txt
```

**Expected output:**
```
Successfully installed azure-ai-projects-2.0.0b1 azure-identity-... python-dotenv-... openai-...
```

---

## Step 6: Login to Azure

### Option A: Using UI (Browser Login)
1. In VS Code terminal, type:
   ```bash
   az login
   ```
2. A browser window will open automatically
3. Sign in with your **Azure account credentials**
4. After successful login, close the browser tab
5. Return to VS Code - you should see your subscription info

### Option B: Using CLI (Device Code - for remote/SSH)
```bash
az login --use-device-code
```
Then follow the instructions to open the URL and enter the code.

### Verify Login
```bash
# Check which account you're logged into
az account show

# List all subscriptions
az account list --output table
```

### Set the Correct Subscription
```bash
# If you have multiple subscriptions, set the correct one
az account set --subscription "<your-subscription-name-or-id>"
```

---

## Step 7: Get Your Azure AI Project Details

### Finding Your PROJECT_ENDPOINT:

1. Open your browser and go to: https://ai.azure.com
2. Sign in with your Azure account
3. Click on your **Project** from the list
4. Click **Overview** in the left sidebar
5. Look for **"Endpoint"** - copy this URL
   - It looks like: `https://your-project-name.services.ai.azure.com`

### Finding Your MODEL_DEPLOYMENT_NAME:

1. In Azure AI Foundry, click **Deployments** in the left sidebar
2. Find your deployed model (e.g., `gpt-4o`, `gpt-4.1`, `gpt-35-turbo`)
3. Copy the **Deployment name** (not the model name)

---

## Step 8: Configure Environment Variables

### Option A: Using UI (VS Code Editor)
1. In VS Code Explorer (left panel), find the `.env` file
2. Double-click to open it
3. Replace the placeholder values:
   ```
   PROJECT_ENDPOINT="https://your-actual-project.services.ai.azure.com"
   MODEL_DEPLOYMENT_NAME="gpt-4o"
   ```
4. Save the file: `Ctrl+S`

### Option B: Using CLI
```bash
# Windows (PowerShell)
notepad .env

# Mac/Linux
nano .env
```

Update with your actual values and save.

---

## Step 9: Run the Agent

### Option A: Using UI (VS Code Run Button)
1. Open `agent.py` in VS Code
2. Click the **Play button** (▶) in the top-right corner
3. Or press `F5` to run with debugging

### Option B: Using CLI
```bash
# Make sure you're in the Python folder and venv is activated
python agent.py
```

### Expected Output:
```
Category,Cost
Accommodation, 674.56
Transportation, 2301.00
Meals, 267.89
Misc., 34.50

Uploaded data.txt
Using agent: data-agent
Enter a prompt (or type 'quit' to exit): 
```

### Test Prompts to Try:
- `What is the total cost?`
- `Which category has the highest cost?`
- `Calculate the average cost per category`
- `Create a pie chart of the costs`

Type `quit` to exit the program.

---

## Troubleshooting

### Issue: "DefaultAzureCredential failed to retrieve a token"
**Solution:**
```bash
# Clear existing login and re-authenticate
az account clear
az login

# Verify you're logged in
az account show
```

### Issue: "Module not found" errors
**Solution:**
```bash
# Make sure virtual environment is activated
# You should see (.venv) in your terminal prompt

# Reinstall dependencies
pip install -r requirements.txt
```

### Issue: "Execution policy" error on Windows PowerShell
**Solution:**
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Issue: "Invalid PROJECT_ENDPOINT"
**Solution:**
- Verify the endpoint URL is correct in `.env`
- Make sure there are no extra spaces or quotes issues
- The URL should start with `https://`

### Issue: "Model deployment not found"
**Solution:**
- Check your deployment name in Azure AI Foundry → Deployments
- Make sure the model is actually deployed (status: Succeeded)
- Use the exact deployment name, not the model name

### Issue: Python not recognized
**Solution:**
- Reinstall Python from https://www.python.org
- During installation, check "Add Python to PATH"
- Restart VS Code after installation

---

## File Structure

```
Labfiles/02-build-ai-agent/Python/
├── agent.py              # Main Python script
├── data.txt              # Sample data file (costs)
├── requirements.txt      # Python dependencies
├── .env                  # Environment variables (your config)
├── .venv/                # Virtual environment (created by you)
└── SETUP_INSTRUCTIONS.md # This file
```

---

## Quick Reference Commands

| Action | Command |
|--------|---------|
| Activate venv (Windows) | `.venv\Scripts\activate` |
| Activate venv (Mac/Linux) | `source .venv/bin/activate` |
| Install dependencies | `pip install -r requirements.txt` |
| Azure login | `az login` |
| Check Azure account | `az account show` |
| Run agent | `python agent.py` |

---

## Need Help?

- **Azure AI Foundry Docs:** https://learn.microsoft.com/azure/ai-studio/
- **Azure Identity Docs:** https://learn.microsoft.com/python/api/azure-identity/
- **Azure CLI Docs:** https://learn.microsoft.com/cli/azure/

