# ✈️ Trip Planner Agent Team (LangGraph)
 
A small multi-agent project built with **LangGraph**. A customer asks for a trip, a team of agents plans it, a human approves it, and the whole conversation is saved in **SQLite**.
 
The notebook `trip_planner_agent_team.ipynb` explains every step in detail, so this README only covers the basics and how to run it.
 
**Author:** Mahnoor Iqbal | Python-AI Course, AshreiTech Academy
 
---
 
## What it does (short version)
 
- `budget_agent` checks the weather, converts currency and splits the budget.
- `itinerary_agent` writes a short day-by-day plan.
- `supervisor` decides who works next.
- `approval` pauses and waits for you: `yes`, `no`, or feedback to rewrite the plan.
- `book_trip` pretends to book, only after you say `yes`.
Both tools use **fake data**, so no real booking or real weather lookup happens.
 
## Tech stack
 
- Python 3.10 or newer
- [LangGraph](https://langchain-ai.github.io/langgraph/) and [LangChain](https://python.langchain.com/)
- [Groq](https://console.groq.com/) API with the model `openai/gpt-oss-120b` (free)
- SQLite (saves the conversation)
## Project files
 
```
.
├── trip_planner_agent_team.ipynb   # the full project
├── requirements.txt                # packages to install
├── .gitignore                      # keeps secrets and the database out of GitHub
└── README.md
```
 
---
 
## Setup and run
 
You can run this project in **Google Colab** (easiest) or on **your own computer**.
 
### Step 1: Get a free Groq API key
 
1. Go to [console.groq.com](https://console.groq.com/) and sign up or log in.
2. Open **API Keys** and click **Create API Key**.
3. Copy the key and keep it safe. **Never upload it to GitHub.**
### Option A: Run in Google Colab
 
1. Open [colab.research.google.com](https://colab.research.google.com/) and choose **File → Upload notebook**. Select `trip_planner_agent_team.ipynb`.
   (Or open it straight from GitHub: **File → Open notebook → GitHub** tab, then paste your repository link.)
2. Click the 🔑 **Secrets** icon on the left side.
3. Click **Add new secret**:
   - **Name:** `Groq_key` (it must match the name used in the notebook code exactly)
   - **Value:** your Groq API key
4. Turn on **Notebook access** for this secret.
5. Click **Runtime → Run all**, or run the cells one by one from top to bottom.
### Option B: Run on your own computer
 
**1. Clone the project**
 
```bash
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>
```
 
**2. Create a virtual environment**
 
```bash
python -m venv venv
```
 
Activate it:
 
- Windows: `venv\Scripts\activate`
- Mac / Linux: `source venv/bin/activate`
**3. Install the packages**
 
```bash
pip install -r requirements.txt
pip install jupyter
```
 
**4. Set your Groq API key**
 
Windows (Command Prompt):
 
```bash
set GROQ_API_KEY=your_key_here
```
 
Windows (PowerShell):
 
```powershell
$env:GROQ_API_KEY="your_key_here"
```
 
Mac / Linux:
 
```bash
export GROQ_API_KEY=your_key_here
```
 
**5. Change two lines in the notebook**
 
The notebook was written for Colab, so one cell uses Colab-only code. Open the notebook and, in the setup cell (the one that starts with `import os, operator, sqlite3`), **delete these two lines**:
 
```python
from google.colab import userdata
os.environ["GROQ_API_KEY"] = userdata.get("Groq_key")
```
 
The notebook will now read the key from the `GROQ_API_KEY` you set in step 4.
 
**6. Start Jupyter and run**
 
```bash
jupyter notebook
```
 
Open `trip_planner_agent_team.ipynb` and run the cells from top to bottom (**Run → Run All Cells**).
 
> Important: start Jupyter from the **same terminal** where you set `GROQ_API_KEY`. If you open a new terminal, set the key again.
 
---
 
## How to use it
 
Run the cells in order. The test at the bottom shows three runs:
 
| Run | What happens |
|---|---|
| **Run 1** | The agents plan the trip, then the graph **stops** at `approval` and shows you the plan. |
| **Run 2** | You send feedback (for example: `Add one cheap restaurant to each day.`). The plan is rewritten. |
| **Run 3** | You answer `yes`. The trip is "booked" (pretend) and the graph ends. |
 
When the graph is waiting for you, you can answer in three ways:
 
- `yes` → books the trip
- `no` → cancels the trip
- **anything else** → counted as feedback, and the plan is rewritten (maximum 3 revisions)
To plan your own trip, change the `request` text inside `test_input`. Try these cities, because the weather tool knows only: **Lahore, Karachi, Murree**. The currency tool supports **USD, EUR, GBP**.
 
The conversation is saved in a file called `trip_planner.db`. This file is **deleted and created fresh** each time you run the build cell, so the test always starts clean.
 
## Common problems
 
| Problem | Fix |
|---|---|
| `ModuleNotFoundError: google.colab` | You are running locally. Do Option B, step 5. |
| `GroqError: api_key must be set` or authentication error | The key is missing or wrong. Set `GROQ_API_KEY` again (or check the Colab secret name is `Groq_key` with Notebook access on). |
| `ModuleNotFoundError` for another package | Run `pip install -r requirements.txt` again inside your virtual environment. |
| Rate limit error from Groq | Wait a minute and run the cell again (the free plan has limits). |
| `KeyError` or weird state after changing code | Run all cells again from the top so the graph is rebuilt. |
 
## Note
 
All weather data and currency rates are **fake**, and `book_trip` only prints a message. This project is for learning.
 
