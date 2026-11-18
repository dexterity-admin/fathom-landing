# Fathom

**Find files before you forget them.**

A fast, private, AI-assisted **file intelligence** desktop app. It finds any file by name *and content* in milliseconds, previews it instantly, visualizes what's eating your disk, and helps you organize — all local-first, no cloud required.

---

## 🎯 The Problem

Your drives are full and you can't find anything.

| Drive | Usage | Reality |
|-------|-------|---------|
| **C:** | 97% full | Lost files, forgotten installers, duplicate downloads |
| **D:** | 96% full | Folders that grew without anyone noticing |

Today's tooling fails:
- **Windows Explorer / Finder** — slow recursive search, filename-only, no disk insight
- **Everything (voidtools)** — blazing filename search, but: name-only, no preview, no organization, dated Win32 UI
- **WinDirStat / TreeSize** — disk treemap, but slow, no search, no dedup, ugly interfaces

**Fathom collapses all of that into one beautiful, instant, private app.**

---

## ✨ The Five Differentiators

<details>
<summary><strong>F1. Instant Name Search</strong> — Sub-100ms across millions of files (Free)</summary>
NTFS USN-journal (Windows) + efficient walk (macOS/Linux) indexer. Type-to-filter, fuzzy ranking, match-highlighting, live result count.
</details>

<details>
<summary><strong>F2. Content Search</strong> — Search *what files say*, not just names (Pro)</summary>
Full-text index inside PDF, DOCX, XLSX, PPTX, Markdown, code. Ripgrep-powered. Everything cannot do this.
</details>

<details>
<summary><strong>F3. Smart Preview</strong> — Instant in-app preview of everything (Pro)</summary>
Images (incl. HEIC), PDF first page, code with syntax highlighting, text, markdown render, metadata panel. No opening another app.
</details>

<details>
<summary><strong>F4. Disk Insight Treemap</strong> — Canvas-rendered Squarify treemap (Pro)</summary>
Click-to-drill, top-20 largest files, cleanup suggestions, per-folder size bars. WinDirStat-inspired + better.
</details>

<details>
<summary><strong>F5. Duplicate Finder</strong> — Size-prefilter + BLAKE2b/SHA-256 grouping (Pro)</summary>
Side-by-side preview, safe bulk-to-trash (never hard-delete), export reports.
</details>

<details>
<summary><strong>F6. Auto-Organize</strong> — Rules engine with undo journal (Pro)</summary>
By type/date/size/keyword + local AI suggestions folder structure & renames. Preview-diff before apply. One-click undo.
</details>

---

## 💰 Pricing

| Tier | Monthly | Annual (2 months free) | Best For |
|------|---------|------------------------|----------|
| **Free** | $0 | — | Personal core search, name/path, image/text preview, 10 bookmarks |
| **Pro** | $9.99/mo | $99/yr | Unlimited drives, content search, full preview, treemap, dedup, organize |
| **Team** | $29/user/mo | $290/user/yr | All Pro + shared presets, SSO hook, admin policies, priority support |

**Free is genuinely useful** — Everything-level name search speed, so it spreads organically. Pro unlocks the five differentiators professionals pay for. Team adds the collaboration/billing surface.

---

## 🛠 Tech Stack

- **Electron** — Main + renderer (vanilla HTML/CSS/JS — no React/bundler, lean `node_modules`)
- **electron-builder** — NSIS + MSI (Windows), DMG universal (macOS), AppImage + .deb (Linux)
- **better-sqlite3** — Local persistence for index, settings, tags, saved searches
- **chokidar** — Incremental FS watching & re-indexing
- **winston** — Structured logging
- **node `crypto`** — BLAKE2b / SHA-256 (zero additional native deps)
- **pdf-parse**, **jszip** — Content extraction from documents

---

## 📦 Get Fathom

<div align="center">

| Platform | Download | Requires |
|----------|----------|----------|
| **Windows** | [NSIS Installer](https://github.com/dexterity-admin/Fathom/releases/latest) / [Portable](https://github.com/dexterity-admin/Fathom/releases/latest) | Windows 10 1903+ (64-bit) |
| **macOS** | [Universal DMG](https://github.com/dexterity-admin/Fathom/releases/latest) | macOS 12+ (Intel + Apple Silicon) |
| **Linux** | [AppImage](https://github.com/dexterity-admin/Fathom/releases/latest) / [.deb](https://github.com/dexterity-admin/Fathom/releases/latest) | glibc 2.28+ |

**Source code:** [github.com/dexterity-admin/Fathom](https://github.com/dexterity-admin/Fathom) (private — see below)

</div>

---

## 🔒 Privacy & License

- **Local-first** — Your data never leaves your machine. No telemetry by default.
- **Offline license validation** — HMAC-signed keys, no account required.
- **Business Source License 1.1** — Converts to Apache-2.0 on 2030-11-18.

---

## 🌐 Public Assets

This repo (`fathom-landing`) holds the **public marketing assets**:

- `index.html` — Full cinematic landing page (Three.js hero, animated differentiators, pricing toggle, FAQ)
- `README.md` — This file (animated canvas particle field below)

The **application source** lives in the private repo `dexterity-admin/Fathom`.

---

## 🎨 Animated Canvas — Particle Field

<canvas id="hero-canvas" width="800" height="220" style="width:100%;max-width:800px;height:auto;display:block;margin:24px auto;border-radius:12px;background:#06090f;border:1px solid #1e2a3a;"></canvas>

<script>
(() => {
  const canvas = document.getElementById('hero-canvas');
  const ctx = canvas.getContext('2d');
  let w = 800, h = 220;
  const DPR = Math.min(window.devicePixelRatio || 1, 2);
  const particles = [];
  const PARTICLE_COUNT = 60;
  const ACCENT = '#2FB8A6';
  const MUTED = '#8A98B4';
  const BG = '#06090F';

  function resize() {
    const rect = canvas.getBoundingClientRect();
    w = Math.min(rect.width, 800);
    h = 220;
    canvas.width = w * DPR;
    canvas.height = h * DPR;
    canvas.style.width = w + 'px';
    canvas.style.height = h + 'px';
    ctx.scale(DPR, DPR);
  }

  class Particle {
    constructor() { this.reset(); }
    reset() {
      this.x = Math.random() * w;
      this.y = Math.random() * h;
      this.vx = (Math.random() - 0.5) * 0.3;
      this.vy = (Math.random() - 0.5) * 0.3;
      this.r = Math.random() * 1.5 + 0.5;
      this.alpha = Math.random() * 0.5 + 0.1;
      this.hue = Math.random() > 0.7 ? 160 : 210; // teal or muted blue
    }
    update() {
      this.x += this.vx;
      this.y += this.vy;
      if (this.x < 0 || this.x > w || this.y < 0 || this.y > h) this.reset();
    }
    draw() {
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2);
      const c = this.hue === 160 ? ACCENT : MUTED;
      ctx.fillStyle = c.replace(')', `, ${this.alpha})`).replace('rgb', 'rgba').replace('#', '');
      // Simple hex to rgba
      let r, g, b;
      if (this.hue === 160) { r=47; g=184; b=166; } else { r=138; g=152; b=180; }
      ctx.fillStyle = `rgba(${r},${g},${b},${this.alpha})`;
      ctx.fill();
    }
  }

  for (let i = 0; i < PARTICLE_COUNT; i++) particles.push(new Particle());

  let mouse = { x: -1000, y: -1000 };
  canvas.addEventListener('mousemove', e => {
    const rect = canvas.getBoundingClientRect();
    mouse.x = (e.clientX - rect.left) * (w / rect.width);
    mouse.y = (e.clientY - rect.top) * (h / rect.height);
  });
  canvas.addEventListener('mouseleave', () => { mouse.x = -1000; mouse.y = -1000; });

  function drawConnections() {
    for (let i = 0; i < particles.length; i++) {
      for (let j = i + 1; j < particles.length; j++) {
        const dx = particles[i].x - particles[j].x;
        const dy = particles[i].y - particles[j].y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (dist < 120) {
          const opacity = (1 - dist / 120) * 0.15;
          ctx.beginPath();
          ctx.moveTo(particles[i].x, particles[i].y);
          ctx.lineTo(particles[j].x, particles[j].y);
          ctx.strokeStyle = `rgba(47,184,166,${opacity})`;
          ctx.lineWidth = 0.5;
          ctx.stroke();
        }
      }
      // Mouse connection
      const mdx = particles[i].x - mouse.x;
      const mdy = particles[i].y - mouse.y;
      const mdist = Math.sqrt(mdx * mdx + mdy * mdy);
      if (mdist < 180) {
        const opacity = (1 - mdist / 180) * 0.25;
        ctx.beginPath();
        ctx.moveTo(particles[i].x, particles[i].y);
        ctx.lineTo(mouse.x, mouse.y);
        ctx.strokeStyle = `rgba(47,184,166,${opacity})`;
        ctx.lineWidth = 0.8;
        ctx.stroke();
      }
    }
  }

  function animate() {
    ctx.clearRect(0, 0, w, h);
    drawConnections();
    for (const p of particles) {
      p.update();
      p.draw();
    }
    requestAnimationFrame(animate);
  }

  resize();
  animate();
  window.addEventListener('resize', resize);
})();
</script>

---

Built with ☕ by **Fathom Labs** — [Privacy](https://fathom.app/privacy) · [Terms](https://fathom.app/terms) · [Contact](https://fathom.app/contact)