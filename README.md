<div align="center">

# ⚡ Xenora
### eXecution Engine for Navigating Operations & Responsive Assistance

[![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![Socket.io](https://img.shields.io/badge/Socket.io-010101?style=for-the-badge&logo=socket.io&logoColor=white)](https://socket.io/)
[![Groq](https://img.shields.io/badge/Groq-Cloud_Inference-F55036?style=for-the-badge)](https://groq.com/)
[![Gemini](https://img.shields.io/badge/Gemini-Vision_API-8E75B2?style=for-the-badge&logo=google&logoColor=white)](https://deepmind.google/technologies/gemini/)
[![Ollama](https://img.shields.io/badge/Ollama-Local_DeepSeek-white?style=for-the-badge&logo=ollama&logoColor=black)](https://ollama.com/)

<p align="center">
  <b>A context-aware, multimodal local desktop AI operating system featuring ambient background vision, hybrid cloud/edge inference, low-latency neural TTS, and automated system orchestration.</b>
</p>

---

</div>

## 📌 Overview

**Xenora** is a stateful, personal desktop intelligence layer designed to eliminate operational friction from daily computing workflows. Developed iteratively over eight months of systems engineering, Xenora bridges natural-language reasoning with low-level Windows APIs, asynchronous visual monitoring, and multi-application pipeline execution.

Unlike conventional conversational AI wrappers, Xenora maintains an active spatial awareness of the desktop environment, preserves long-term user context across sessions, and executes complex multi-step deployment routines with sub-second latency.

---

## ✨ Core Capabilities

* **Multimodal Screen Vision & Ambient Tracking:** Native Windows GDI/`PrintWindow` capture enables asynchronous visual parsing of background windows without interrupting the user's active foreground tasks.
* **Hybrid Cloud/Edge Inference Engine:** Adaptive query routing powered by high-throughput cloud inference (Groq) with seamless zero-downtime failover to quantized local models (DeepSeek-R1 via Ollama) during network loss.
* **High-Throughput Desktop Orchestration:** Translates complex natural-language directives into parallel OS process launches (e.g., deploying browser workspaces, communication channels, media streams, and games simultaneously in under 5 seconds).
* **🎙️ Low-Latency Emotion-Aware Voice Engine:** On-device neural speech synthesis powered by Kokoro ONNX, featuring runtime sentiment detection to dynamically adapt cadence, tone, and prosody.
* **Non-Blocking Real-Time Architecture:** Asynchronous communication bridge using Flask and Socket.IO to stream real-time audio buffers, system telemetry, active watch-tasks, and visual feeds to an Electron/Web frontend.
* **Persistent State & Semantic Memory Engine:** Multi-tiered storage tracking user preferences, interaction intervals, and session summaries to continuously ground conversational context.

---

## 🏛️ System Architecture

```text
                             ┌───────────────────────┐
                             │   Natural Language    │
                             │   Input (Mic / UI)    │
                             └───────────┬───────────┘
                                         │
                                         ▼
                             ┌───────────────────────┐
                             │  Asynchronous Core    │
                             │  State & Dispatcher   │
                             └───────────┬───────────┘
                                         │
         ┌───────────────────────────────┼───────────────────────────────┐
         │                               │                               │
         ▼                               ▼                               ▼
┌──────────────────┐           ┌──────────────────┐            ┌───────────────────┐
│  Hybrid Brain    │           │ Desktop Vision   │            │ Hardware & OS     │
├──────────────────┤           ├──────────────────┤            ├───────────────────┤
│ Primary: Groq    │           │ PrintWindow GDI  │            │ Win32 / ctypes    │
│ Fallback: Ollama │           │ Gemini Vision    │            │ Process Spawning  │
│ Semantic Memory  │           │ Pixel-Diff Gate  │            │ Sockets / Flask   │
└────────┬─────────┘           └────────┬─────────┘            └─────────┬─────────┘
         │                               │                               │
         └───────────────────────────────┼───────────────────────────────┘
                                         │
                                         ▼
                             ┌───────────────────────┐
                             │  Kokoro ONNX Audio /  │
                             │ Native Notifications  │
                             └───────────────────────┘
```

---

## 🛠️ Key Engineering Challenges & Solutions

### 1. Zero-Impact Background Vision Tracking
* **The Challenge:** Continuously polling vision models on active desktop video feeds introduces substantial latency and rapidly consumes API token limits.
* **The Solution:** Implemented background frame capture using the Win32 `PrintWindow` API (`PW_RENDERFULLCONTENT`), operating independently of foreground window focus. A lightweight NumPy pixel-difference algorithm gates every frame, dispatching multimodal API calls only when meaningful visual state shifts occur.

### 2. Multi-Target Batching for Token Optimization
* **The Challenge:** Polling independent background tasks (such as friend presence across games or download progress bars) scales token consumption linearly with each new target.
* **The Solution:** Engineered a group-coordinator thread that crops localized dynamic regions (such as chat panes or status docks) and prompts the vision model to return structured JSON evaluating all monitored targets in a single request. This bounds API usage to scale with the number of *windows* monitored rather than the number of *targets*.

### 3. Fault-Tolerant Hybrid Routing & Hardware Awareness
* **The Challenge:** Standard software ACPI suspension commands (`SetSuspendState`) frequently produce silent failures on modern laptop platforms (Modern Standby/S0 states), while intermittent network drops interrupt cloud LLM threads.
* **The Solution:** Built defensive hardware-interception layers to guide power states reliably. Paired this with a background connection monitor that automatically switches runtime context to local Ollama models whenever latency spikes or internet access drops.

---

## 💻 Technology Stack

| Layer | Technologies & Tools |
| :--- | :--- |
| **Core Architecture** | Python 3.12, Multithreading (`threading`, `queue`), Event Loops |
| **LLM & Inference** | Groq API, Google Gemini Vision API, Ollama (DeepSeek-R1) |
| **Speech & Audio** | Kokoro ONNX, SoundDevice, SoundFile, NumPy |
| **OS & Vision Capture** | Windows GDI (`pywin32`, `win32gui`, `ctypes`), PyAutoGUI, PIL |
| **Web & Automation** | Selenium WebDriver, yt-dlp, Requests |
| **UI Transport** | Flask, Flask-SocketIO, WebSockets, REST APIs |

---

## 🔒 Repository Status

This repository serves as a **technical architecture showcase and case study** for the Xenora desktop engine. 

The core execution source code, proprietary prompt schemas, and personal integration scripts remain private to protect system integrity and personal workspace configurations.

---

<div align="center">

**Developed with precision by Kunal**  
*Showcasing local-first desktop autonomy and multimodal systems engineering.*

</div>
