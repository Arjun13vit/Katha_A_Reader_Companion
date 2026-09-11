# Katha — A Reader's Companion 📖

> Turn any PDF into an immersive, AI-assisted reading experience.

Katha transforms static PDF documents — novels, textbooks, research papers — into an interactive, book-like reading experience. Read leisurely with a flippable book UI and ambient soundscapes, or switch to Academic Mode where an AI assistant summarizes, explains, and narrates content on demand.

---

## ✨ Features

### Core Reading Experience
- 📖 **Flippable book UI** — PDFs rendered as a realistic page-turning book (toggle to continuous scroll mode)
- 🔍 **Zoom controls** — auto-fit on load, manual zoom in/out
- 🖼️ **Auto-generated cover page** — automatically creates a cover with the document title when the source PDF doesn't have one
- 🔖 **Bookmarks** — save and jump back to any page, persisted per user
- 📝 **Notes** — attach written notes to specific pages or highlighted passages
- ⏯️ **Resume reading** — automatically picks up where you left off
- 🎨 **Themes** — light/dark/sepia visual themes
- 🎵 **Ambient soundscapes** — manually select background audio (rain, ocean, forest, lo-fi, etc.) to set the mood while reading

### Reading Modes
- 🌿 **Leisure Mode** — a clean, distraction-free interface for novels and casual reading
- 🎓 **Academic Mode** — keeps an AI assistant on standby for study material

### AI Features (Academic Mode)
- 🧠 **Summarize** — highlight any passage and get a concise AI-generated summary
- 💡 **Explain Simply** — highlight dense or technical text and get a plain-language explanation
- 🔊 **AI Chapter Narration (TTS)** — request AI-generated audio narration of the current page or a selected passage, so you can listen instead of read

---

## 🛠️ Tech Stack

**Frontend**
- React (Vite)
- [PDF.js](https://mozilla.github.io/pdf.js/) — PDF parsing and text-layer rendering
- [page-flip](https://github.com/Nodlik/StPageFlip) — page-turn animation
- Web Audio API — ambient soundscape playback
- Axios

**Backend**
- Node.js + Express
- MongoDB (Atlas) + Mongoose
- JWT-based authentication

**AI Layer**
- LLM API (summarization / simplified explanation) via a secure backend proxy
- Text-to-Speech API for on-demand chapter/passage narration

**DevOps**
- Docker + Docker Compose for local development

---

## 📂 Project Structure

```
katha/
├── frontend/          # React app (PDF rendering, flipbook UI, AI panel)
├── backend/           # Express API (auth, bookmarks, notes, AI proxy, TTS proxy)
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   └── services/      # AI summarization + TTS integration
├── design/            # DA2: architecture/UML/pattern diagrams (.drawio + PNG) and Figma UI exports
├── docker-compose.yml
├── .env.example
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Node.js 18+ (optional, only needed for non-Docker local dev)
- A MongoDB Atlas connection string
- An LLM API key (for summarize/explain)
- A TTS API key (for chapter narration)

### Run with Docker (recommended)

```bash
git clone <repository-url>
cd katha
cp .env.example .env
# fill in MONGO_URI, LLM_API_KEY, TTS_API_KEY in .env
docker-compose up --build
```

- Frontend: [http://localhost:3000](http://localhost:3000)
- Backend API: [http://localhost:8080](http://localhost:8080)

### Run without Docker

```bash
# Backend
cd backend
npm install
npm run dev

# Frontend (new terminal)
cd frontend
npm install
npm run dev
```

---

## 🧩 Environment Variables

See `.env.example` for the full list. Key variables:

| Variable | Description |
|---|---|
| `MONGO_URI` | MongoDB Atlas connection string |
| `JWT_SECRET` | Secret used to sign auth tokens |
| `LLM_API_KEY` | API key for summarize/explain feature |
| `TTS_API_KEY` | API key for chapter narration |
| `REACT_APP_API_BASE_URL` | Backend URL used by the frontend |

---

## 🧱 Software Design (DA2)

Katha follows a layered client–server architecture — Frontend → Backend API → Service Layer → MongoDB Atlas — with an AI Proxy branch that keeps LLM and TTS vendor keys off the client, all running inside a Docker Compose boundary for local development. SOLID's Dependency Inversion Principle is applied through an `AIService` interface that `AIController` depends on, with `OpenAIProvider`, `LocalLLMProvider`, and `TTSProvider` implementing it — so swapping AI vendors never touches controller code.

**GoF Design Patterns applied:**

| Pattern | Where it's used |
|---|---|
| **Adapter** | Normalizing calls to the external LLM API and TTS API behind a common interface |
| **Facade** | `AcademicModeFacade` simplifies summarize/explain/narrate into one entry point for the frontend |
| **Strategy** | Interchangeable reading/render strategies (flipbook vs. continuous scroll) |
| **Observer** | Reading-progress sync between the `BookViewer` and `BookmarkService` |
| **Factory Method** | Generating book covers and soundscape selections |
| **Singleton** | Single shared MongoDB connection instance |

All editable diagram sources (`.drawio`) and their PNG exports — architecture diagram, UML class diagram, and design pattern map — are in [`/design`](./design).

![High-Level Architecture](design/architecture.png)
![UML Class Diagram – AI Service Layer](design/uml-class-diagram.png)
![Design Pattern Map](design/pattern-map.png)

*(Rename the paths above to match the exact filenames you uploaded into `/design` if they differ.)*

---

## 🎨 UI Design (Figma)

Six core screens were designed in Figma to cover the full reading flow — Library, Flipbook Reader, Academic Mode AI panel, Bookmarks, Notes, and Settings. Exports are included in [`/design`](./design) alongside the diagrams above.

- **Library / Home** — book grid with progress rings and a "Resume reading" banner
- **Flipbook Reading View** — two-page book layout with zoom controls
- **Academic Mode AI Panel** — highlighted text paired with Summarize / Explain / Narrate actions
- **Bookmarks** — list view of all saved bookmarks
- **Notes** — split view with a note editor alongside the page
- **Settings** — theme selection and Leisure/Academic mode toggle

---

## 🗺️ Roadmap

- [x] PDF upload + flipbook rendering
- [x] Bookmarks & auto-cover generation
- [x] Software design: architecture, UML, and pattern diagrams (DA2)
- [x] UI design: Figma screens for all core flows (DA2)
- [ ] Notes tied to highlights
- [ ] Zoom controls
- [ ] Leisure / Academic mode toggle
- [ ] AI summarize & explain
- [ ] AI chapter narration (TTS)
- [ ] Docker Compose full stack
- [ ] OCR support for scanned PDFs *(future)*
- [ ] Read-along synced-highlight narration *(future)*
- [ ] Offline/PWA support *(future)*

---

## 👥 Contributors

- **ARJUN** — Backend (Express, MongoDB, AI/TTS integration)
- **ARUNADITYA RAGURAMAN** — Frontend (React, PDF.js, UI/UX)

---

## 📄 License

This project is developed as part of an academic Software Engineering course assignment.
