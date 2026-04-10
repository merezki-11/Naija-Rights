# 🇳🇬 Naija Rights: Your AI-Powered Pocket Constitution

**Naija Rights** is an end-to-end, full-stack Retrieval-Augmented Generation (RAG) application designed to democratize access to the Nigerian Constitution. 

By translating dense, complex legal jargon into relatable, localized Nigerian Pidgin, the application provides the public with a highly accessible conversational interface to understand their fundamental rights.

![Naija Rights Banner](https://img.shields.io/badge/Status-Live-success) ![Python](https://img.shields.io/badge/Backend-FastAPI-blue) ![LLM](https://img.shields.io/badge/AI-Google_Gemini-orange) ![Vercel](https://img.shields.io/badge/Deployed_on-Vercel-black)

## 📌 Key Features

- **Conversational RAG Architecture:** Employs a robust Retrieval-Augmented Generation pipeline to fetch hyper-relevant constitutional clauses and generate accurate, context-aware answers.
- **"Street-Smart" AI Persona:** Aggressively enforces a unique prompt structure that responds exclusively in natural Nigerian Pidgin, delivering short, punchy, and highly relatable legal advice.
- **Format Citations:** Every AI response strictly includes exact citations mapping back to the Nigerian Constitution (e.g., `📌 Source: Chapter IV, Section 35(3)`).
- **Responsive "Glassmorphism" Frontend:** A lightweight, vanilla HTML/CSS/JS frontend styled with a modern frosted-glass aesthetic. Fully optimized dynamically to avoid view-port clipping on ultra-narrow mobile screens.
- **API Rate-Limit Bypass:** Implements localized `all-MiniLM-L6-v2` embeddings via ChromaDB during the data ingestion phase, specifically architected to prevent API rate-limit exhaustion.

## 🏗️ Technical Architecture

This repository adopts a decoupled architecture hosted via a unified monolithic codebase.

### **Backend (Deployed on Render)**
- **Framework:** FastAPI
- **LLM Engine:** Google Gemini (`gemini-2.5-flash`) via LangChain.
- **Vector Database:** ChromaDB (Persisted locally within the deployment to avoid cold-start embedding generation).
- **Embeddings:** Local HuggingFace embeddings (`all-MiniLM-L6-v2`).

### **Frontend (Deployed on Vercel)**
- **Stack:** Vanilla HTML5, CSS3, JavaScript (ES6+).
- **Design Pattern:** Fully responsive flexbox layout utilizing `100dvh` for mobile viewports.
- **Integration:** Asynchronous fetching interacting securely via CORS with the Render backend.

## 🚀 Deployment Strategy

The application spans two distinct hosting platforms to maximize speed and efficiency:

1. **Backend API (Render):** The root directory drives the FastAPI deployment. The `start.sh` entry point script intercepts the boot process, safely executing `ingest.py` (which skips if the local `chroma_db` is populated), followed by spinning up Uvicorn to host `/chat`.
2. **Frontend UI (Vercel):** The `/frontend` directory is independently hosted on Vercel as a blazing-fast static edge site, routing API calls back to the Render environment.

## 🛠️ Local Development Setup

To run this application efficiently on your local machine:

### 1. Clone & Install Dependencies
```bash
git clone https://github.com/merezki-11/Naija-Rights.git
cd Naija-Rights
pip install -r requirements.txt
```

### 2. Environment Variables
Create a `.env` file in the root directory containing your Gemini API secret:
```env
GEMINI_API_KEY=your_google_gemini_key_here
```

### 3. Ingest the Constitution Data
Run the ingestion script. This extracts the PDF content, vectorizes it using the local embedding model, and persists it to the `chroma_db` directory.
```bash
python ingest.py
```

### 4. Boot the FastAPI Server
```bash
uvicorn main:app --reload
```
The backend will be available at `http://localhost:8000`.

### 5. Launch the Frontend
Open `/frontend/index.html` directly in your browser or run a simple local HTTP server:
```bash
cd frontend
python -m http.server 3000
```

## ⚖️ Disclaimer
*Naija Rights is an AI assistant providing simplified constitutional references. It is not a substitute for official legal representation.*
