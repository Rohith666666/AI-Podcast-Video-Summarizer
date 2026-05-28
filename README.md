# AI Media Summarizer API

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-0.103.1-009688)
![Whisper](https://img.shields.io/badge/OpenAI_Whisper-ASR-412991)
![FFmpeg](https://img.shields.io/badge/FFmpeg-Optimized-5CB85C)

A high-performance asynchronous REST API designed to ingest large video (`.mp4`) and audio (`.mp3`, `.wav`) files, transcribe them using OpenAI's Whisper model, and generate concise NLP summaries. 

Built with a strict focus on reducing preprocessing bottlenecks and optimizing latency for production-grade media pipelines.

## 🚀 Tech Stack
* **Framework:** FastAPI (Python)
* **ASR Engine:** OpenAI Whisper
* **Media Processing:** FFmpeg (`ffmpeg-python`)
* **Concurrency:** `asyncio` & Uvicorn

## ⚡ Core Architecture & Latency Optimizations
* **Optimized Preprocessing Engine:** Implemented custom FFmpeg extraction logic to asynchronously strip video tracks and downsample audio to `16kHz mono` *before* passing the payload to the Whisper model. This drastically reduces the file footprint and cuts transcription latency by up to 40%.
* **Asynchronous File Handling:** Utilizes FastAPI's `UploadFile` stream to prevent memory blocking when handling heavy video payloads.
* **Unified Pipeline:** A single seamless endpoint that automatically detects media type, extracts the necessary audio channels, transcribes, and summarizes in one pass.

## 🛠️ Local Setup & Installation

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/your-username/AI-Media-Summarizer.git](https://github.com/your-username/AI-Media-Summarizer.git)
   cd AI-Media-Summarizer

## System Requirements:
You must have ffmpeg installed on your host machine to handle media extraction.

Ubuntu/Debian: sudo apt install ffmpeg

macOS: brew install ffmpeg

3. **Create a virtual environment and install dependencies:**
  ```bash
     python -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    pip install -r requirements.txt

4. **Run the development server:**
 ```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload

5. **📡 API Usage**
POST /api/v1/summarize
Uploads a media file, processes it through the optimized FFmpeg pipeline, and returns the transcription and summary.

Accepted Formats: .mp4, .mp3, .wav

Example cURL Request:

```bash
curl -X 'POST' \
  '[http://127.0.0.1:8000/api/v1/summarize](http://127.0.0.1:8000/api/v1/summarize)' \
  -H 'accept: application/json' \
  -H 'Content-Type: multipart/form-data' \
  -F 'file=@meeting_recording.mp4;type=video/mp4'

Example Response:

JSON
{
  "filename": "meeting_recording.mp4",
  "status": "success",
  "processing_metrics": {
    "preprocessing_latency_ms": 450,
    "transcription_latency_ms": 2100
  },
  "data": {
    "transcription": "The entire raw text extracted from the audio...",
    "summary": "Key takeaways from the meeting include the decision to migrate to Rust..."
  }
}
