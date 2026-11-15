# JARVIS — Hybrid AI Desktop Assistant

A Windows desktop assistant I built to explore what it takes to move from **AI conversation** to **real computer actions**.

JARVIS combines cloud and local AI, live voice, browser/desktop automation, memory, permissions and audit logging behind a cinematic Qt interface. The project became a practical experiment in a much harder problem than chatbot UX: making an assistant act on a computer **reliably and safely**.

## What I built

- **Hybrid AI routing** using Gemini for cloud/live interactions with a local Ollama/Qwen fallback
- **Live voice pipeline** with Gemini Live, plus local faster-whisper transcription and Piper text-to-speech
- **Wake-word detection** and compact/cinematic desktop interfaces built with PySide6 + QML
- **Browser automation** through Playwright
- **Windows and file tools** for controlled desktop actions
- **Permission / approval layers** before higher-risk actions
- **SQLite-backed memory and document search**
- **Audit logging, credential redaction and emergency-stop controls**

## Architecture

```text
Voice / typed input
        |
        v
Agent + provider router
   |            |
Gemini       Ollama/Qwen
   |
   v
Validated tool requests
   |
Permission / approval engine
   |
   +--> Browser automation
   +--> Windows actions
   +--> Files / documents / shell
   |
Audit log + memory
```

The UI displays assistant state, but tools are not executed directly from model output. Requests pass through validation and permission checks first.

## What worked best

The strongest part of the project was the conversational/voice experience. With Gemini connected, live responses became much faster, while the local Qwen path gave me an offline fallback.

I also tested repeated browser workflows. Tasks I repeated and refined could become significantly more reliable — including interacting with web-based tools such as Lovable — but this exposed the difference between a workflow that works repeatedly in one environment and a **general-purpose agent that works everywhere**.

## Why I stopped developing it

The biggest limitation was not conversation. It was **reliable action**.

Real websites introduced problems such as changing page structures, authentication, anti-bot restrictions and brittle browser steps. That made it difficult to give JARVIS broad access without either frequent failures or unacceptable permissions.

That was one of the most useful lessons from the project:

> An AI understanding an instruction is much easier than an AI safely and consistently carrying it out in the real world.

## Main technologies

Python 3.11 · PySide6 / Qt Quick / QML · Gemini · Ollama/Qwen · faster-whisper · Piper · openWakeWord · Playwright · SQLite · pywinauto / pywin32 · pytest

## Current state

This repository is a cleaned portfolio copy of the original project.

Important limitations:

- model weights and private runtime data are intentionally excluded
- some original installation paths still reference `C:\JarvisAI`
- local voice requires separate model/runtime setup
- scheduling exists as a component but was not integrated into full end-to-end assistant execution
- not every provider adapter present in source was wired into the active application

See [`PROJECT_REPORT.md`](PROJECT_REPORT.md) for the detailed source inspection and known limitations.

## Run locally

Windows + Python 3.11 are required.

Before enabling automation, review and update the paths in `config/tool_policy.yaml` so they point to your own working copy.

```powershell
uv sync --frozen --all-groups
ollama pull qwen3:8b
.\.venv\Scripts\playwright.exe install chromium
.\.venv\Scripts\python.exe -m piper.download_voices --data-dir .\models\piper en_GB-alan-medium
.\.venv\Scripts\python.exe -m app.main --safe-start
```

Additional wake-word model files are required for local wake detection.

## My contribution

I defined the assistant's requirements and tool boundaries, chose and integrated the model and automation components, tested workflows, traced failures and iterated on the desktop experience. Coding assistants contributed substantially to implementation. The project demonstrates system integration and troubleshooting; the limitations and test coverage are detailed in the technical report.

## Security note

The portfolio export excludes browser profiles, cookies, saved credentials, runtime databases, logs, generated user files and model weights. Gemini credentials were stored through Windows Credential Manager rather than committed in source.

This project should be treated as an experimental personal assistant, not production security software.
