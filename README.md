# PipeSight AI — Agentic Sewer & Stormwater Pipe Inspection

PipeSight AI is an autonomous CCTV sewer and stormwater pipe inspection platform. It combines **OpenCV 5**, a trained **YOLO26s** defect detection model (`best.pt`), **ByteTrack** multi-object tracking, and a deterministic **Agentic Vision controller** to assess image quality, identify pipeline defects, and generate diagnostic reports.

---

## 1. Project Overview & Technologies

PipeSight AI separates perception, defect tracking, and inspection reporting into a unified repository with decoupled backend and frontend services:

- **Backend (`backend/`)**:
  - **FastAPI** ASGI web framework for asynchronous video ingestion and status polling.
  - **OpenCV 5** for sequential video frame decoding, CLAHE luminance enhancement, blur variance analysis, and HUD rendering.
  - **Ultralytics YOLO26s** (`best.pt`) for detecting 5 defect categories: *Crack*, *Debris*, *Joint Fault*, *Root Intrusion*, and *Surface Damage*.
  - **ByteTrack** for persistent multi-object tracking and candidate defect event deduplication across frames.
  - **SQLite (WAL Mode)** for zero-cost, persistent job state and report storage surviving server restarts.
  - **ReportLab** for formal engineering PDF inspection reports.
- **Frontend (`frontend/`)**:
  - **React 19 + TypeScript + Vite** inspection dashboard.
  - High-contrast industrial UI with responsive tables, video playback HUD, evidence image lightbox, and agent execution trace.
  - Download buttons for annotated MP4 video, formal PDF report, structured JSON telemetry, and complete ZIP package.
- **Deployment (`deploy/`)**:
  - Single AWS EC2 instance deployment templates using **Nginx** reverse proxy and **systemd**.
  - Cost-controlled architecture targeting < $20 for demonstration periods on `t3a.medium` CPU instances.

---

## 2. Repository Structure

```
PipeSight-AI/
├── backend/
│   ├── app/
│   │   ├── api/             # FastAPI REST endpoints
│   │   ├── services/        # Video, Vision, Agent, Storage, DB services
│   │   ├── utils/           # OpenCV 5 quality assessment, CLAHE, COOL adapter
│   │   ├── config.py        # Settings and environment variables
│   │   ├── schemas.py       # Pydantic data models
│   │   └── main.py          # Application entrypoint
│   ├── models/
│   │   └── README.md        # Instructions to place and transfer best.pt
│   ├── trackers/
│   │   └── bytetrack.yaml   # ByteTrack tracker configuration
│   ├── tests/               # Pytest automated test suite (30 tests)
│   ├── scripts/             # CPU benchmark & verification scripts
│   ├── storage/
│   │   └── .gitkeep         # Local job storage directory
│   ├── requirements.txt     # Pinned Python dependencies
│   ├── Dockerfile           # Multi-stage production container
│   ├── .dockerignore
│   ├── .env.example         # Backend environment variables template
│   └── README.md            # Detailed backend documentation
├── frontend/
│   ├── src/                 # React components, hooks, services, types
│   ├── public/              # Static public assets
│   ├── package.json         # Pinned npm dependencies
│   ├── package-lock.json
│   ├── .env.example         # Frontend environment template
│   └── README.md            # Detailed frontend documentation
├── deploy/
│   ├── nginx.conf.example   # Nginx reverse proxy & static hosting config
│   ├── pipesight.service.example  # Systemd service unit definition
│   ├── deploy_ec2.sh        # Automated Ubuntu EC2 provisioning script
│   └── README.md            # Step-by-step AWS EC2 deployment guide
├── .gitignore
├── .gitattributes
└── README.md
```

---

## 3. Prerequisites

- **Python**: Version 3.10 or 3.11.
- **Node.js**: Version 18+ (tested on Node 20 & 24).
- **npm**: Version 9+.
- **Model Checkpoint**: Your trained YOLO26s checkpoint `best.pt` placed in `backend/models/best.pt`.
- **System Libraries (Linux / Ubuntu)**: `ffmpeg`, `libgl1`, `libglib2.0-0`, `libgomp1`.

---

## 4. Model Placement

Place your trained `best.pt` weights file in the `backend/models/` directory:

```bash
# Path must be:
backend/models/best.pt
```

> **Note:** Model weights (`*.pt`) are excluded from Git via `.gitignore`. See [`backend/models/README.md`](file:///d:/Users/kerat/Downloads/OPENCV_Sewer/PipeSight_AI/backend/models/README.md) for instructions on securely transferring `best.pt` to EC2 via `scp`.

---

## 5. Local Setup & Running

### Backend Startup

1. Open a terminal in the `backend/` directory:
   ```bash
   cd backend
   ```

2. Create and activate a Python virtual environment:
   ```bash
   # Linux / macOS
   python3 -m venv venv
   source venv/bin/activate

   # Windows PowerShell
   python -m venv venv
   .\venv\Scripts\Activate.ps1
   ```

3. Install pinned dependencies:
   ```bash
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

4. Configure environment:
   ```bash
   cp .env.example .env
   ```

5. Start the FastAPI backend:
   ```bash
   uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
   ```

Backend will be available at `http://127.0.0.1:8000` (API documentation at `http://127.0.0.1:8000/docs`).

### Frontend Startup

1. Open a new terminal in the `frontend/` directory:
   ```bash
   cd frontend
   ```

2. Configure environment:
   ```bash
   cp .env.example .env
   ```
   Ensure `VITE_API_BASE_URL=http://127.0.0.1:8000`.

3. Install dependencies and start Vite dev server:
   ```bash
   npm install
   npm run dev
   ```

Frontend will be available at `http://localhost:5173`.

---

## 6. Running Tests

Execute the automated backend test suite (30 unit & integration tests):

```bash
cd backend
python -m pytest tests -v
```

Execute frontend production build check:

```bash
cd frontend
npm run build
```

---

## 7. Deployment Instructions

For complete, step-by-step instructions on deploying both frontend and backend to an **AWS EC2 Ubuntu instance** with Nginx, HTTPS, systemd, and strict cost controls (< $20), see:

👉 [**AWS EC2 Production Deployment Guide (`deploy/README.md`)**](file:///d:/Users/kerat/Downloads/OPENCV_Sewer/PipeSight_AI/deploy/README.md)

Additional component documentation:
- [Backend Architecture & API Reference (`backend/README.md`)](file:///d:/Users/kerat/Downloads/OPENCV_Sewer/PipeSight_AI/backend/README.md)
- [Frontend Inspection Dashboard (`frontend/README.md`)](file:///d:/Users/kerat/Downloads/OPENCV_Sewer/PipeSight_AI/frontend/README.md)
- [Model Weights & Checkpoint Policy (`backend/models/README.md`)](file:///d:/Users/kerat/Downloads/OPENCV_Sewer/PipeSight_AI/backend/models/README.md)
