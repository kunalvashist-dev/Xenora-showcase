<div align="center">

# ⚡ Xenora
### eXecution Engine for Navigating Operations & Responsive Assistance

[![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Electron](https://img.shields.io/badge/Electron-47848F?style=for-the-badge&logo=electron&logoColor=white)](https://www.electronjs.org/)
[![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![Socket.io](https://img.shields.io/badge/Socket.io-010101?style=for-the-badge&logo=socket.io&logoColor=white)](https://socket.io/)
[![Groq](https://img.shields.io/badge/Groq-Cloud_Inference-F55036?style=for-the-badge)](https://groq.com/)
[![Gemini](https://img.shields.io/badge/Gemini-Vision_API-8E75B2?style=for-the-badge&logo=google&logoColor=white)](https://deepmind.google/technologies/gemini/)
[![Kokoro ONNX](https://img.shields.io/badge/Kokoro_ONNX-Neural_TTS-00D26A?style=for-the-badge)](https://github.com/thewh1teagle/kokoro-onnx)
[![Vosk](https://img.shields.io/badge/Vosk-Offline_WakeWord-2D72D9?style=for-the-badge)](https://alphacephei.com/vosk/)

<p align="center">
  <b>A stateful, multimodal local desktop AI operating system featuring an interactive multi-window Electron HUD, ambient background vision, zero-console offline wake-word activation, hybrid cloud/edge reasoning, and parallel desktop orchestration.</b>
</p>

---

</div>

## 📌 Overview

**Xenora** is a high-throughput, context-aware personal desktop intelligence system designed to eliminate operational friction from everyday computing. Developed across eight months of iterative systems engineering, Xenora unifies natural-language intent parsing with low-level Windows APIs, persistent background screen perception, on-device neural voice synthesis, and multi-process execution pipelines.

Unlike conventional conversational wrappers, Xenora maintains active spatial awareness of desktop windows, tracks continuous state transitions, evaluates compound conditional workflows, and interfaces via a multi-window Electron HUD.

---

## ✨ System Capabilities

### 1. Multi-Window Electron Interface & HUD
* **Ambient Floating Orb:** Native-draggable HUD widget featuring real-time visual telemetry for online, offline, thinking, muted, task-complete, and shutdown countdown states.
* **Radial Geometric Pie Menu:** Custom trigonometric overlay with mathematical hit detection, dynamic sector rendering, and toggle synchronization.
* **Geospatial Intelligence Terminal:** Real-time world map layer powered by MapLibre GL and Leaflet, displaying live RSS intelligence, market charts, and automated geopolitical hotspot tracking.

### 2. Standalone Offline Wake-Word Detection
* **Zero-Console Background Daemon:** Runs silently via `pythonw.exe`, listening continuously for the custom wake-word using on-device Kaldi/Vosk speech recognition models with zero cloud egress.
* **Constrained Grammar Decoding:** Employs narrow phonetic lookup sets to spot atypical vocal triggers without competing against broader dictation models.

### 3. Ambient Screen Vision & Background Monitoring
* **Zero-Impact Window Perception:** Captures background surfaces using native Win32 `PrintWindow` APIs (`PW_RENDERFULLCONTENT`), analyzing hidden windows without shifting user focus.
* **Multi-Target Token Optimization:** Batches monitored targets within a single dynamically cropped region, returning structured JSON verdicts across all entities in a single multimodal inference call.

### 4. Unified Intent Routing & Compound Planner
* **Single-Pass Disambiguation:** Resolves complex, multi-action directives with full conversational and visual context preservation.
* **Declarative Compound Execution:** Decomposes conditional requests into sequential primitive steps with deterministic timeouts and branch execution logic.

### 5. Parallel Desktop Process Orchestration
* **Multi-App Deployment Pipelines:** Launches multi-application workspaces in parallel in under 5 seconds.
* **Self-Learning App Locator:** Traverses Start Menu structures and local application trees to locate unknown binaries dynamically.

### 6. Non-Blocking Neural Audio Synthesis
* **Asynchronous Emotion TTS:** On-device neural speech generation via Kokoro ONNX, decoupled to a dedicated worker queue to prevent main-thread latency.

---

## 🏛️ System Architecture

```text
                           ┌────────────────────────────┐
                           │   Vosk Background Daemon   │
                           │   (Offline Wake-Word)      │
                           └─────────────┬──────────────┘
                                         │ Hotkey / Voice Trigger
                                         ▼
                           ┌────────────────────────────┐
                           │   Multi-Window Frontend    │
                           │   (Orb / Pie / Intel Map)  │
                           └─────────────┬──────────────┘
                                         │ WebSockets / IPC
                                         ▼
                           ┌────────────────────────────┐
                           │   Unified Intent Router    │
                           │   & Compound Planner       │
                           └─────────────┬──────────────┘
                                         │
         ┌───────────────────────────────┼───────────────────────────────┐
         │                               │                               │
         ▼                               ▼                               ▼
┌──────────────────┐           ┌──────────────────┐            ┌───────────────────┐
│   Hybrid Brain   │           │ Desktop Vision   │            │ Hardware & OS     │
├──────────────────┤           ├──────────────────┤            ├───────────────────┤
│ Primary: Groq    │           │ PrintWindow GDI  │            │ Win32 / ctypes    │
│ Fallback: Ollama │           │ Gemini Vision    │            │ Process Spawning  │
│ Semantic Memory  │           │ Pixel-Diff Gate  │            │ Sockets / Flask   │
└────────┬─────────┘           └─────────┬────────┘            └─────────┬─────────┘
         │                               │                               │
         └───────────────────────────────┼───────────────────────────────┘
                                         │
                                         ▼
                           ┌────────────────────────────┐
                           │  Kokoro ONNX Worker Queue  │
                           │  & Desktop Notification    │
                           └────────────────────────────┘
```

---

## 🛠️ Key Engineering Challenges & Solutions

### 1. Multi-Target Batching for Token Optimization
* **The Challenge:** Polling independent background tasks (such as friend presence across games or download progress bars) scales token consumption linearly with each new target.
* **The Solution:** Engineered a group-coordinator thread that crops localized dynamic regions (such as chat panes or status docks) and prompts the vision model to return structured JSON evaluating all monitored targets in a single request. This bounds API usage to scale with the number of *windows* monitored rather than the number of *targets*.

### 2. Eliminating Voice-Induced Execution Latency
* **The Challenge:** Inline neural speech synthesis and character-by-character text streaming blocked the command execution thread, making the assistant unresponsive during long verbal replies.
* **The Solution:** Decoupled synthesis, playback, and character streaming into an asynchronous, queue-driven worker thread. Commands complete instantly while speech streams sequentially in the background.

### 3. Fault-Tolerant Hybrid Routing & Hardware Awareness
* **The Challenge:** Standard software ACPI suspension commands (`SetSuspendState`) frequently produce silent failures on modern laptop platforms (Modern Standby/S0 states), while intermittent network drops interrupt cloud LLM threads.
* **The Solution:** Built defensive hardware-interception layers to guide power states reliably. Paired this with a background connection monitor that automatically switches runtime context to local Ollama models whenever latency spikes or internet access drops.

---

## 💻 Technology Stack

| Layer | Technologies & Tools |
| :--- | :--- |
| **Frontend & UI** | Electron, HTML5/CSS3, MapLibre GL, Leaflet, Chart.js, Socket.IO Client |
| **Backend Core** | Python 3.12, Flask, Flask-SocketIO, Multithreading (`threading`, `queue`) |
| **LLM & Reasoning** | Groq API, Google Gemini Vision API, Ollama (DeepSeek-R1) |
| **Voice & Speech** | Kokoro ONNX, Vosk Speech Recognition, SoundDevice, SoundFile |
| **OS & Vision Capture** | Windows Win32 API (`pywin32`, `win32gui`, `ctypes`), PyAutoGUI, PIL |
| **Web & Automation** | Selenium WebDriver, yt-dlp, Requests |

---

## 🔒 Repository Status

This repository serves as a **technical architecture showcase and case study** for the Xenora desktop engine. 

The core execution source code, proprietary prompt schemas, and personal integration scripts remain private to protect system integrity and personal workspace configurations.

---

<div align="center">

**Developed with precision by Kunal**  
*Showcasing local-first desktop autonomy and multimodal systems engineering.*

</div>
