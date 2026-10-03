# PiLOT

> A voice-first AI copilot for completing tasks on Android: it listens, sees the interface, plans the work, acts, and verifies progress.

[![Watch the demo](https://img.shields.io/badge/Watch%20the-demo-7C3AED?style=for-the-badge&logo=google-drive&logoColor=white)](https://drive.google.com/drive/folders/1FqgYpMiQj_5vOK4IQp4g-8P_Rgs6xoPw?usp=sharing)
[![View the pitch](https://img.shields.io/badge/View%20the-pitch%20deck-FF6B35?style=for-the-badge&logo=google-slides&logoColor=white)](https://docs.google.com/presentation/d/1pSE42yMQI_wxztAGyn1mliVlwJy3WxxmoOWjArlYDO0/edit?slide=id.p1#slide=id.p1)

<a href="https://drive.google.com/drive/folders/1FqgYpMiQj_5vOK4IQp4g-8P_Rgs6xoPw?usp=sharing"><img src="pilot_screenshot.png" alt="PiLOT running on Android" width="300" /></a>

Built for the **IEEE UBC Engineering Design Team Competition 2026** · *A Better Tomorrow* software track.

## How it works

```text
Voice request → Android app → Planner → Actor → Verifier
                    ↑                         ↓
            Accessibility service ← next action + status
```

PiLOT combines an Android accessibility service with a FastAPI multi-agent backend. It turns a spoken goal into a plan, reads the live UI, takes one action at a time, and confirms that the screen changed as expected.

**Designed for predictability:** LLMs handle language where useful; UI normalization, action scoring, retries, confirmations, and live feedback stay deterministic.

## Stack

`Kotlin` · `Jetpack Compose` · `Android Accessibility Service` · `FastAPI` · `Pydantic` · `Groq` · `Ollama`

## Run it

```bash
cd pilot-backend
python -m venv .venv && source .venv/bin/activate
pip install -r ../requirements.txt
# Add GROQ_API_KEY to pilot-backend/.env
python main.py
```

Open the project in Android Studio, set `SERVER_URL` in `app/build.gradle.kts` to your backend’s reachable IP, install on an Android 11+ device, and enable PiLOT’s accessibility and overlay permissions.

## Team

Aryan Ballani · Kanish Khanna · Vivaan Wadhwa · Apram Ahuja

- **Aryan:** deterministic multi-agent workflow, architecture contracts, and frontend/backend integration.
- **Apram:** primary pitch development and integration support.

> Prototype note: PiLOT uses Android accessibility and overlay permissions; run it only on devices and apps you are authorized to control.
