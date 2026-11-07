# Linguistack 2.0: Offline-First, AI-Powered Inclusive ICT Learning Platform

An inclusive, 100% offline educational Progressive Web App (PWA) engineered for Sri Lankan Grade 6 & 7 secondary school students. Built to overcome rural internet dropouts, legacy hardware constraints[cite: 3], and physical/visual accessibility barriers[cite: 3].

---

## 🌟 Key Research Contributions & Features

* **100% Offline PWA Architecture:** Operates entirely within browser RAM without server dependency[cite: 3], utilizing service workers, W3C IndexedDB (sub-5ms write latencies)[cite: 3], and local WASM caching.
* **Touchless 3D Gesture Control:** Real-time 1–4 finger counting engine using MediaPipe WebAssembly (WASM) and 3D Euclidean spatial vector geometry, ensuring **98.28% accuracy** and 100% rotation invariance under $0^\circ$ to $90^\circ$ hand pitch tilt[cite: 3].
* **WCAG 2.1 AAA Accessibility Framework:** Comprehensive inclusion suite achieving minimum **7:1 contrast ratios** across custom themes[cite: 5, 8].
* **Dynamic Colorblind Matrix:** Instant DOM theme injection (`data-theme`) supporting Protanopia, Deuteranopia, Tritanopia, and High-Contrast OLED Yellow (19.5:1 contrast)[cite: 5, 8].
* **Low-Vision Floating Dock (`LowVisionToolbar.tsx`):** Persistent accessibility controls featuring an oversized yellow pointer, a 350ms-delay **Hover-to-Speak Text-to-Speech (TTS)** assistant (<12ms latency), and single-column layout reflow up to 200% zoom[cite: 5, 8].
* **CPU Canvas Optical Fallback Engine:** Scans RGB luminance columns in HTML5 Canvas memory at 30 FPS when WebGL/GPU acceleration is absent on legacy PCs (Intel Celeron / Core i3)[cite: 3].

---

## 🏗️ 4-Tier System Architecture

1. **Tier 1 (Presentation & Accessibility Layer):** React UI, WCAG 2.1 AAA contrast engine, dynamic colorblind themes, and Low-Vision assistive toolbar[cite: 3].
2. **Tier 2 (Local AI Perception Engine):** MediaPipe WASM runtime, 3D Euclidean spatial distance math model, and CPU canvas fallback loop[cite: 3].
3. **Tier 3 (Offline Storage Tier):** W3C IndexedDB handling transactional object stores (`studentProfiles`, `quizAttempts`, `interactionLogs`) with ACID compliance[cite: 3].
4. **Tier 4 (Ephemeral Privacy & Memory Buffer):** Secure local WASM memory buffers ensuring camera video streams are processed instantly with immediate frame drops[cite: 3].

---

## 📊 Empirical Evaluation & Benchmarks

| Hardware Tier | Specifications | FPS & Latency | RAM Usage |
|---|---|---|---|
| **Tier 1 (Legacy Laptop)** | Intel Celeron N4020 (1.1GHz), 4GB RAM[cite: 3] | 24.2 (WASM) / 30.0 (CPU)[cite: 3] | 88 MB[cite: 3] |
| **Tier 2 (Lab Desktop)** | Intel Core i3-7100 (3.9GHz), 4GB RAM[cite: 3] | 29.8 (WASM) / 30.0 (CPU)[cite: 3] | 112 MB[cite: 3] |
| **Tier 3 (Modern Desktop)** | Intel Core i5-10400 (2.9GHz), 8GB RAM[cite: 3] | 30.0 FPS (GPU WASM)[cite: 3] | 145 MB[cite: 3] |

* **Pedagogical Learning Gain:** Pre- to post-test evaluations across $N=120$ students demonstrated a score increase from 39.1% to 84.1%, yielding a high normalized learning gain of **$g = 0.74$**[cite: 3].
* **System Usability Scale (SUS):** Rated **93.2/100 (Grade A+)** by visually impaired and colorblind secondary students[cite: 3].

---

## 💻 Core Code Sample: Accessibility Manager (`AccessibilityManager.ts`)

Developed by V. Sathiyapiragash (Accessibility Specialist & WCAG AAA Lead)[cite: 5, 8]:

```typescript
export type ColorBlindMode = 
  | 'standard'
  | 'protanopia'
  | 'deuteranopia'
  | 'tritanopia'
  | 'oled-high-contrast'
  | 'oled-yellow';

export class AccessibilityManager {
  private currentMode: ColorBlindMode = 'standard';
  private hoverSpeakEnabled: boolean = false;
  private hoverTimeout: ReturnType<typeof setTimeout> | null = null;

  public setTheme(mode: ColorBlindMode): void {
    this.currentMode = mode;
    const root = document.documentElement;
    root.setAttribute('data-theme', mode);
  }

  public enableHoverToSpeak(): void {
    if (this.hoverSpeakEnabled) return;
    this.hoverSpeakEnabled = true;
    document.addEventListener('mouseover', this.handleHoverSpeak);
  }

  private handleHoverSpeak = (e: MouseEvent): void => {
    if (this.hoverTimeout) clearTimeout(this.hoverTimeout);
    const target = e.target as HTMLElement;
    if (!target) return;
    
    this.hoverTimeout = setTimeout(() => {
      const text = target.getAttribute('aria-label') || target.innerText?.trim();
      if (text && text.length < 300) {
        const utterance = new SpeechSynthesisUtterance(text);
        window.speechSynthesis.speak(utterance);
      }
    }, 350);
  };
}
