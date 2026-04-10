# Naija Rights

A full-stack Retrieval-Augmented Generation (RAG) application designed to democratize access to the Nigerian Constitution. The system translates dense legal jargon into localized Nigerian Pidgin, providing a highly accessible conversational interface for the public to uncover their fundamental rights alongside official section citations.

**Live Demo:** [our-naija-rights.vercel.app](https://our-naija-rights.vercel.app)
**Backend API:** [naija-rights.onrender.com](https://naija-rights.onrender.com/docs)

---

## Screenshots

### Welcome Screen
![Welcome Screen](images/welcome.png)

### Conversational Pidgin Interface
![Conversational Interface](images/conversational-interface.png)

### Strict Section Citations
![Section Citations](images/section-citations.png)

### Mobile View
![Mobile View](images/mobile-view.png)

---

## How It Works

The system utilizes a custom RAG pipeline to ground every AI response strictly within the Nigerian Constitution:

```text
User asks a legal question
        ↓
System embeds the query locally
        ↓
   ┌────────────────────────────────────┐
   │                                    │
   │        ChromaDB Vector Store       │
   │  (Retrieves top 8 relevant chunks  │
   │   from the Nigerian Constitution)  │
   │                                    │
   └──────────────┬─────────────────────┘
                  ↓
Prompt Engineer synthesizes chunks + user query
(Enforces street-smart Pidgin persona & citation rules)
                  ↓
       Google Gemini 2.5 Flash LLM
                  ↓
        Response returned to user
```

**Example interactions:**
- "Can police search my phone?" → System fetches and cites Chapter IV, Section 37 on privacy.
- "If they arrest me on Friday, when should I go to court?" → System fetches and cites Section 35(4) on reasonable time.
- "Can I do a peaceful protest?" → System fetches and cites Section 40 on peaceful assembly.

---

## Features

- Localized AI Persona — strictly responds in natural, conversational Nigerian Pidgin
- Official Citations — every response includes exact Chapter, Part, and Section references
- Local Embeddings — uses all-MiniLM-L6-v2 via ChromaDB natively to bypass API rate limits
- Intelligent Retrieval — fetches the top 8 most contextually relevant constitutional chunks per query
- Responsive Glassmorphism UI — premium frosted-glass aesthetic deployed on Edge networks
- Mobile Optimized — deeply adaptive layouts utilizing explicit dynamic viewport (100dvh) scaling
- Decoupled Architecture — frontend and backend separated for maximum performance and stability
- Handles Render free tier cold starts gracefully via robust frontend network catching

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | Vanilla HTML5, CSS3, JavaScript (ES6+) |
| Backend | FastAPI, Python 3.11 |
| Framework | LangChain |
| LLM | Google Gemini 2.5 Flash |
| Embeddings | all-MiniLM-L6-v2 (Local HuggingFace) |
| Vector Database | ChromaDB |
| Frontend Hosting | Vercel |
| Backend Hosting | Render |

---

## Core Components

```python
def get_relevant_chunks(query: str, top_k: int = 8) -> list[dict]:
    """Retrieves the most relevant constitutional sections from the local ChromaDB vector store."""
    # Uses sentence-transformers natively for offline semantic similarity search

def generate_answer(query: str, chunks: list, history: list) -> dict:
    """Synthesizes the retrieved chunks and forces the LLM into the Pidgin persona."""
    # Prompts Gemini 2.5 Flash to summarize the law and format strict legal citations
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/health` | Health check |
| `POST` | `/chat` | Send a legal query to the RAG system |

### POST /chat

**Request:**
```json
{
  "question": "Can police search my phone?",
  "history": [],
  "eli15": false
}
```

**Response:**
```json
{
  "answer": "How far? The constitution strictly protects your privacy. No police officer hold the right to search your phone without warrant...",
  "citations": [
    {
      "chapter": "IV",
      "part": "II",
      "section_number": "37",
      "section_title": "Right to private and family life",
      "raw_text": "The privacy of citizens, their homes, correspondence, telephone conversations..."
    }
  ]
}
```

---

## Running Locally

```bash
git clone https://github.com/merezki-11/Naija-Rights.git
cd Naija-Rights
pip install -r requirements.txt
```

Create a `.env` file:
```
GEMINI_API_KEY=your_gemini_api_key_here
```

Ingest the Knowledge Base (Constitution):
```bash
python ingest.py
```

Start the backend server:
```bash
uvicorn main:app --reload
```

API runs at `http://127.0.0.1:8000`
Interactive docs at `http://127.0.0.1:8000/docs`

Launch the frontend interface:
```bash
cd frontend
python -m http.server 3000
```

---

## Project Structure

```text
Naija-Rights/
│
├── frontend/            # Vercel-hosted Vanilla JS/CSS/HTML web client
│   ├── index.html       # Glassmorphic UI template
│   ├── style.css        # Responsive styling rules
│   └── app.js           # API integration and DOM updates
│
├── main.py              # FastAPI application and endpoints
├── retriever.py         # ChromaDB querying logic
├── generator.py         # Gemini LLM and prompt engineering pipeline
├── ingest.py            # PDF text extraction and offline embedding pipeline
├── requirements.txt     # Pinned Python dependencies
├── start.sh             # Deployment boot script
└── README.md
```

---

## Author

**Macnelson Chibuike**
- GitHub: [@merezki-11](https://github.com/merezki-11)
- LinkedIn: [Macnelson Chibuike](https://www.linkedin.com/in/macnelson-chibuike)

---

## License

This project is open source and available under the [MIT License](LICENSE).
