# <h1 align="center">Mishka: An AI-Driven Study Companion for Enhancing Student Focus and Learning Efficiency</h1>

## Mishka AI: Core Intelligence Engine

Welcome to the core AI repository of **Mishka**. 
Mishka is a comprehensive graduation project designed to guide modern students, defeat distraction, and transform dense academic materials into gamified cosmic wisdom. 

This specific repository showcases the **AI Core** of Mishka. 

---

## System Architecture

The AI Core of Mishka is split into two specialized sub-systems that handle cognitive processing and behavioral analytics concurrently

* **The Study Companion:** A Streamlit and FastAPI-powered intelligent hub that extracts text from documents, manages persistent session memory, and interfaces with Gemini 2.5 Flash via OpenRouter to generate interactive study assets.
* **The Study Monitor:** A Streamlit client handling WebRTC video ingestion. It processes frames in a background thread to compute focus and postural ergonomics using mathematical landmark deviations.

---

## Core Features

### 1. Cognitive Synthesis Core (Study Companion)
* **Structured Generation:** Forces JSON parsing on LLM responses to generate clean, interactive Quizzes, Flashcards, and Mind Maps.
* **Persistent Session Tracker:** Uses a custom `Memory Manager` to preserve conversational arrays for ongoing chat queries.

### 2. Asynchronous Behavioral Analytics (Study Monitor)
The engine processes real-time camera frames through optimized MediaPipe Face and Pose models, running on a zero-freeze concurrency pipeline to classify the student's operational state into one of four distinct focus conditions:

* **Focused State:** The optimal baseline configuration where the student is actively working, maintaining valid baseline metrics across both tracking models.
* **Looking Away (Attention Offset - $AO$):** Triggered when facial gaze metrics deviate past safe coordinate centerlines. It tracks gaze deviation by measuring the distance of the nose tip relative to the horizontal face bounding box:
  $$AO = \frac{|x_{nose} - x_{center}|}{w_{bbox}}$$
* **Bad Posture (Posture Index - $PI$):** Flagged when skeletal shoulder coordinates slump below ergonomic thresholds. It monitors upper-body slouching via MediaPipe Pose (Complexity 0) by averaging shoulder vertical positions:
  $$PI = \frac{y_{left\_shoulder} + y_{right\_shoulder}}{2}$$
* **User Absent:** Detected immediately if zero valid facial or body vectors are registered by the camera framework.
* **Zero-Freeze Concurrency:** Decouples these heavy matrix math operations from the main UI stream entirely by utilizing a bounded background FIFO `queue.Queue` pipeline.

---

## Directory Structure & File Roles
```text
Mishka Study Companion/
├── app.py              # Central FastAPI router handling backend endpoints
├── streamlit_app.py    # Main user dashboard interface layout
├── requirements.txt    # Library registries and operational requirements
├── .env                # Private cryptographic store for API key insulation
└── modules/
    ├── file_processor.py   # Multi-format document parsing and text extractor tool
    ├── generator.py        # Core AI module storing custom tool injection schemas
    └── memory_manager.py   # Stateful session allocator and history tracker

Mishka Study Monitor/
├── main.py             # Secondary backend endpoint gateway (FastAPI)
├── app.py              # WebRTC streamer rendering the Streamlit user interface
├── study_monitor.py    # Core computer vision processing and matrix analytics class
└── requirements.txt    # Vision framework dependency tracker
```
---

## How to Run the Project

To run the full Mishka ecosystem locally, you will need to open **two separate terminal windows** (one for each application layer) and ensure your Python environment is active in both.

First, open your primary terminal, and clone the repository:
```cmd
git clone https://github.com/Mariam-Alashmawy/Mishka-AI-Core.git

```
---

### Running Part 1: The Mishka Study Companion

1. Open your first terminal window, enter the companion directory, and initialize its environment:
```cmd
cd "Mishka Study Companion"
python -m venv .venv
.venv\Scripts\activate

```


2. Install the necessary processing and AI dependencies:
```cmd
pip install -r requirements.txt

```


3. Create a `.env` file in this folder and insert your key configuration:
```text
OPENROUTER_API_KEY=your_openrouter_api_key_here
```


4. Start the FastAPI endpoints backend server:
```cmd
uvicorn app:app --reload --port 8000

```


5. In a new terminal tab (with your environment active), run the Streamlit user interface dashboard:
```cmd
streamlit run streamlit_app.py

```


*The companion dashboard will open automatically in your browser at `http://localhost:8501`.*

---

### Running Part 2: The Mishka Study Monitor

1. Open a new terminal window, navigate to the monitor directory, and create your environment then activate it:
```cmd
.venv\Scripts\activate
cd "Mishka Study Monitor"
python -m venv .venv
.venv\Scripts\activate

```


2. Install the specific computer vision requirements:
```cmd
pip install -r requirements.txt

```


*(Note: This locks your setup to mediapipe==0.10.21 and opencv-python==4.8.0.74 for core stability).*
3. Launch the FastAPI telemetry data receiver endpoints:
```cmd
uvicorn main:app --reload --port 8001

```


4. Run the main Streamlit application web interface to open your webcam:
```cmd
streamlit run app.py

```


*The vision companion dashboard will open automatically at `http://localhost:8502`.*
