<!--
PhishDetect AI — README.md
Author: Hassan Ali Abrar
License: MIT
-->

# PhishDetect AI — Android (Java) — On-Device Phishing Guard

> *“Guard the text, catch the lie — on device, quick as a sigh.”*  
> Lightweight, private, and fast phishing detection that runs entirely on your Android device.

---

## ⚡ Vision
Detect suspicious links from SMS/clipboard/text in **< 100 ms** on-device; fully private (no remote DB), TFLite model stored in app assets, upgradeable ML pipeline.

---

## 🧩 Key Principles
- **Java + Android Studio**: core app & "backend" inside the app (no server).  
- **Private-first**: no remote database; all data and models stored locally.  
- **ML**: initial model is a **Random Forest** converted to **TensorFlow Lite** (TFLite) — later upgrade path to stronger models (TF-DF, small transformers, quantized ensembles).  
- **Fast**: target **<100ms** per inference via quantization, NNAPI/GPU delegates, optimized features.  
- **Reactive**: listens to clipboard & incoming messages, extracts links, scores them, and issues local Notification when suspicious.

---

## 🔭 High-level features
- Real-time **link extraction** from clipboard & incoming message text  
- **On-device inference** using `model.tflite` (in `app/src/main/assets/`)  
- **Notification alerts** for suspicious URLs (customizable sensitivity)  
- **No external network** required for detection — privacy first  
- Lightweight **feature extractor** (lexical URL features, token counts, domain heuristics) — compact and fast

---

## ⚙️ How it works — summary
1. **Listen**: ClipboardManager + SMS/Notification listener picks up new text.  
2. **Extract**: A tiny parser extracts URLs and contextual tokens from text.  
3. **Feature Vector**: Convert URL/text into a small numeric vector (length, subdomain count, token entropy, suspicious keywords count, TLD heuristics).  
4. **Infer**: Run `model.tflite` through TensorFlow Lite Interpreter (local).  
5. **Act**: If score > threshold → show local Notification and optional in-app prompt.

---

## 🔧 Quick Setup (developer)
1. Open Android Studio → **Open** existing project → select this folder.  
2. Place `model.tflite` and `labels.txt` in `app/src/main/assets/`.  
3. Add TFLite dependency in `app/build.gradle`:
```gradle
dependencies {
    implementation 'org.tensorflow:tensorflow-lite:2.11.0'            // version example
    implementation 'org.tensorflow:tensorflow-lite-support:0.4.3'
    // Optional delegates:
    // implementation 'org.tensorflow:tensorflow-lite-gpu:2.11.0'
}
```
--- 
Build & run on a real device (emulator inference speed differs).


## 🛠️ Performance & optimization tips to meet < 100 ms

Convert model to int8 or float16 quantized TFLite (smaller & faster).

Use NNAPI / GPU delegate when available (fall back to CPU).

Keep feature vector very small (e.g., 12–32 float features).

Warm up interpreter on app start.

Batch very small (1 sample) inference — avoid heavy pre/post-processing on UI thread.

Use Interpreter.Options().setNumThreads(...) tuned for device.

---


## 🔐 Privacy-first design

No network calls for inference or storage by default.

Models and logs remain on-device in app-private storage.

If you add optional telemetry later, make it opt-in and anonymized.

---

## ❤️ Contribute / Ideas

Improve feature extractor (add WHOIS / domain-age heuristics — offline precomputed).

Add in-app simulation module for awareness training.

Provide a small local “quarantine” UI for flagged links.

---

## 📜 License

MIT License — use and adapt, but please share improvements back to the community responsibly.

---

## 👋 Final note 

Tiny engine, heavy guard

Build small, iterate fast
