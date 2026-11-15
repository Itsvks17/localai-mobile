# LocalAI Mobile — Turnkey On-Device AI Starter Kit

<p align="center">
  <img src="assets/banner.png" alt="LocalAI Mobile Banner" width="100%" />
</p>

<p align="center">
  <a href="https://darkryoshin.gumroad.com/l/localai-mobile"><img src="https://img.shields.io/badge/🚀%20Get%20Full%20Starter%20Kit-Gumroad%20($39)-ff90e8?style=for-the-badge&logo=gumroad&logoColor=white" alt="Get Starter Kit on Gumroad" /></a>
  <a href="https://expo.dev"><img src="https://img.shields.io/badge/Expo-SDK%2052-black?style=for-the-badge&logo=expo&logoColor=white" alt="Expo 52" /></a>
  <a href="https://reactnative.dev"><img src="https://img.shields.io/badge/React%20Native-0.76.9-61dafb?style=for-the-badge&logo=react&logoColor=black" alt="React Native 0.76" /></a>
  <img src="https://img.shields.io/badge/Tests-119%20Passing-success?style=for-the-badge&logo=node.js&logoColor=white" alt="119 Tests Passing" />
  <img src="https://img.shields.io/badge/Inference-100%25%20Offline-blueviolet?style=for-the-badge" alt="100% Offline" />
</p>

---

## ⚡ Overview

**LocalAI Mobile** is a production-ready mobile starter kit built with **Expo 52**, **React Native 0.76.9**, and **TypeScript** that allows you to run local GGUF Large Language Models (LLMs) **100% offline** directly on iOS and Android devices using **`llama.rn`** and **SQLite**.

Zero cloud dependencies, zero monthly API bills (OpenAI / Anthropic / Groq), and 100% complete privacy for your end users.

> 💡 **Ready to launch your own offline AI mobile app?**  
> Skip hundreds of hours of debugging native C++ throttles, background foreground services, and document extractors.  
> 👉 **[Download the Full Production Starter Kit on Gumroad ($39)](https://darkryoshin.gumroad.com/l/localai-mobile)**

---

## 🔥 Key Technical Capabilities

### 1. 🧠 100% Local On-Device Inference
* Powered by native **`llama.rn`** bindings with GPU/CPU acceleration.
* Runs standard quantized GGUF models directly on device:
  * **Llama 3.2 1B & 3B Instruct**
  * **Qwen 2.5 0.5B, 1.5B & 3B Instruct**
  * **SmolLM2 1.7B & 360M**
* In-app HuggingFace model downloader with progress tracking and automatic integrity checks.

### 2. ⚡ Adaptive C++ Duty-Cycle Throttling
* Includes custom-patched native decode loops to regulate sustained mobile inference.
* Eliminates phone overheating, battery drain spikes, and Android OS low-memory killer (OOM) crashes during long token generations.

### 3. 🔄 Background Streaming Foreground Service
* Dedicated native Android foreground service keeps streaming LLM tokens even when the user minimizes the app or turns off their screen.
* Includes persistent notification controls with stop actions and wake locks.

### 4. 🎭 6 AI Personas Out-of-the-Box
* Instant, one-tap switching between tailored system prompt personas:
  * 💬 **Everyday Companion**: Casual, friendly conversation.
  * ✍️ **Creative Writer**: Storytelling and drafting.
  * 🔬 **Feynman Tutor**: Explains complex concepts in simple terms.
  * 💼 **Executive Assistant**: Action items, summaries, and meeting notes.
  * 🧘 **Life Coach**: Habit building and reflection.
  * 💻 **Developer**: Structured code explanations with syntax-highlighted code blocks.

### 5. 👁️ Multimodal OCR & Local Document Extraction
* **Camera & Photo OCR**: Real-time on-device text recognition via `expo-mlkit-ocr`.
* **Native Document Parser**: Custom Android native module to read and extract text from local **PDFs** (up to 64 pages) and Word **DOCX** files directly on device.
* **Code & Text Attachments**: Attach `.ts`, `.py`, `.json`, `.txt` files directly into context.

### 6. 💾 Atomic SQLite Response Checkpointing
* Streams tokens into local SQLite database every 2 seconds.
* If a generation is interrupted by an incoming phone call or app termination, conversations can be seamlessly resumed without lost state.

### 7. 🎨 Dynamic Material You Theming
* Extracts accessible, high-contrast UI color palettes dynamically from user wallpaper photos.
* Clean light and dark mode support.

---

## 📱 Screenshots Showcase

<p align="center">
  <img src="assets/01_main_chat.png" width="30%" alt="Main Chat UI" />
  <img src="assets/02_persona_selector.png" width="30%" alt="Persona Selector" />
  <img src="assets/03_model_picker.png" width="30%" alt="Model Picker" />
</p>

<p align="center">
  <img src="assets/04_model_manager.png" width="45%" alt="Model Manager" />
  <img src="assets/08_attachment_menu.png" width="45%" alt="Multimodal Attachments" />
</p>

---

## 🚀 Tested on Real Hardware

Verified on physical devices and emulators:
* **Physical Device**: Motorola Edge 50 Neo (Android 14 / 15, MediaTek Dimensity 7300, 8GB RAM).
* **Speed**: 18–25+ tokens/sec on Llama 3.2 1B (Q4_K_M); 10–14 tokens/sec on Qwen 2.5 1.5B.
* **Memory Footprint**: Safe 800MB–1.4GB RAM ceiling with adaptive throttling enabled.
* **Architecture**: Fully compatible with React Native 0.76.9 New Architecture / Old Architecture, EAS Build, and local Gradle/Xcode builds.

---

## 📦 What You Get in the Full Starter Kit

When you purchase the starter kit, you get immediate access to:

```text
LocalAI-Mobile/
├── src/
│   ├── app/                 # Expo Router file-based screens & navigation
│   ├── components/          # 30+ reusable atomic UI components (chat, models, settings)
│   ├── hooks/               # useLocalLlm, useChat, useForegroundService, useWallpaper
│   ├── services/            # llama.rn wrapper, SQLite database, HuggingFace downloader
│   └── types/               # Strict TypeScript definitions
├── android/                 # Custom native modules (DocumentParser, ForegroundService)
├── plugins/                 # Expo config plugins for llama.rn native builds
├── patches/                 # llama.rn adaptive C++ duty-cycle patch
├── tests/                   # 119 automated regression & unit tests
├── QUICKSTART.md            # 5-minute setup guide
└── README.md                # Full architectural documentation
```

---

## 🛠️ Quickstart (Once Downloaded)

```bash
# 1. Install dependencies
npm install

# 2. Run test suite & typecheck
npm run typecheck
npm test

# 3. Launch on Android or iOS
npm run android
npm run ios
```

---

## 🛒 Purchase & Commercial License

* Single Developer License allows unlimited personal and commercial apps.
* No attribution required in your final compiled mobile app.

👉 **[Get Instant Access on Gumroad ($39)](https://darkryoshin.gumroad.com/l/localai-mobile)**

---

<p align="center">
  <sub>Built with ❤️ for indie mobile developers building the private, on-device AI future.</sub>
</p>
