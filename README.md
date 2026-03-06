# GMM Background Subtraction — Live Demo

A real-time, in-browser implementation of the **Stauffer & Grimson (1999)** background subtraction algorithm, running entirely on your device using your own camera.

🔗 **[Try the live demo →](https://rahulbishnoi0014.github.io/gmm_by_rahul/)**

---

## What it does

The demo splits your camera feed into four live panels:

| Panel | Description |
|---|---|
| **Original** | Raw camera feed |
| **FG Mask** | Binary foreground / background decision |
| **Foreground** | Moving objects, isolated |
| **Background** | Learned static background |

No server. No uploads. Everything runs locally in your browser.

---

## The Algorithm

This is a direct implementation of the paper:

> Stauffer, C. & Grimson, W.E.L. (1999). *Adaptive background mixture models for real-time tracking.* CVPR.

### Core idea

Each pixel in the frame is modelled as a **mixture of K Gaussians**. Over time, stable (background) regions develop Gaussians with high weight and low variance. Moving objects produce Gaussians that are inconsistent and get low weight.

### Step by step

**1. Matching** — For each new frame, every pixel is compared to its K Gaussians using the Mahalanobis distance. A component matches if the pixel falls within threshold `T` standard deviations of the mean.

**2. Update** — Matched components update their mean, variance, and weight using learning rate `α`. Unmatched components slowly decay in weight.

**3. Replace** — If no component matches a pixel at all, the weakest component is replaced with a new Gaussian centred on the current pixel value. This lets the model adapt to new backgrounds.

**4. BG decision** — Components are ranked by `ω/√σ²` (high weight, low variance = reliable background). The top components whose weights sum to `B` form the background model. A pixel is foreground if it doesn't match any of them.

### Key parameters

| Symbol | Name | Effect |
|---|---|---|
| `α` | Learning rate | Higher → adapts faster, less stable |
| `K` | Components per pixel | Higher → handles more scene complexity |
| `T` | Match threshold | Lower → stricter, more foreground noise |
| `B` | Background ratio | Lower → fewer components in BG model |

All four parameters are adjustable live via sliders while the demo is running.

---

## Run locally

Just open `index.html` in any modern browser. No build step, no dependencies, no install.

```bash
git clone https://github.com/Rahulbishnoi0014/gmm_by_rahul
cd gmm-demo
open index.html        # macOS
# or just drag index.html into your browser
```

---

## About

**Rahul Bishnoi**
Masters Student, IIT Delhi

I built this as part of my coursework on computer vision and machine learning. The original assignment implemented Stauffer-Grimson on a pre-recorded video using Python and NumPy. This version ports the same algorithm to JavaScript so anyone can run it live in their browser — no setup required.

The math is identical to the paper. The only changes are using typed arrays (`Float32Array`) instead of NumPy for performance, and replacing `cv2.imshow` with HTML5 Canvas.

---

*Stauffer, C. & Grimson, W.E.L. (1999). Adaptive background mixture models for real-time tracking. CVPR.*
