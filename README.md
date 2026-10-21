# LiquidGlass Studio — Custom Liquid Glass Code Generator

An interactive, high-fidelity visual designer and code generator for creating customizable **Liquid Glass** refraction effects, reverse-engineered from `https://liquid-glass-pi.vercel.app/#examples` using **REA** (Reverse Engineer Anything).

---

## 🔬 How It Works (REA Reverse-Engineered Architecture)

Traditional "glassmorphism" on the web relies exclusively on `backdrop-filter: blur(...)` with a translucent white background. This misses the physical phenomenon of **refraction**—the bending and deflection of light rays as they pass through curved glass edges.

Through static analysis of the target Next.js application bundles using REA, we uncovered the exact optical shader pipeline:

```
┌────────────────────────────────┐
│   Procedural Tangent Normal    │  Calculates Signed Distance Field (SDF)
│      Map Generator (Canvas)    │  Encodes horizontal & vertical deflection
└───────────────┬────────────────┘  into Red & Green channels [0..255]
                │
                ▼
┌────────────────────────────────┐
│       SVG Displacement         │  <feDisplacementMap in="SourceGraphic"
│         Filter Pipeline        │   in2="normalMap" xChannelSelector="R"
└───────────────┬────────────────┘   yChannelSelector="G" scale="...">
                │
                ▼
┌────────────────────────────────┐
│       CSS Backdrop Filter      │  backdrop-filter: url(#filter_id)
│        & Physical Optics       │                   blur(...) brightness(...)
└────────────────────────────────┘  Multi-layer specular rim reflections (inset shadow)
```

### Key Technical Insights

1. **Normal Map Encoding**:
   - Red channel represents horizontal ($X$) deflection.
   - Green channel represents vertical ($Y$) deflection.
   - Neutral baseline is `rgb(128, 128, 255)` (normal vector pointing directly at observer $[0, 0, 1]$).
2. **Backdrop-Filter Integration**:
   - `backdrop-filter: url(#liquid_glass_filter)` pulls the rendered backdrop pixels behind the element, displaces them based on the normal map, and then applies secondary blur, brightness, and saturation adjustments.
3. **Physical Specular Highlights**:
   - Multi-layered inset and drop shadows create the realistic caustic glass appearance:
     ```css
     box-shadow:
       0 12px 32px rgba(0, 0, 0, 0.40),
       0 4px 12px rgba(0, 0, 0, 0.25),
       0 -12px 28px inset rgba(0, 0, 0, 0.20),
       0 -1px 4px 1px inset rgba(255, 255, 255, 0.74);
     ```

---

## 🚀 Running the Studio

### Quick Run

You can run the built-in zero-dependency Node server:

```bash
npm start
# or
node server.mjs
```

Then navigate to: **`http://localhost:3000`**

Alternatively, you can open `index.html` directly in any modern browser without any server needed!

### Deploy to GitHub Pages

This repository includes a ready-to-use GitHub Actions workflow (`.github/workflows/deploy.yml`).
1. Push this repository to GitHub.
2. Go to **Settings > Pages > Build and deployment**.
3. Under **Source**, select **GitHub Actions**.
4. Your studio will automatically be live at `https://<username>.github.io/<repository-name>/`!

---

## ✨ Features

- **Real-Time Interactive Lens**: Draggable liquid glass lens on an active canvas with animated fluid mesh and photography backgrounds.
- **Full Optical Controls**:
  - Width, Height, Corner Radius
  - Refraction Distortion Scale
  - Post-Blur, Brightness, Saturation
  - Specular Rim Highlight Intensity & Inset Shadow Depth
- **Curated Templates**:
  - **macOS Liquid Dock**: macOS-style application dock with floating icons.
  - **Floating Navigation Bar**: Sleek glass navigation pill with links and search.
  - **Frosted Card**: Modern glass profile/product card with cover image and body.
- **Multi-Format Code Generator**:
  - Standalone HTML + Inline SVG (Zero external dependencies)
  - One-click copy or custom HTML/CSS generator.

---

## 📄 License & Credits

- Developed by **tylonmademe**
- Licensed under the [MIT License](LICENSE)
