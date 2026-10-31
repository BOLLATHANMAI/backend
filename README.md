# 🧠 Sophia Collaborative AI Engine (Multi-Modal Backend)

> A real-time collaborative learning and multi-modal AI orchestration microservice powering synchronous study rooms, contextual document/video synthesis with Gemini 1.5, and automated voice narration.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg?logo=python)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-2.x-lightgrey.svg?logo=flask)](https://flask.palletsprojects.com/)
[![WebSockets](https://img.shields.io/badge/Socket.IO-Realtime-black.svg?logo=socketdotio)](https://socket.io/)
[![Google Gemini](https://img.shields.io/badge/Google%20GenAI-Gemini%201.5%20Flash-orange.svg?logo=google)](https://ai.google.dev/)
[![Azure AI](https://img.shields.io/badge/Azure-Speech%20SDK-blue.svg?logo=microsoftazure)](https://azure.microsoft.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 🎯 Problem & Solution

* **The Problem:** Modern remote learning environments isolate students from interactive study groups. Reviewing fragmented study resources (lengthy YouTube lectures, lecture slide decks, dense PDF textbooks) without live multi-user collaboration leads to low comprehension and retention.
* **The Solution:** Sophia is a real-time multi-modal study engine designed for group learning. Built on Flask-SocketIO, it synchronizes collaborative rooms where peers inspect shared media, while an orchestrated AI agent leverages **Google Gemini 1.5 Flash** to answer natural language queries across ingested YouTube transcripts and PDF chapters. An **Azure Cognitive Services Speech** pipeline synthesizes audio overviews on demand.
* **Market Validation & Metric Wins:**
  * Awarded **1st Place** at the *BIG G Foundation Incubator Pitch Competition*.
  * Restructured prompt synthesis and concurrent document extraction to slash end-to-end processing latency by **50 seconds**.

---

## 🏗️ Architecture & Orchestration Flow

```mermaid
flowchart TD
    Client[Web Client / Mobile App] <-->|Socket.IO Events| SocketServer[Flask-SocketIO Server]
    Client -->|REST API Requests| RestAPI[Flask REST Endpoints]
    
    subgraph Multi-Modal Ingestion Pipeline
        RestAPI --> YT[YouTube Transcript API]
        RestAPI --> PDF[PyPDF2 Document Extractor]
    end
    
    subgraph AI Orchestration Layer
        SocketServer -->|@Ai Inquiries| ContextAgg[Context Aggregator]
        YT --> ContextAgg
        PDF --> ContextAgg
        ContextAgg --> Gemini[Gemini 1.5 Flash LLM]
        Gemini --> ResponseGen[Structured Response & Quiz Generator]
    end
    
    subgraph Voice Synthesis
        Gemini --> AzureTTS[Azure Cognitive Services Speech SDK]
        AzureTTS --> AudioFile[Generated MP3 Audio Summaries]
    end
```

---

## 🚀 Key Features

* **Synchronous Multi-User Rooms:** Real-time Socket.IO room management supporting join/leave telemetry, participant state broadcasting, and shared resource previews.
* **In-Room AI Co-Pilot (`@Ai`):** Group participants can query study materials directly within room chat. The backend extracts context from uploaded PDFs and active YouTube transcripts to supply grounded answers.
* **Automated Assessment Generation (`/api/generate-questions`):** Synthesizes 5 rigorous, content-specific conceptual questions from ingested materials for active recall.
* **AI Podcast / Audio Synthesis (`/api/generate-audio`):** Translates key document takeaways into synthesized spoken-audio lectures using Azure Speech SDK.
* **Multi-Lingual Translation:** Integrates `deep-translator` for international student support across non-native learning materials.

---

## 🛠️ Tech Stack

| Layer | Technologies |
| :--- | :--- |
| **Server Framework** | Python, Flask, Flask-CORS |
| **Real-time Networking** | Flask-SocketIO, Python-SocketIO, Eventlet/Gevent |
| **Generative AI** | Google Generative AI SDK (`gemini-1.5-flash`) |
| **Document & Video Ingestion** | `youtube-transcript-api`, `PyPDF2`, `requests` |
| **Voice & Speech Synthesis** | Azure Cognitive Services Speech SDK (`azure-cognitiveservices-speech`) |
| **Translation** | `deep-translator` |

---

## 📁 Project Structure

```
backend/
├── app.py                  # Core Flask & SocketIO application, routes & event handlers
├── requirements.txt        # Production python dependencies
├── .env.example            # Environment variable template
├── .gitignore              # Git ignore rules for virtualenvs, keys & uploads
└── README.md               # Architecture and documentation
```

---

## ⚡ Quick Start

### Prerequisites
* Python 3.10+
* Google Gemini API Key
* Azure Cognitive Services Speech Key & Region

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/BOLLATHANMAI/backend.git
   cd backend
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up Environment Variables:**
   ```bash
   cp .env.example .env
   ```
   Edit `.env` with your credentials:
   ```env
   GENAI_API_KEY=your_gemini_api_key
   SPEECH_KEY=your_azure_speech_key
   SPEECH_REGION=your_azure_speech_region
   PORT=5000
   ```

5. **Start the server:**
   ```bash
   python app.py
   ```
   Server listens on `http://localhost:5000` with WebSocket support enabled.

---

## 📡 API & Socket Event Reference

### REST Endpoints
* `POST /api/generate-questions`: Takes `{ "youtube_urls": [...], "pdf_paths": [...] }`, returns 5 contextual study questions.
* `POST /api/generate-audio`: Generates an audio lecture file from active room context.
* `GET /audio/<filename>`: Streams generated audio summaries.

### Socket.IO Events
* `join`: Join room with `{ "roomId": "...", "participant": "..." }`
* `leave`: Disconnect and update room roster
* `resource_added`: Broadcast uploaded document or video URL
* `resource_selected`: Synchronize active preview across all participant screens
* `message`: In-room chat; messages starting with `@Ai` trigger multi-modal contextual answers

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 👩‍💻 Author

**Bolla Thanmai**  
*Full-Stack & Backend Engineer*  
* [LinkedIn](https://www.linkedin.com/in/bolla-thanmai) • [GitHub](https://github.com/BOLLATHANMAI)
