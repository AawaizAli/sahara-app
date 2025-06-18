---

# Sahara: Voice-Based Medical Triage App

Sahara is a multilingual voice-based medical triage app built to help reduce the influence of unlicensed medical practitioners ("quacks") in rural areas of Pakistan. It connects users to verified doctors through an AI-powered voice assistant that works in English, Urdu, and Sindhi.

---

## 🧠 Project Overview

- **Voice Bot**: Handles speech-to-text, NLP triage, and text-to-speech for medical queries.
- **Mobile App**: Two separate apps for Android (Kotlin) and iOS (Swift).
- **Backend API**: Interfaces between bot logic, mobile clients, and the database.
- **Goal**: Scale to production with proper CI/CD, multilingual support, and scalability.

---

## 🔧 Tech Stack

### Backend
- **Framework**: FastAPI (Python)
- **Database**: PostgreSQL
- **ORM**: SQLAlchemy or SQLModel
- **Auth**: JWT (JSON Web Tokens)
- **Storage**: Amazon S3 or Cloudinary (for voice files)
- **Testing**: Pytest
- **Containerization**: Docker + Docker Compose
- **Deployment**: Railway / Render (for MVP), AWS/GCP (future scale)

### Android App
- **Language**: Kotlin
- **Architecture**: MVVM
- **Networking**: Retrofit + OkHttp
- **DI**: Hilt
- **Audio**: MediaRecorder + AudioManager

### iOS App
- **Language**: Swift
- **Architecture**: MVVM + Combine/SwiftUI
- **Networking**: URLSession or Alamofire
- **Audio**: AVFoundation

### Voice Bots
- **Language**: Python
- **Speech-to-Text**: Whisper / Vosk / DeepSpeech
- **Text-to-Speech**: gTTS / pyttsx3 / Coqui-TTS
- **Multilingual Handling**: langdetect / fastText
- **ML**: scikit-learn / HuggingFace Transformers

---

## 📁 Folder Structure

```

/sahara-app
├── backend/              # FastAPI server code
├── bots/                 # Individual bot projects
│   ├── aawaiz/
│   │   ├── main.py
│   │   └── requirements.txt
│   ├── ashhal/
│   ├── umer/
│   └── shuja/
├── android-app/          # Kotlin-based Android project
├── ios-app/              # Swift-based iOS project
├── docker-compose.yml
├── README.md

````

---

## 🧪 Setting Up

### 1. Backend
```bash
cd backend
pip install -r requirements.txt
uvicorn app.main:app --reload
````

### 2. Bot (Any Member)

```bash
cd bots/aawaiz
pip install -r requirements.txt
python main.py
```

### 3. Android

* Open in Android Studio
* Sync Gradle
* Run on emulator or device

### 4. iOS

* Open in Xcode
* Trust team certificate
* Run on simulator or device

---

## 👥 Bot Development Rules

* Each member works inside their own folder inside `/bots/`:

  * `main.py`: Entrypoint for your voice bot logic
  * `requirements.txt`: Declare all dependencies needed to run your bot

* All bots should eventually expose a simple callable function:

  ```python
  def run_bot(audio_file_path: str) -> str:
      """Takes audio input and returns response text"""
  ```

---

## 🌿 Branching Strategy

| Branch      | Purpose                                      |
| ----------- | -------------------------------------------- |
| `main`      | Production-ready code only                   |
| `dev`       | Main development and feature staging         |
| `feature/*` | Optional features or experiments (temporary) |

### Git Rules

* No one commits directly to `main`
* All PRs must go from `dev` → `main` and be reviewed by at least 1 other member
* Frequent merges from `dev` to keep in sync
* Use clear commit messages

---

## 🚀 Deployment Rules

* Deployment will only happen from the `main` branch
* Use `.env` files and secrets manager for API keys
* Use Docker Compose for deploying backend + any services (e.g., PostgreSQL)
* CI/CD setup to be added using GitHub Actions
* Use `vercel.json`, `render.yaml`, or `railway.json` if using PaaS

---

## 📌 Future Enhancements

* WebSocket-based real-time voice relay
* Emotion recognition from voice
* Automatic doctor recommendation using patient history
* Centralized logging and analytics with ELK stack

---

## 🧾 Licensing & Credit

This is an academic project by:

* Aawaiz
* Ashhal
* Umer
* Shuja

MIT License

---
