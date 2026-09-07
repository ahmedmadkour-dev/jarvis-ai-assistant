# JARVIS — Project Report

## Purpose

JARVIS was a Windows desktop AI-assistant experiment focused on the gap between **conversation** and **reliable computer action**. The project combined local and cloud models, voice, browser automation, desktop tools, memory and permission controls.

## Technical stack

- Python 3.11
- PySide6 / Qt Quick / QML
- Gemini / Gemini Live
- Ollama + Qwen
- faster-whisper
- Piper TTS
- openWakeWord
- Playwright
- pywinauto / pywin32
- SQLite
- pytest, Ruff, mypy, Bandit, PyInstaller

## Architecture

The application routes typed or spoken input through an agent/provider layer. Model-generated tool requests are validated before reaching higher-risk actions such as browser, shell, Windows or file operations. The project also contains approval, audit, redaction and emergency-stop mechanisms.

Key areas in the preserved source include:

- `app/agent/` — agent/tool-calling logic
- `app/models/` — Gemini, Ollama and provider routing
- `app/voice/` — live/local speech pipeline
- `app/browser/` — Playwright browser worker
- `app/tools/` — file, document, shell and Windows actions
- `app/permissions/` and `app/security/` — action controls and credential handling
- `app/memory/` — SQLite-backed memory/indexing
- `app/ui/` — Classic and cinematic desktop interfaces

## What worked

The strongest part was the live conversational experience. Gemini Live made voice interactions much faster, while local Qwen/Ollama provided an offline path. Repeated browser workflows could also become reliable after iteration; I successfully used JARVIS for repeated web interactions including workflows with tools such as Lovable.

## Main limitation

The project became difficult when moving from a known repeated workflow to a general-purpose assistant. Real websites introduced authentication, changing interfaces, anti-bot restrictions and brittle automation steps. This made safe broad computer access much harder than natural conversation.

## Validation of the preserved copy

During portfolio cleanup:

- all 75 Python source/test files parsed successfully
- 15 selected existing tests passed
- security/redaction, permissions, memory, scheduling-data controls and configuration were included in those checks

The full live desktop/voice/browser suite was not re-run because private credentials, runtime data and model weights were deliberately excluded.

## Known limitations

- some paths still reference the original `C:\JarvisAI` installation
- model weights are not included
- wake-word assets require separate setup
- scheduling exists as a component but was not fully wired into end-to-end assistant execution
- some provider adapters exist in source but were not active in the main application

## Security / privacy

The portfolio copy excludes browser profiles, cookies, user memory databases, preferences, audit logs, generated user work and model weights. No live hardcoded project credential was identified in the cleaned source. Gemini credentials were accessed through Windows Credential Manager rather than stored directly in project files.

## AI-assisted development

I used AI coding tools extensively. I directed the product, architecture decisions, integrations, testing and debugging while AI generated and modified substantial parts of the code. This project is presented as evidence of system design, integration, experimentation and troubleshooting — not as a claim that every line was written manually.
