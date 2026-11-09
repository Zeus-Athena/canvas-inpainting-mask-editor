# Canvas Inpainting Mask Editor

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Live Demo](https://img.shields.io/badge/Live%20Demo-CleanerPix-emerald.svg)](https://cleanerpix.com)
[![Product Hunt](https://img.shields.io/badge/Product%20Hunt-CleanerPix-orange?logo=producthunt&logoColor=white)](https://www.producthunt.com/products/cleanerpix)
[![Status](https://img.shields.io/badge/Status-Production%20Ready-success.svg)](https://cleanerpix.com)

A lightweight, zero-dependency HTML5 Canvas brush masking tool designed for **AI image inpainting**, **object removal**, **watermark erasing**, and **sticker cleanup** workflows.

🔗 **Production Live Demo & Online Tool**: [CleanerPix - Free AI Image Cleaner](https://cleanerpix.com)

---

## 🌟 Overview

When building web interfaces for AI image inpainting models (such as **LaMa**, **Stable Diffusion Inpainting**, **Flux Inpainting**, or **OpenCV Inpainting**), frontend clients must generate an accurate binary mask (`mask.png` where unmasked areas are black `#000000` and masked areas to be regenerated are white `#ffffff`).

This repository provides a standalone, production-tested interactive canvas component extracted and adapted from the frontend architecture of [CleanerPix](https://cleanerpix.com). It enables users to upload photos, draw brush masks with variable thickness, undo strokes, and export inference-ready mask images.

---

## ✨ Features

- 🖌️ **Interactive Brush Masking**: Smooth, stutter-free bezier/line-cap drawing directly on an overlaid canvas.
- 🎯 **AI Inpainting Binary Export**: Exports pixel-perfect black & white mask images (`#000000` background, `#ffffff` brush areas) matching original image dimensions.
- 📐 **Dynamic Brush Cursor**: Visual circle cursor tracking current brush radius in real-time.
- 🔄 **Undo & Clear History**: Multi-step history stack for mistake recovery.
- 📱 **Mobile & Desktop Ready**: Full pointer event abstraction supporting mouse, Apple Pencil, and touch screens.
- ⚡ **Zero Dependencies**: Pure HTML5 Canvas 2D and vanilla JavaScript. No heavy UI framework or canvas wrapper needed.

---

## 🚀 Live Demo & Interactive Playground

- **Full AI Object Removal Tool**: [https://cleanerpix.com](https://cleanerpix.com)
- **Product Hunt Community**: [https://www.producthunt.com/products/cleanerpix](https://www.producthunt.com/products/cleanerpix)
- **Sticker & Watermark Eraser**: [https://cleanerpix.com/remove-sticker-from-image](https://cleanerpix.com/remove-sticker-from-image)
- **GitHub Pages Demo**: [https://zeus-athena.github.io/canvas-inpainting-mask-editor/](https://zeus-athena.github.io/canvas-inpainting-mask-editor/)

---

## 📦 Quick Start

### 1. Run Locally
Simply clone the repository and open `index.html` in any browser:
```bash
git clone https://github.com/Zeus-Athena/canvas-inpainting-mask-editor.git
cd canvas-inpainting-mask-editor
open index.html
```

### 2. Embed into Your Own Project

```html
<div id="mask-editor-container" style="position: relative;">
  <img id="source-image" src="photo.jpg" alt="Source" />
  <canvas id="mask-canvas" style="position: absolute; left: 0; top: 0;"></canvas>
</div>
```

```javascript
// Initialize mask overlay
const canvas = document.getElementById('mask-canvas');
const ctx = canvas.getContext('2d');
let brushRadius = 24;

// Set visual mask overlay styling (e.g. semi-transparent neon purple)
ctx.strokeStyle = 'rgba(168, 85, 247, 0.65)';
ctx.fillStyle = 'rgba(168, 85, 247, 0.65)';
ctx.lineCap = 'round';
ctx.lineJoin = 'round';

// Export binary mask for AI backend
function exportBinaryMask(width, height) {
  const offscreen = document.createElement('canvas');
  offscreen.width = width;
  offscreen.height = height;
  const offCtx = offscreen.getContext('2d');
  
  // Read brush strokes from overlay canvas
  const srcData = ctx.getImageData(0, 0, width, height);
  const outputImgData = offCtx.createImageData(width, height);
  const srcPixels = srcData.data;
  const outPixels = outputImgData.data;
  
  // Binary threshold: Painted -> White, Unpainted -> Black
  for (let i = 0; i < srcPixels.length; i += 4) {
    const alpha = srcPixels[i + 3];
    if (alpha > 20) {
      outPixels[i] = 255;     // R
      outPixels[i + 1] = 255; // G
      outPixels[i + 2] = 255; // B
      outPixels[i + 3] = 255; // A
    } else {
      outPixels[i] = 0;       // R
      outPixels[i + 1] = 0;   // G
      outPixels[i + 2] = 0;   // B
      outPixels[i + 3] = 255; // A
    }
  }
  offCtx.putImageData(outputImgData, 0, 0);
  return offscreen.toDataURL('image/png');
}
```

---

## 🛠️ Supported Workflows & Guides

- [Remove Unwanted Objects from Photos](https://cleanerpix.com/remove-object-from-photo)
- [Remove Tourists and Photobombers](https://cleanerpix.com/remove-person-from-photo)
- [Erase Text, Subtitles, and Watermarks Without Blurring](https://cleanerpix.com/blog/remove-watermark-or-text-without-blurring)
- [Cleanup.pictures Alternatives Guide](https://cleanerpix.com/blog/cleanup-pictures-alternatives)

---

## 📄 License

Distributed under the MIT License. See [LICENSE](LICENSE) for more details.

Built with ❤️ by the community & powered by [CleanerPix](https://cleanerpix.com).
