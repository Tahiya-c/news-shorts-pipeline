# 🎬 News Shorts Pipeline

Automated pipeline to convert long-form Bengali news videos (2–10 min) into YouTube Shorts (45–55s) using Faster-Whisper + Gemini AI.

**Performance**: ~2 minutes (GPU) | ~15–20 minutes (CPU)

---

## ✨ Features

- 🎯 **AI-Powered Selection** — Gemini API picks the best coherent 45–55s section
- 🗣️ **Bengali Transcription** — Faster-Whisper (small model) for accurate Bengali speech-to-text
- 📐 **Vertical Format** — Auto-converts to 9:16 (1080×1920) YouTube Shorts format
- 🌐 **Web UI** — Drag-and-drop upload with real-time progress tracking
- 🖥️ **CLI Support** — Direct `pipeline.py` processing for batch jobs
- 📝 **Auto Transcripts** — Generated `.txt` files showing selected segments

---

## 🚀 Quick Start

### 1. Install

```bash
git clone https://github.com/Tahiya-c/news-shorts-pipeline.git
cd news-shorts-pipeline
python -m venv venv

# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate

pip install -r requirements.txt
```

### 2. Configure

```bash
# Create your .env file
cp .env.example .env
# Add your Gemini API key — get one free at https://aistudio.google.com
```

### 3. Run

**Web UI (Recommended):**
```bash
python app.py
# Visit http://localhost:5000
```

**Command Line:**
```bash
# Drop your video into the input/ folder, then:
python pipeline.py
# Results appear in output/
```

---

## 🏗️ How It Works

```
INPUT VIDEO (2–10 min)
    ↓
[Faster-Whisper] → Bengali speech-to-text with timestamps
    ↓
[Gemini API] → Analyzes segments, selects best 45–55s section
    ↓
[FFmpeg] → Cuts video to selected timeframe
    ↓
[FFmpeg] → Converts to 9:16 vertical format
    ↓
OUTPUT → YouTube Short (.mp4) + Transcript (.txt)
```

---

## 📊 Performance

| Stage | GPU (RTX 3080) | CPU (i7) |
|-------|----------------|----------|
| Transcription | ~10s | 60–90s |
| Gemini Analysis | ~1–2s | ~1–2s |
| Video Cutting | ~3–5s | ~10–15s |
| Vertical Conversion | ~10–15s | ~30–60s |
| **TOTAL** | **~2 min** | **~15–20 min** |

---

## 🔧 Configuration

```bash
# .env (required)
GEMINI_API_KEY=AIza...

# Optional — override defaults
PIPELINE_INPUT=./input
PIPELINE_OUTPUT=./output
DEVICE=cuda          # or 'cpu'
COMPUTE_TYPE=float16 # or 'int8'
```

---

## 📁 Project Structure

```
news-shorts-pipeline/
├── pipeline.py        # Core processing engine
├── app.py             # Flask web server
├── dashboard.html     # Standalone UI (optional)
├── requirements.txt   # Python dependencies
├── .env               # Your secrets — don't commit this
├── .env.example       # Template for setup
├── .gitignore
├── uploads/           # Temp files during processing
└── outputs/           # Final shorts + transcripts
```

---

## 📦 Requirements

FFmpeg must be installed separately:
- **Windows**: [ffmpeg.org](https://ffmpeg.org/download.html) → add to PATH
- **Mac**: `brew install ffmpeg`
- **Linux**: `sudo apt install ffmpeg`

Python dependencies (`requirements.txt`):
```
faster-whisper>=0.9.1
google-generativeai>=0.3.0
flask>=2.3.0
flask-cors>=4.0.0
werkzeug>=2.3.0
requests>=2.31.0
```

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| `"No valid segments"` | Try a video with clearer Bengali audio |
| `"API key error"` | Check `GEMINI_API_KEY` in `.env` |
| `"ffmpeg not found"` | Install FFmpeg and add it to PATH |
| `"GPU not detected"` | Set `DEVICE=cpu` in `.env`, or install CUDA 11.8+ |
| `"Video too short"` | Use videos ≥ 60 seconds |

---

## 🚀 Deployment

### Local
```bash
python app.py  # http://localhost:5000
```

### Railway.app
1. Connect this GitHub repo
2. Set `GEMINI_API_KEY` as an environment variable
3. Deploy → get a public URL

---

## 📝 Web API

```
POST   /api/upload          Upload video and start processing
GET    /api/job/{job_id}    Check job status and progress
GET    /api/download/{file} Download completed short or transcript
GET    /api/jobs            List all jobs
DELETE /api/job/{job_id}    Cancel a job
```

---

## 📊 UI Demo & Sample Outputs

- **[View UI Demo](UI_demo/ModelDemo_UI.pdf)** — Web interface walkthrough
- **[Sample Output Videos](https://drive.google.com/drive/folders/1rl8WCg64T1sL45UTp-StPXgI4pydDsTi?usp=sharing)** — Production examples from ETV

*Each video was produced with different time intervals and format settings per client requirements.*

---

## 📄 License

MIT

---

**Made for Bengali content creators 🇧🇩**

**Last updated**: June 2026 | v1.1.0
