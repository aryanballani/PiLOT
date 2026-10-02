# PiLOT

> A voice-first AI copilot that helps people complete tasks on their Android phone—by understanding the screen, planning the work, taking actions, and keeping the user informed.

Built for the **IEEE UBC Engineering Design Team Competition 2026** · *A Better Tomorrow* software track.

[Watch the demo](https://drive.google.com/drive/folders/1FqgYpMiQj_5vOK4IQp4g-8P_Rgs6xoPw?usp=sharing) · [View the pitch deck](https://docs.google.com/presentation/d/1pSE42yMQI_wxztAGyn1mliVlwJy3WxxmoOWjArlYDO0/edit?slide=id.p1#slide=id.p1) · [Design board](https://excalidraw.com/#room=02e1f9c73fd1451035de,46S9NwkHy9VtV6IGWPz9Qg)

## The idea

Modern phones are powerful, but routine multi-step tasks can still be cumbersome—especially when navigating unfamiliar interfaces or when hands-free help would make a meaningful difference. PiLOT turns a spoken request into a guided, observable task loop:

1. **Listen** — captures a voice request on-device.
2. **Plan** — translates the request into a short, structured sequence of goals.
3. **See** — reads the current Android UI through an accessibility service.
4. **Act & verify** — performs one deliberate action at a time, then checks whether the screen changed as expected.
5. **Keep you in the loop** — uses a clear visual glow and spoken status updates, requesting confirmation for sensitive moments.

## Why PiLOT is different

PiLOT is designed around **predictability**, not just autonomy. Its multi-agent system uses LLMs selectively for language understanding while keeping the action rail deterministic wherever possible: UI normalization, action scoring, gesture execution, retry logic, confirmations, and status feedback all follow explicit rules. That makes the prototype easier to reason about, test, and demo.

```text
Voice request
     ↓
Android companion app ──→ FastAPI orchestration server
     ↑                         ↓
Accessibility “Eyes” ← Planner → Actor → Verifier
     ↓                         ↓
On-screen actions  ←── structured next action + status
```

## Architecture

| Layer | Responsibility |
| --- | --- |
| **Android app** | Jetpack Compose interface, speech input/output, accessibility-powered UI inspection and actions, plus the floating status overlay. |
| **Orchestrator** | Owns the task state machine, tracks history, advances steps, and applies recovery policies. |
| **Planner** | Creates a concise, structured plan from a voice transcription. |
| **Actor** | Matches the current UI to the task objective and returns the next action: tap, type, scroll, back, open app, or wait. |
| **Verifier** | Checks before/after UI state to determine whether a step succeeded, failed, or needs user input. |
| **Safety & feedback** | Confirmation-aware flow, deterministic spoken templates, and a visual glow that communicates listening, working, completion, and errors. |

### Deterministic action rail

The system is deliberately hybrid. Planning and semantic verification can use an LLM, while the operational loop remains rule-driven:

- Canonicalized, filtered Android UI trees instead of opaque screen guesses.
- Deterministic element scoring: exact text, keyword match, resource ID, content description, then clickability.
- A two-step typing policy: focus a field before entering text.
- Explicit task states: `IDLE → LISTENING → PLANNING → EXECUTING → VERIFYING → DONE`.
- Bounded recovery: retry → scroll → back → wait → vision fallback → ask the user.
- Deterministic confirmation vocabulary and visual states for clear human oversight.

## Tech stack

- **Android:** Kotlin, Jetpack Compose, Android Accessibility Service, SpeechRecognizer, TextToSpeech, Hilt, Ktor
- **Backend:** Python, FastAPI, Pydantic, Uvicorn
- **AI:** Groq-backed language models, with optional Ollama support
- **Design:** Excalidraw and a rapid-prototyping workflow built for an 8-hour competition

## Run locally

### 1. Start the backend

```bash
cd pilot-backend
python -m venv .venv
source .venv/bin/activate
pip install -r ../requirements.txt
```

Create a `.env` file in `pilot-backend/` with the configuration expected by `config.py`, including a `GROQ_API_KEY`. Then run:

```bash
python main.py
```

The API starts on port `8000` by default. A `GET /health` endpoint confirms it is running.

### 2. Configure and run the Android app

1. Open this repository in Android Studio.
2. Update `SERVER_URL` in `app/build.gradle.kts` to the reachable IP address of the machine running the backend.
3. Build and install on an Android device running API 30 or later.
4. Enable PiLOT’s accessibility service and overlay permissions when prompted.
5. Tap the floating PiLOT button and speak a task.

> **Prototype note:** PiLOT uses Android accessibility and overlay permissions to inspect and interact with the active interface. Only run it on a device and applications you are authorized to control.

## API at a glance

| Endpoint | Purpose |
| --- | --- |
| `POST /task/start` | Begins a task from a voice transcription and returns the plan. |
| `POST /task/screen` | Submits the latest UI tree and receives the next action. |
| `POST /task/verify` | Validates an executed action against before/after UI state. |
| `POST /task/user-response` | Handles confirmations, cancellations, and other spoken responses. |
| `POST /task/cancel` | Stops the active task. |
| `POST /agent/step` | Provides a compact, stateless loop for rapid prototyping. |

## Project highlights

- Delivered a working Android-to-server AI automation loop under competition constraints.
- Combined accessible voice input, UI automation, and live feedback into one cohesive interaction model.
- Designed contracts between specialized agents so each stage has a clear, inspectable responsibility.
- Prioritized deterministic execution and bounded fallbacks to make agent behavior safer and more reproducible.

## Team contributions

PiLOT was built collaboratively. In particular, **Aryan Ballani** drove the reliability-oriented multi-agent work: introducing deterministic agent nodes, formalizing the end-to-end agent contracts and fallback rail, integrating the frontend with the backend, and resolving the rebase/integration issues that brought the prototype together for the final push.

## Team

- Aryan Ballani
- Kanish Khanna
- Vivaan Wadhwa

## Competition

PiLOT was created for the [IEEE UBC Engineering Design Team Competition](https://events.vtools.ieee.org/m/544706), an 8-hour engineering challenge held at UBC on March 21, 2026.
