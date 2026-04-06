# Pro Features Reference

Verified, production-ready code for enhanced presentation features. Include these based on user selections in Phase 1.

## Architecture Change: Absolute Positioning + Transitions

**IMPORTANT:** When Pro Features are enabled, slides use absolute positioning with JS-controlled transitions instead of scroll-snap. This enables smooth page transitions (fade/slide/zoom/flip).

```css
/* Slides are absolutely positioned, not scroll-snapped */
.slide {
    position: absolute;
    top: 0; left: 0;
    opacity: 0;
    pointer-events: none;
}
.slide.active { opacity: 1; pointer-events: auto; z-index: 2; }
.slide.prev { z-index: 1; }
```

Navigation is handled entirely by JS (`goToSlide()`), not scrolling.

---

## 1. Fragment System (Step-by-Step Reveal)

Elements with `.fragment` class are hidden initially. Pressing Space reveals them one at a time. When all fragments on a slide are revealed, Space advances to the next slide.

### CSS

```css
.fragment {
    opacity: 0;
    transform: translateY(24px);
    transition: opacity 0.5s var(--ease-out-expo), transform 0.5s var(--ease-out-expo);
    pointer-events: none;
}
.fragment.visible {
    opacity: 1;
    transform: translateY(0);
    pointer-events: auto;
}

/* Variants */
.fragment.fade-left { transform: translateX(-40px); }
.fragment.fade-left.visible { transform: translateX(0); }
.fragment.fade-right { transform: translateX(40px); }
.fragment.fade-right.visible { transform: translateX(0); }
.fragment.scale-up { transform: scale(0.8); }
.fragment.scale-up.visible { transform: scale(1); }
.fragment.blur-in { filter: blur(8px); transform: translateY(0); }
.fragment.blur-in.visible { filter: blur(0); }

/* Stagger delays */
.fragment:nth-child(1) { transition-delay: 0s; }
.fragment:nth-child(2) { transition-delay: 0.08s; }
.fragment:nth-child(3) { transition-delay: 0.16s; }
.fragment:nth-child(4) { transition-delay: 0.24s; }
.fragment:nth-child(5) { transition-delay: 0.32s; }
.fragment:nth-child(6) { transition-delay: 0.4s; }
```

### JS Logic

```javascript
// In the next() method:
next() {
    // Try to reveal next fragment first
    const slide = this.slides[this.currentSlide];
    const unrevealed = slide.querySelectorAll('.fragment:not(.visible)');
    if (unrevealed.length > 0) {
        unrevealed[0].classList.add('visible');
        return; // Don't advance slide
    }
    // All fragments revealed, go to next slide
    if (this.currentSlide < this.totalSlides - 1) {
        this.goToSlide(this.currentSlide + 1, 'next');
    }
}

// Auto-reveal all fragments when entering a slide (with stagger):
autoRevealFragments(slideIndex) {
    const slide = this.slides[slideIndex];
    const fragments = slide.querySelectorAll('.fragment');
    fragments.forEach((f, i) => {
        setTimeout(() => f.classList.add('visible'), i * 120);
    });
}
```

### HTML Usage

```html
<section class="slide">
    <div class="slide-content">
        <h2 class="fragment">Title appears first</h2>
        <p class="fragment">Then this paragraph</p>
        <div class="card fragment fade-left">Then this card from left</div>
        <div class="card fragment scale-up">Then this with zoom</div>
    </div>
</section>
```

---

## 2. Page Transition System

Four transition effects controlled by `data-transition` attribute on body.

### CSS

```css
/* CSS variable for duration */
:root { --transition-duration: 0.7s; }

/* FADE */
[data-transition="fade"] .slide.active { animation: fadeIn var(--transition-duration) var(--ease-out-expo) forwards; }
[data-transition="fade"] .slide.prev { animation: fadeOut var(--transition-duration) var(--ease-out-expo) forwards; }
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
@keyframes fadeOut { from { opacity: 1; } to { opacity: 0; } }

/* SLIDE */
[data-transition="slide"] .slide.active { animation: slideIn var(--transition-duration) var(--ease-out-expo) forwards; }
[data-transition="slide"] .slide.prev { animation: slideOut var(--transition-duration) var(--ease-out-expo) forwards; }
[data-transition="slide"][data-direction="prev"] .slide.active { animation: slideInReverse var(--transition-duration) var(--ease-out-expo) forwards; }
[data-transition="slide"][data-direction="prev"] .slide.prev { animation: slideOutReverse var(--transition-duration) var(--ease-out-expo) forwards; }
@keyframes slideIn { from { opacity: 0; transform: translateX(100px); } to { opacity: 1; transform: translateX(0); } }
@keyframes slideOut { from { opacity: 1; transform: translateX(0); } to { opacity: 0; transform: translateX(-100px); } }
@keyframes slideInReverse { from { opacity: 0; transform: translateX(-100px); } to { opacity: 1; transform: translateX(0); } }
@keyframes slideOutReverse { from { opacity: 1; transform: translateX(0); } to { opacity: 0; transform: translateX(100px); } }

/* ZOOM */
[data-transition="zoom"] .slide.active { animation: zoomIn var(--transition-duration) var(--ease-out-expo) forwards; }
[data-transition="zoom"] .slide.prev { animation: zoomOut var(--transition-duration) var(--ease-out-expo) forwards; }
@keyframes zoomIn { from { opacity: 0; transform: scale(0.85); } to { opacity: 1; transform: scale(1); } }
@keyframes zoomOut { from { opacity: 1; transform: scale(1); } to { opacity: 0; transform: scale(1.15); } }

/* FLIP */
[data-transition="flip"] .slide.active { animation: flipIn var(--transition-duration) var(--ease-out-expo) forwards; }
[data-transition="flip"] .slide.prev { animation: flipOut var(--transition-duration) var(--ease-out-expo) forwards; }
@keyframes flipIn { from { opacity: 0; transform: perspective(1200px) rotateY(-30deg); } to { opacity: 1; transform: perspective(1200px) rotateY(0); } }
@keyframes flipOut { from { opacity: 1; transform: perspective(1200px) rotateY(0); } to { opacity: 0; transform: perspective(1200px) rotateY(30deg); } }
```

### JS Navigation

```javascript
goToSlide(index, direction = 'next') {
    if (index < 0 || index >= this.totalSlides || index === this.currentSlide) return;
    this.isTransitioning = true;
    document.body.setAttribute('data-direction', direction);

    const prevSlide = this.slides[this.currentSlide];
    const nextSlide = this.slides[index];

    this.slides.forEach(s => s.classList.remove('active', 'prev'));
    prevSlide.classList.add('prev');
    nextSlide.classList.add('active');

    this.currentSlide = index;
    this.updateUI();

    // Auto-reveal fragments after transition starts
    setTimeout(() => this.autoRevealFragments(index), 200);

    const duration = parseFloat(getComputedStyle(document.documentElement)
        .getPropertyValue('--transition-duration')) * 1000 || 700;
    setTimeout(() => {
        prevSlide.classList.remove('prev');
        this.isTransitioning = false;
    }, duration + 50);
}
```

---

## 3. Theme System (4 Themes + Live Switch)

Define 4 themes using `[data-theme]` attribute on `<html>`.

```css
/* Default dark theme in :root */
:root { --bg-primary: #0a0a0f; --text-primary: #f0f0f5; /* ... */ }

/* Light */
[data-theme="light"] { --bg-primary: #fafafa; --text-primary: #1a1a2e; /* ... */ }

/* Warm */
[data-theme="warm"] { --bg-primary: #1a1410; --text-primary: #f5e6d3; --accent: #e17055; /* ... */ }

/* Ocean */
[data-theme="ocean"] { --bg-primary: #0b1628; --text-primary: #e8f4f8; --accent: #0984e3; /* ... */ }
```

Switch with: `document.documentElement.setAttribute('data-theme', 'light');`
Keyboard: T key cycles through themes.

---

## 4. Settings Panel

Left-aligned panel with three tabs: Feature toggles, Appearance, Page management. Includes scene presets for one-click configuration.

### Scene Presets

```javascript
const scenePresets = {
    present:  { fragments: true, timer: true, drawing: true, blackout: true, progressBar: true, pageNumber: true, navDots: false, dataAnimations: true },
    edit:     { fragments: false, timer: false, drawing: false, blackout: false, progressBar: true, pageNumber: true, navDots: true, dataAnimations: false },
    minimal:  { fragments: false, timer: false, drawing: false, blackout: false, progressBar: false, pageNumber: false, navDots: false, dataAnimations: false },
    full:     { fragments: true, timer: true, drawing: true, blackout: true, progressBar: true, pageNumber: true, navDots: true, dataAnimations: true }
};
```

### Feature Toggles (功能开关 tab)

Grouped by user mental model, NOT by technical implementation:
- **演讲辅助**: 分步显示, 计时器, 画笔标注, 黑屏暂停
- **界面显示**: 进度条, 页码, 导航圆点, 数据动画

Each toggle shows the feature name + a description explaining WHEN to use it (e.g., "按 B 暂停画面，注意力回到你身上"), not a technical description.

### Appearance Tab (外观 tab)

Contains inline selectors (NOT toggles) for:
- Theme: color dot buttons for dark/light/warm/ocean
- Transition: buttons for 淡入/滑动/缩放/翻转

### Page Management Tab (页面管理 tab)

Slide list with:
- Drag handle for reorder (HTML5 drag-and-drop)
- Click title to jump to slide
- ✎ button to enter edit mode on that slide
- ✕ button to delete slide (with confirm dialog)

---

## 5. Data Chart Animations

### Counter Animation (数字跳动)

```html
<p class="stat-number" data-target="2847" data-suffix="">0</p>
```

```javascript
animateNumber(el, start, end, duration, suffix) {
    const startTime = performance.now();
    const animate = (currentTime) => {
        const elapsed = currentTime - startTime;
        const progress = Math.min(elapsed / duration, 1);
        const eased = 1 - Math.pow(1 - progress, 4); // ease-out quart
        const current = Math.round(start + (end - start) * eased);
        el.textContent = current.toLocaleString() + suffix;
        if (progress < 1) requestAnimationFrame(animate);
    };
    requestAnimationFrame(animate);
}
```

### Bar Chart Animation (柱状图生长)

```html
<div class="bar" data-height="85%"><span class="bar-value">Label</span></div>
```

```css
.bar {
    transition: height 1.2s var(--ease-out-expo);
    height: 0;
}
.bar-value {
    opacity: 0;
    transition: opacity 0.5s ease 1s; /* Appears after bar finishes growing */
}
.bar.animated .bar-value { opacity: 1; }
```

```javascript
// Trigger when slide becomes visible:
slide.querySelectorAll('.bar[data-height]').forEach((bar, i) => {
    setTimeout(() => {
        bar.style.height = bar.dataset.height;
        bar.classList.add('animated');
    }, i * 80);
});
```

---

## 6. Drawing Canvas (画笔标注)

Full-screen canvas overlay for pen annotations during presentations.

```html
<canvas class="drawing-canvas"></canvas>
<div class="drawing-toolbar">
    <button class="draw-btn" data-size="3">细</button>
    <button class="draw-btn" data-size="6">中</button>
    <button class="draw-btn" data-size="12">粗</button>
    <!-- color dots -->
    <button class="color-dot" data-color="#ff4757" style="background:#ff4757"></button>
    <button class="color-dot" data-color="#ffa502" style="background:#ffa502"></button>
    <button class="color-dot" data-color="#2ed573" style="background:#2ed573"></button>
    <button class="color-dot" data-color="#1e90ff" style="background:#1e90ff"></button>
    <button class="color-dot" data-color="#ffffff" style="background:#ffffff"></button>
    <button class="draw-btn" onclick="clearDrawing()">清除</button>
    <button class="draw-btn" onclick="toggleDrawing()">退出</button>
</div>
```

Toggle with D key. Canvas uses standard 2D context drawing with `mousedown/mousemove/mouseup` and touch equivalents.

---

## 7. Print / PDF Export

**Critical:** Browsers don't print backgrounds by default. Must include:

```css
@media print {
    html, body {
        overflow: visible !important;
        height: auto !important;
        -webkit-print-color-adjust: exact !important;
        print-color-adjust: exact !important;
        color-adjust: exact !important;
    }
    * {
        -webkit-print-color-adjust: exact !important;
        print-color-adjust: exact !important;
    }
    .slide {
        position: relative !important;
        opacity: 1 !important;
        page-break-after: always;
        height: 100vh !important;
        background: var(--bg-primary) !important;
    }
    /* Hide all UI */
    .progress-bar, .nav-dots, .page-number, .timer,
    .theme-switcher, .transition-switcher, .drawing-toolbar,
    .overview-overlay, .blackout, .help-panel, .help-backdrop,
    .settings-btn, .settings-panel, .settings-backdrop { display: none !important; }
    /* Show all fragments */
    .fragment { opacity: 1 !important; transform: none !important; }
}
```

---

## 8. Always-Include Features

These are included in every presentation regardless of user selection:

| Feature | Key | Description |
|---------|-----|-------------|
| Page number | — | Right-bottom corner "3 / 12" display |
| Progress bar | — | Top thin bar showing progress |
| Fullscreen | F | Fullscreen API toggle |
| Thumbnail overview | O | Grid view of all slides for quick jump |
| Blackout | B | Black screen pause |
| Help panel | ? | Shows all available keyboard shortcuts |
| Timer | — | Bottom-left elapsed time, click to reset |
| Touch/swipe | — | Touch navigation for mobile |

---

## Keyboard Shortcut Summary

| Key | Action |
|-----|--------|
| ← → | Previous / Next slide |
| Space | Next fragment or next slide |
| F | Toggle fullscreen |
| O | Thumbnail overview |
| E | Toggle edit mode |
| B | Blackout screen |
| D | Drawing mode |
| T | Cycle theme |
| P | Print / Export PDF |
| ? | Help panel |
| Esc | Close any overlay |
