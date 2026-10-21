<div align="center">

  <h1>✨ LiquidGlass Studio</h1>
  <p><strong>Next-generation physical refraction & optical glassmorphism studio for the modern web.</strong></p>

  <p>
    <a href="https://tylonmademe-1.github.io/liquid-glass-studio/"><img src="https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-3b82f6?style=for-the-badge&logo=github" alt="Live Demo"></a>
    <img src="https://img.shields.io/badge/License-MIT-emerald?style=for-the-badge" alt="License">
    <img src="https://img.shields.io/badge/Built%20With-Vanilla%20HTML%2FCSS%2FJS-f59e0b?style=for-the-badge&logo=javascript" alt="Vanilla">
    <img src="https://img.shields.io/badge/Zero%20Dependencies-100%25-6366f1?style=for-the-badge" alt="Zero Dependencies">
  </p>

  <p>
    <a href="#-quick-start">Quick Start</a> •
    <a href="#-how-it-works">How It Works</a> •
    <a href="#-features">Features</a> •
    <a href="#-curated-templates">Templates</a> •
    <a href="#-exporting--using-in-your-app">Code Generator</a> •
    <a href="#-author--license">License</a>
  </p>

</div>

---

## 🔮 Overview

Standard "glassmorphism" on the web relies only on simple blur (`backdrop-filter: blur(...)`) with a semi-transparent white background. This completely misses the physical laws of **optical refraction**—the chromatic bending, distortion, and caustic deflection of light rays as they pass through curved, polished glass surfaces.

**LiquidGlass Studio** brings true physical refraction to the browser with zero dependencies:
- Real-time procedural **tangent normal maps** computed via continuous Signed Distance Fields (SDF).
- Hardware-accelerated **SVG displacement filters** (`feDisplacementMap`).
- Dynamic multi-layered specular lighting, caustic rim highlights, and ambient depth.
- Interactive macOS desktop environment with draggable windows, customizable sliders, wallpaper switching, and instant code generation.

---

## 🔬 How It Works (Optical Shader Engine)

```
┌─────────────────────────────────┐
│     Procedural Normal Map       │  Calculates Signed Distance Field (SDF) &
│      Generator (Offscreen)      │  encodes horizontal & vertical deflection vectors
└────────────────┬────────────────┘  into Red & Green color channels [0..255]
                 │
                 ▼
┌─────────────────────────────────┐
│   Hardware SVG Displacement     │  <feDisplacementMap in="SourceGraphic"
│        Filter Engine            │   in2="normalMap" scale="..." />
└────────────────┬────────────────┘
                 │
                 ▼
┌─────────────────────────────────┐
│       CSS Backdrop Filter       │  backdrop-filter: url(#lens_filter)
│    & Multi-Layer Speculars      │                   blur(...) brightness(...) saturate(...)
└─────────────────────────────────┘  Realistic inner caustic rim highlights & shadows
```

### Optical Highlights
1. **Tangent Normal Encoding**:
   - The **Red channel** encodes horizontal ($X$) deflection.
   - The **Green channel** encodes vertical ($Y$) deflection.
   - Neutral surface baseline is `rgb(128, 128, 255)` (normal vector pointing straight at the viewer $[0, 0, 1]$).
2. **Backdrop Refraction**:
   - The SVG displacement filter dynamically samples backdrop pixels behind the element and distorts them according to the normal map gradients, followed by post-blur and chromatic saturation.
3. **Physical Specular Caustics**:
   - Realistic bevel reflections and inset rim shadows simulate thick physical glass:
     ```css
     box-shadow:
       0 4px 8px rgba(0, 0, 0, 0.25),
       0 -10px 25px inset rgba(0, 0, 0, 0.15),
       0 -1px 4px 1px inset rgba(255, 255, 255, 0.74);
     ```

---

## 🚀 Quick Start

### 1. Run Locally (Zero Setup Needed)

```bash
# Clone the repository
git clone https://github.com/tylonmademe-1/liquid-glass-studio.git
cd liquid-glass-studio

# Start the built-in local server
npm start
# or: node server.mjs
```

Open **`http://localhost:3000`** in your browser.

> [!TIP]
> You can also double-click and open `index.html` directly in Google Chrome, Microsoft Edge, Safari, or Brave—no server required!

### 2. Deploy to GitHub Pages

This repo includes an automated GitHub Actions deployment workflow (`.github/workflows/deploy.yml`):
1. In your GitHub repo, go to **Settings** $\rightarrow$ **Pages**.
2. Under **Build and deployment** $\rightarrow$ **Source**, choose **GitHub Actions**.
3. Your site will automatically be live at `https://<your-username>.github.io/liquid-glass-studio/`!

---

## ✨ Features

- 💧 **Physical Draggable Lens**: Floating liquid glass bubble that dynamically refracts whatever is behind it in real time (windows, desktop icons, and wallpapers).
- 🎛️ **Precision Optical Controls**:
  - **Width & Height**: Real-time dimension resizing.
  - **Corner Radius**: From sharp-edged glass cards to pill lenses.
  - **Distortion Scale**: Control the refraction index and displacement intensity.
  - **Post-Blur**: Softens refracted rays for frosted vs. crystal-clear glass.
  - **Brightness & Saturation**: Adjust caustic luminescence and vibrant backdrop transmission.
- 🎨 **Liquid Glass Sliders**: Custom interactive capsule sliders with mini-lens magnification and fluid swiping physics.
- 🖼️ **Dynamic Wallpapers**: Switch between curated 4K dynamic desktop themes with smooth cross-fading.
- 💻 **Window Management**: Authentic macOS window controls (traffic light buttons, drag, minimize, maximize, and spotlight search via `⌘K` or `Ctrl+K`).
- ⚡ **120 FPS Performance**: GPU-accelerated SVG filter engine with zero layout reflows during pointer dragging.

---

## 📦 Curated Templates

LiquidGlass Studio includes pre-configured, battle-tested templates:

| Template | Preview Description |
| :--- | :--- |
| **🍎 macOS Dock** | Floating application dock with translucent glass bar, rounded icons, and interactive hover magnification. |
| **🧭 Floating Navbar** | Sleek glass navigation pill with search input, navigation links, and subtle caustic borders. |
| **🎴 Frosted Card** | Modern physical glass card with hero image cover, glass typography, and physical toolbar. |

---

## 🛠️ Exporting & Using in Your App

You can generate and copy clean, dependency-free HTML/CSS directly from the **Generate HTML/CSS** tab inside the studio:

### Basic Liquid Glass Snippet

```html
<!-- 1. The SVG Displacement Filter (Place anywhere in your HTML) -->
<svg width="0" height="0" style="position: fixed; pointer-events: none;">
  <defs>
    <filter id="liquid_glass" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB" x="0" y="0" width="300" height="200">
      <feImage href="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='300' height='200'><radialGradient id='invmap' cx='50%' cy='50%' r='75%'><stop offset='0%' stop-color='rgb(128,128,255)'/><stop offset='90%' stop-color='rgb(255,255,255)'/></radialGradient><rect width='100%' height='100%' fill='url(%23invmap)'/></svg>" result="normalMap" />
      <feDisplacementMap in="SourceGraphic" in2="normalMap" scale="49" xChannelSelector="R" yChannelSelector="G" />
    </filter>
  </defs>
</svg>

<!-- 2. The Liquid Glass Element -->
<div style="
  width: 300px;
  height: 200px;
  border-radius: 100px;
  backdrop-filter: url(#liquid_glass) blur(0.25px) brightness(1.5) saturate(1.1);
  -webkit-backdrop-filter: url(#liquid_glass) blur(0.25px) brightness(1.5) saturate(1.1);
  box-shadow:
    0 4px 8px rgba(0, 0, 0, 0.25),
    0 -10px 25px inset rgba(0, 0, 0, 0.15),
    0 -1px 4px 1px inset rgba(255, 255, 255, 0.74);
">
  <!-- Your content here -->
</div>
```

---

## ⌨️ Shortcuts

- `⌘K` or `Ctrl + K`: Open Spotlight search modal to quickly jump between templates, windows, and themes.
- `ESC`: Close modals or active dialogs.

---

## 📄 Author & License

- Created by **[tylonmademe](https://github.com/tylonmademe-1)**
- Released under the [MIT License](LICENSE). Feel free to use, modify, and build upon this in your personal or commercial projects!
