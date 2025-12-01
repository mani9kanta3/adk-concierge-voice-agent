# Google Calendar & Maps Integration for ADK Voice Assistant



-----

# 🎙️ Atlas: Your ADK Voice Calendar & Concierge Assistant

Welcome to **Atlas**, your personal AI voice concierge. This project isn't just a chatbot; it's a fully functional ADK agent that can listen to your voice, understand what you need, and both manage your Google Calendar in real time and find nearby places using Google Maps/Places (cafés, restaurants, petrol bunks, and more).

Think of this project as a robot secretary + local guide that lives inside your computer. You talk to it, and it keeps your schedule organized while also helping you discover and navigate places around you.


[Image of AI voice assistant conceptual diagram]

-----

## 🧩 How It Works (The Big Picture)

This project uses the **ADK (Agent Development Kit)** and the **Gemini 2.0 Flash** model. It works in a loop like this:

1. **Input:** You speak to the website (or type a message).
2. **Streaming Connection:** Your voice/text is sent to Atlas through a WebSocket — like a live, always-open phone line.
3. **Agent Thinking:** Atlas listens, understands what you want, and decides:
   - Does this request involve your Google Calendar?
   - Is this a concierge request (e.g., “find a good café”)?
   - Or is it a general question?
4. **Action via Tools:**  
   - For calendar tasks: Atlas uses its Calendar Tools  
   - For location/concierge tasks: Atlas uses the Google Places Tool  
   - For normal chat: Atlas replies directly
5. **Response:** Atlas speaks back with a smooth, natural voice and confirms what it did (schedule, edit, find, suggest, etc.).

Atlas = Calendar Manager + Local Guide + Voice Assistant.

[Image of client server AI architecture diagram]

-----

## 🛠️ What Can Atlas Do? (The Capabilities)

Atlas has access to several **Tools** (Python functions) that allow it to take real actions—not just answer questions.  
With these tools, Atlas can manage your schedule and also help you find places around you.

### 📅 Calendar Management
- **🗂️ List Events:** Ask Atlas what’s on your schedule today, tomorrow, or next week.
- **➕ Create Events:** Say “Schedule a meeting tomorrow at 5 PM,” and Atlas will add it instantly.
- **✏️ Edit Events:** Need to move a meeting? Atlas updates it for you.
- **🗑️ Delete Events:** Plans changed? Atlas removes events from your Google Calendar.

### 🗺️ Concierge / Local Recommendations
- **🍽️ Find restaurants, cafés, or places near any location you mention.**
- **⛽ Search petrol bunks, ATMs, gyms, malls, tourist spots, etc.**
- **⭐ Get ratings, addresses, and top suggestions directly from Google Places.**

Atlas = Your **calendar manager + local guide + voice assistant**, all in one.


-----

## 📂 Project Structure (The Files)

Here is how the codebase is organized. Think of the folder like a human body that works together:

---

### 1. `app/jarvis/agent.py` (The Brain) 🧠

This file contains the **Instructions** and the **Agent configuration**.

- It tells the AI: “You are Atlas — a voice-powered calendar + concierge assistant.”
- It lists all available tools: Calendar tools + the new LocationTool.
- It defines the model used: **Gemini 2.0 Flash Experimental** (chosen for real-time voice understanding).
- It decides when to call which tool (calendar request vs. local search request).

---

### 2. `app/jarvis/tools/` (The Hands) ✋  
This directory contains all the **Tools** that Atlas can use.

#### `tools.py`
- Connects to Google Calendar.
- Implements: `list_events`, `create_event`, `edit_event`, `delete_event`.
- Converts Google dates into easy formats the agent can understand.

#### `location_tool.py` (NEW: Concierge Tool)
- Connects to **Google Places API**.
- Finds restaurants, cafés, petrol bunks, ATMs, tourist attractions, etc.
- Returns structured JSON that Atlas summarizes naturally.

These tools are Atlas’s “hands” — they let the agent take real-world actions.

---

### 3. `app/main.py` (The Nervous System) ⚡

This is the bridge between your browser and the AI agent.

- Runs the web server using **FastAPI**.
- Manages the **WebSocket** (live, always-on connection).
- Streams audio from browser → agent and agent → browser.
- Handles real-time responses so Atlas feels instant and conversational.

---

### 4. `static/` (The Face / UI) 🎧  
Contains:

- `index.html` → The interface
- `app.js` → Controls audio input/output + WebSocket
- CSS/JS files for the frontend

This is the “face” that the user interacts with.


-----
## 🚀 How to Run This Project

### 🧱 Prerequisites

Before running Atlas, ensure you have:

- **Python 3.10+**
- **Google Cloud Account**
- **Google Calendar API enabled**
- **Google Places API enabled**
- A `.env` file containing:
```bash
GEMINI_API_KEY=your_gemini_key
GOOGLE_MAPS_API_KEY=your_places_api_key
 ```


### Step-by-Step Setup

1.  **Download Credentials:**
    You need to create a project in Google Cloud, enable the Calendar API, and download your `credentials.json` file. This is like giving Atlas the key to your office.

2.  **Install Dependencies:**
    We use `uv` or `pip` to install the requirements found in `pyproject.toml`.

    ```bash
    pip install -r requirements.txt
    ```

3.  **Authenticate Locally:**
    Run the setup script to log in to Google on your machine.

    ```bash
    python setup_calendar.py
    ```

4.  **Run the App (Phase 1 - Testing):**
    You can test quickly using the ADK web interface.

    ```bash
    adk web
    ```

5.  **Run the Full App (Phase 2 - Production):**
    To see the custom website with the voice interface:

    ```bash
    uvicorn app.main:app --reload
    ```

-----

## ⚠️ Important Notes

  * **The Model:** We use `Gemini 2.0 Flash Experimental`. Why? Because standard models are sometimes too slow for real-time voice, or they don't support "Native Audio" (listening to raw sound files).
  * **Safety:** The agent is instructed to be careful. For example, if you ask to delete an event, it looks for the specific ID of that event first to make sure it doesn't delete the wrong thing.

[Image of Python WebSocket real-time communication diagram]

-----

## 🎓 What You Are Learning

By building this, you are learning:

1. **Multimodal AI:** How AI handles text and audio together in real time.  
2. **Tool Calling:** How to make AI trigger real Python functions safely.  
3. **WebSockets:** How to build real-time apps that stream voice without page reloads.  
4. **API Integration:** How to connect Python to Google Services like Calendar.  
5. **External Data Tools:** How to integrate Google Places API for concierge-style local searches.
\
This document explains how to set up and use the Google Calendar and Google Places integrations with your ADK Voice Assistant.

## Setup Instructions

### 1. Install Dependencies

First, create a virtual environment:

```bash
# Create a virtual environment
python -m venv .venv

uv venv --python 3.12

.venv\Scripts\activate

#cmd- .venv\Scripts\activate.bat

#apple- source .venv/bin/activate 
```

Activate the virtual environment:

On Windows:
```bash
# Activate virtual environment on Windows
.venv\Scripts\activate
```


python --version - check instalation

On macOS/Linux:
```bash
# Activate virtual environment on macOS/Linux
source .venv/bin/activate
```

Then, install all required Python packages using pip:

```bash
# Install all dependencies
pip install -r requirements.txt
```

### 2. Set Up Gemini API Key

1. Create or use an existing [Google AI Studio](https://aistudio.google.com/) account
2. Get your Gemini API key from the [API Keys section](https://aistudio.google.com/app/apikeys)
3. Set the API key as an environment variable:

Create a `.env` file in the project root with:

```
GOOGLE_API_KEY=your_api_key_here
```

### 3. Create a Google Cloud Project

1. Go to the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Enable the Google Calendar API for your project:
   - In the sidebar, navigate to "APIs & Services" > "Library"
   - Search for "Google Calendar API" and enable it
   - Again search for "Places API(New) & Places API" and enable it

### 4. Create OAuth 2.0 Credentials

1. In the Google Cloud Console, navigate to "APIs & Services" > "Credentials"
2. Click "Create Credentials" and select "OAuth client ID"
3. For application type, select "Desktop application"
4. Name your OAuth client (e.g., "ADK Voice Calendar Integration")
5. Click "Create"
6. Download the credentials JSON file
7. Save the file as `credentials.json` in the root directory of this project

### 5. Add Places API Key

1. In the Google Cloud Console, navigate to "APIs & Services" > "Credentials"
2. Click "Create Credentials → API Key"
3. Copy the generated API key
4. Add the key to your `.env` file:

```
GOOGLE_API_KEY=your_api_key_here
GOOGLE_MAPS_API_KEY=your_places_api_key_here
```

### 5. Run the Setup Script

Run the setup script to authenticate with Google Calendar:

```bash
python setup_calendar_auth.py
```

This will:
1. Start the OAuth 2.0 authorization flow
2. Open your browser to authorize the application
3. Save the access token securely for future use
4. Test the connection to your Google Calendar

## Working with Multiple Calendars

The system supports managing **multiple Google Calendars** under your account.  
During OAuth authorization, the assistant receives permission to access all calendars linked to your Google profile.

You can:

1. **List all calendars** (personal, work, shared, etc.)
2. **Choose a specific calendar** for adding, editing, or deleting events
3. Allow the assistant to **default to your Primary Calendar** if none is specified

### Example Commands:
- "Show me all the calendars I have."
- "Add this event to my Work calendar."
- "What's scheduled on my Family calendar this weekend?"
- "Create a reminder in my Personal calendar."

---

## Using the Calendar Assistant (Voice / Text)

After setup, you can control your Google Calendar naturally through voice or text.  
The AI agent interprets your request, decides which tool to call (read, write, modify), and updates your calendar in real-time.

### Supported Calendar Operations

#### 📅 View & Query Events:
- "What's on my schedule today?"
- "Show my appointments for next week."
- "Do I have any meetings tomorrow afternoon?"

#### ➕ Create Events:
- "Create a meeting with John tomorrow at 2 PM."
- "Schedule a doctor's visit next Friday at 10 AM."
- "Add a 'gym workout' event at 6 AM on Monday."

#### ✏️ Modify Events:
- "Reschedule my meeting with Sarah to Thursday at 11 AM."
- "Move my 4 PM call to 5 PM."
- "Change my 'dentist appointment' to 'Dental Cleaning'."

#### 🗑️ Delete Events:
- "Cancel my 3 PM meeting today."
- "Remove the team sync from my calendar."

---

## 🧭 Concierge Features

With the **Google Places API** added, the assistant can now act as a **Concierge Agent**, helping you with location-based queries.

### Examples:
- "Find a good Italian restaurant near me."
- "Are there any ATMs near my office?"
- "Show me cafés within 5 kilometers."
- "Find petrol stations near Hyderabad."
- "What are some tourist attractions near my meeting location?"

The AI automatically:
- Searches Places API  
- Formats results  
- Returns top options  


## Running the Application

After completing the setup, you can run the application using the following command:

```bash
# Start the ADK Voice Assistant with hot-reloading enabled
uvicorn main:app --reload
```

This will start the application server, and you can interact with your voice assistant through the provided interface.

## Troubleshooting

### Token Errors

If you encounter authentication errors:

1. Delete the token file at `~/.credentials/calendar_token.json`
2. Run the setup script again

### Permission Issues

If you need additional calendar permissions:

1. Delete the token file at `~/.credentials/calendar_token.json`
2. Edit the `SCOPES` variable in `app/jarvis/tools/calendar_utils.py`
3. Run the setup script again

### API Quota

Google Calendar API has usage quotas. If you hit quota limits:

1. Check your [Google Cloud Console](https://console.cloud.google.com/)
2. Navigate to "APIs & Services" > "Dashboard"
3. Select "Google Calendar API"
4. View your quota usage and consider upgrading if necessary

### 🌍 Places API / Concierge Troubleshooting

If the Concierge feature returns:

- Empty results  
- “No places found”  
- “The tool is malfunctioning”  

Follow these checks:

#### ✅ 1. Verify Places API Is Enabled
Go to:
```bash
Google Cloud Console → APIs & Services → Library → Places API
```

Make sure it is **enabled** for your project.

#### ✅ 2. Ensure Billing Is Enabled
The Places API does **not work without billing**.

Check here:
```bash
Google Cloud Console → Billing
```

If no billing account is linked, attach one to your project.

#### ✅ 3. Check API Key Restrictions
If you applied restrictions, confirm:
```bash
API Restrictions → Places API (enabled)
Application Restrictions → Your current environment allowed
```


If unsure → remove restrictions temporarily to test.

#### ✅ 4. Verify Your `.env` File
Your API key must be present:
```bash
GOOGLE_MAPS_API_KEY=your_actual_key_here
```


Restart the app after editing the `.env` file.

#### 🔄 5. Restart Your FastAPI Server
```bash
uvicorn app.main:app --reload
```
This reloads environment variables and updates the running app.


### Package Installation Issues

If you encounter issues installing the required packages:

1. Make sure you're using Python 3.8 or newer
2. Try upgrading pip: `pip install --upgrade pip`
3. Install packages individually if a specific package is causing problems

## Security Considerations

- The OAuth token is stored securely in your user directory
- Never share your `credentials.json` file or the generated token
- The application only requests the minimum permissions needed for calendar operations



