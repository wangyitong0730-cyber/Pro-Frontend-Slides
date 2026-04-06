# Animation Patterns Reference

Use this reference when generating presentations. Match animations to the intended feeling.

## Effect-to-Feeling Guide

| Feeling | Animations | Visual Cues |
|---------|-----------|-------------|
| **Dramatic / Cinematic** | Slow fade-ins (1-1.5s), large scale transitions (0.9 to 1), parallax scrolling, **spotlight cursor** | Dark backgrounds, spotlight effects, full-bleed images |
| **Techy / Futuristic** | Neon glow (box-shadow), glitch/scramble text, grid reveals, **SVG stroke draw** | Particle systems (canvas), grid patterns, monospace accents, cyan/magenta/electric blue |
| **Playful / Friendly** | Bouncy easing (spring physics), floating/bobbing, **3D tilt cards** | Rounded corners, pastel/bright colors, hand-drawn elements |
| **Professional / Corporate** | Subtle fast animations (200-300ms), clean slides, **staggered entrance** | Navy/slate/charcoal, precise spacing, data visualization focus |
| **Calm / Minimal** | Very slow subtle motion, gentle fades, **staggered entrance** | High whitespace, muted palette, serif typography, generous padding |
| **Editorial / Magazine** | Staggered text reveals, image-text interplay, **SVG stroke draw** | Strong type hierarchy, pull quotes, grid-breaking layouts, serif headlines + sans body |
| **Storytelling / Narrative** | **Spotlight cursor**, **3D tilt**, **SVG stroke draw**, **staggered entrance** | 引导注意力，配合叙事节奏，逐步揭示内容 |

## Entrance Animations

```css
/* Fade + Slide Up (most versatile) */
.reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.6s var(--ease-out-expo),
                transform 0.6s var(--ease-out-expo);
}
.visible .reveal {
    opacity: 1;
    transform: translateY(0);
}

/* Scale In */
.reveal-scale {
    opacity: 0;
    transform: scale(0.9);
    transition: opacity 0.6s, transform 0.6s var(--ease-out-expo);
}

/* Slide from Left */
.reveal-left {
    opacity: 0;
    transform: translateX(-50px);
    transition: opacity 0.6s, transform 0.6s var(--ease-out-expo);
}

/* Blur In */
.reveal-blur {
    opacity: 0;
    filter: blur(10px);
    transition: opacity 0.8s, filter 0.8s var(--ease-out-expo);
}
```

## Background Effects

```css
/* Gradient Mesh — layered radial gradients for depth */
.gradient-bg {
    background:
        radial-gradient(ellipse at 20% 80%, rgba(120, 0, 255, 0.3) 0%, transparent 50%),
        radial-gradient(ellipse at 80% 20%, rgba(0, 255, 200, 0.2) 0%, transparent 50%),
        var(--bg-primary);
}

/* Noise Texture — inline SVG for grain */
.noise-bg {
    background-image: url("data:image/svg+xml,..."); /* Inline SVG noise */
}

/* Grid Pattern — subtle structural lines */
.grid-bg {
    background-image:
        linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
    background-size: 50px 50px;
}
```

## Interactive Effects

### Spotlight Cursor — 聚光灯效果

鼠标即激光笔，移到哪里哪里亮。适合引导观众注意力、强调重点内容。

```css
/* 聚光灯光标 — 跟随鼠标的光晕 */
.cursor-spotlight {
    position: fixed;
    width: 300px; height: 300px;
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    background: radial-gradient(circle, rgba(255,255,255,0.08) 0%, transparent 70%);
    transform: translate(-50%, -50%);
    transition: opacity 0.3s;
}
.cursor-dot {
    position: fixed;
    width: 8px; height: 8px;
    border-radius: 50%;
    background: rgba(255,255,255,0.9);
    pointer-events: none;
    z-index: 10000;
    transform: translate(-50%, -50%);
    transition: transform 0.15s ease;
}
.cursor-dot.hovering {
    transform: translate(-50%, -50%) scale(3);
    background: rgba(120, 119, 255, 0.6);
}

/* 聚光灯照亮区域 — 用在内容面板上 */
.spotlight-panel {
    position: relative;
    overflow: hidden;
}
.spotlight-panel::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(circle at var(--mx, 50%) var(--my, 50%),
        rgba(120,119,255,0.15) 0%, transparent 50%);
    pointer-events: none;
    transition: background 0.3s;
}
/* 文字默认暗淡，hover 时高亮 */
.spotlight-panel p {
    color: rgba(255,255,255,0.25);
    transition: color 0.3s;
}
.spotlight-panel:hover p { color: rgba(255,255,255,0.15); }
.spotlight-panel p:hover {
    color: rgba(255,255,255,0.95) !important;
    text-shadow: 0 0 20px rgba(120,119,255,0.3);
}
```

```javascript
/* 聚光灯 JS — 跟踪鼠标位置 */
const spotlight = document.querySelector('.cursor-spotlight');
const cursorDot = document.querySelector('.cursor-dot');

document.addEventListener('mousemove', (e) => {
    if (spotlight) {
        spotlight.style.left = e.clientX + 'px';
        spotlight.style.top = e.clientY + 'px';
    }
    if (cursorDot) {
        cursorDot.style.left = e.clientX + 'px';
        cursorDot.style.top = e.clientY + 'px';
    }
    // 聚光灯面板内的光照跟踪
    document.querySelectorAll('.spotlight-panel').forEach(panel => {
        const rect = panel.getBoundingClientRect();
        const x = ((e.clientX - rect.left) / rect.width) * 100;
        const y = ((e.clientY - rect.top) / rect.height) * 100;
        panel.style.setProperty('--mx', x + '%');
        panel.style.setProperty('--my', y + '%');
    });
});

// hover 状态：可交互元素上光标变大
document.querySelectorAll('a, button, [data-tilt], .nav-dot').forEach(el => {
    el.addEventListener('mouseenter', () => cursorDot?.classList.add('hovering'));
    el.addEventListener('mouseleave', () => cursorDot?.classList.remove('hovering'));
});
```

**使用场景：** 演讲/演示中引导观众逐行阅读重点内容。记得在 body 上设 `cursor: none;`，并在移动端 `@media (max-width: 768px)` 中隐藏自定义光标。

---

### 3D Tilt on Hover — 悬浮 3D 倾斜

卡片跟随鼠标角度实时倾斜，立体感极强。适合特性展示、产品卡片。

```css
/* 3D 倾斜卡片容器 */
.tilt-card {
    transform-style: preserve-3d;
    transition: transform 0.1s ease, box-shadow 0.3s;
    will-change: transform;
}
.tilt-card:hover {
    box-shadow: 0 20px 60px rgba(120,119,255,0.15),
                0 0 0 1px rgba(120,119,255,0.2);
}
/* 卡片内元素的 3D 层次 — translateZ 越大越"突出" */
.tilt-card .icon  { transform: translateZ(30px); }
.tilt-card h3     { transform: translateZ(20px); }
.tilt-card p      { transform: translateZ(10px); }
```

```javascript
/* 3D Tilt — 鼠标位置驱动卡片旋转 */
document.querySelectorAll('[data-tilt]').forEach(card => {
    card.addEventListener('mousemove', (e) => {
        const rect = card.getBoundingClientRect();
        const x = (e.clientX - rect.left) / rect.width;
        const y = (e.clientY - rect.top) / rect.height;
        const rotateY = (x - 0.5) * 20;  // 左右倾斜 ±10°
        const rotateX = (0.5 - y) * 20;  // 上下倾斜 ±10°
        card.style.transform =
            `perspective(800px) rotateX(${rotateX}deg) rotateY(${rotateY}deg) scale3d(1.03,1.03,1.03)`;
    });
    card.addEventListener('mouseleave', () => {
        card.style.transform = 'perspective(800px) rotateX(0) rotateY(0) scale3d(1,1,1)';
    });
});
```

**使用方式：** 在卡片元素上加 `data-tilt` 属性即可启用。卡片内子元素用 `translateZ()` 制造层次。

---

### SVG Stroke Draw — SVG 描边绘制

图标线条像画笔一笔一笔画出来，有"现场作画"的叙事感。适合图标、流程图、示意图。

```css
/* SVG 描边绘制动画 */
.svg-draw svg path,
.svg-draw svg circle,
.svg-draw svg polyline,
.svg-draw svg line,
.svg-draw svg rect {
    fill: none;
    stroke: var(--accent-color, #7877ff);
    stroke-width: 2;
    stroke-linecap: round;
    stroke-linejoin: round;
    stroke-dasharray: 300;
    stroke-dashoffset: 300;
    transition: stroke-dashoffset 1.5s ease;
}
/* 进入视窗后绘制完成 */
.svg-draw.visible svg path,
.svg-draw.visible svg circle,
.svg-draw.visible svg polyline,
.svg-draw.visible svg line,
.svg-draw.visible svg rect {
    stroke-dashoffset: 0;
}
/* 图标下方标签延迟出现 */
.svg-draw .label {
    opacity: 0;
    transform: translateY(10px);
    transition: all 0.5s ease 1s;
}
.svg-draw.visible .label {
    opacity: 1;
    transform: translateY(0);
}
```

```javascript
/* SVG 描边 — 滚动到视窗时触发，逐个延迟 */
function initSvgDraw() {
    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                const items = entry.target.querySelectorAll('.svg-draw');
                items.forEach((item, i) => {
                    setTimeout(() => item.classList.add('visible'), i * 200);
                });
                observer.unobserve(entry.target);
            }
        });
    }, { threshold: 0.3 });
    document.querySelectorAll('.svg-draw-container').forEach(el => observer.observe(el));
}
initSvgDraw();
```

**注意：** `stroke-dasharray` 值需要大于等于 SVG 路径的实际长度，否则线条画不完。复杂路径可通过 JS `path.getTotalLength()` 获取精确值。

---

### Staggered Entrance — 交错入场

列表/网格项按顺序依次入场，配合讲述节奏逐条展开，避免一次性信息过载。

```css
/* 交错入场 — 初始状态 */
.stagger-item {
    opacity: 0;
    transform: translateY(30px);
    transition: all 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}
/* 进入视窗后显示 */
.stagger-item.visible {
    opacity: 1;
    transform: translateY(0);
}
```

```javascript
/* 交错入场 — 滚动到视窗时逐个显示 */
function initStaggerEntrance() {
    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                const items = entry.target.querySelectorAll('.stagger-item');
                items.forEach((item, i) => {
                    setTimeout(() => item.classList.add('visible'), i * 120);
                });
                observer.unobserve(entry.target);
            }
        });
    }, { threshold: 0.2 });
    document.querySelectorAll('.stagger-container').forEach(el => observer.observe(el));
}
initStaggerEntrance();
```

**使用方式：** 外层容器加 `.stagger-container`，每个子项加 `.stagger-item`。`setTimeout` 的 `i * 120` 控制间隔（120ms），可调整节奏。适用于特性列表、团队介绍、时间线等。

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Fonts not loading | Check Fontshare/Google Fonts URL; ensure font names match in CSS |
| Animations not triggering | Verify Intersection Observer is running; check `.visible` class is being added |
| Scroll snap not working | Ensure `scroll-snap-type: y mandatory` on html; each slide needs `scroll-snap-align: start` |
| Mobile issues | Disable heavy effects at 768px breakpoint; test touch events; reduce particle count |
| Performance issues | Use `will-change` sparingly; prefer `transform`/`opacity` animations; throttle scroll handlers |
