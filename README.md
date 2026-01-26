# 📸 VibeCam

**Transform your everyday moments into stunning polaroid-style memories with beautiful aesthetic filters — all in your browser.**

VibeCam is a beginner-friendly web application that lets you capture photos directly from your camera, apply gorgeous aesthetic filters in real-time, and download your creations as beautiful polaroid-style images. Best of all? Everything happens locally on your device — your photos never leave your browser.

## ✨ Features

- 📷 **Live Camera Capture** — Use your device's camera to capture moments in real-time
- 🎨 **5 Aesthetic Filters** — Choose from Vintage, Warm, Cool, Noir, and Soft filter styles
- 🎭 **Polaroid Effect** — Add authentic polaroid frame styling to your photos
- 🔒 **100% Private** — All processing happens locally; no uploads, no tracking, no data collection
- ⚡ **Instant Preview** — Apply filters and see changes in real-time
- 🎯 **User-Friendly** — Simple, intuitive interface designed for everyone
- 📱 **Fully Responsive** — Works beautifully on mobile, tablet, and desktop
- 🆓 **Free Forever** — No subscriptions, no sign-ups, no hidden fees

## 🛠️ Tech Stack

- **Vue 3** (Composition API) — Progressive JavaScript framework
- **TypeScript** — Type-safe development
- **Vite** — Lightning-fast build tool and dev server
- **Tailwind CSS v3** — Utility-first styling framework
- **PostCSS** — CSS transformation and processing
- **ogl** — WebGL library for animated aurora background
- **Canvas API** — Image processing and filter rendering
- **MediaDevices API** — Camera access and streaming

## 🚀 Quick Start

### Prerequisites
- Node.js 16+ installed
- npm or yarn package manager

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/vibecam.git
   cd vibecam
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start the development server**
   ```bash
   npm run dev
   ```
   The app will be available at `http://localhost:5174`

4. **Build for production**
   ```bash
   npm run build
   ```

5. **Preview production build**
   ```bash
   npm run preview
   ```

## 📖 How It Works

### 1. **Capture**
Click the "Start Creating" button or scroll to the camera section and tap "Capture" to open your device's camera. Your browser will request camera permission — this is safe and required only once.

### 2. **Choose a Filter**
Select from 5 aesthetic filters:
- **Vintage** — Warm sepia tones for a nostalgic feel
- **Warm** — Golden hues for a sunny, cheerful vibe
- **Cool** — Blue tones for a calm, serene aesthetic
- **Noir** — High contrast black and white for drama
- **Soft** — Dreamy pastels for a gentle, romantic look

### 3. **Capture & Download**
Take your photo with the selected filter already applied. Preview it, and if you like it, download as a polaroid-style PNG image. Want to try again? Simply retake!

## 📁 Project Structure

```
vibecam/
├── src/
│   ├── components/
│   │   ├── Navbar.vue              # Top navigation with scroll awareness
│   │   ├── Aurora.vue              # WebGL animated background
│   │   ├── HeroSection.vue         # Landing section with CTA
│   │   ├── FilterSection.vue       # 5 filter previews (polaroid cards)
│   │   ├── AboutSection.vue        # Features, stats, and why VibeCam
│   │   ├── CameraCallToAction.vue  # Camera section heading
│   │   ├── CameraView.vue          # Full camera modal with capture
│   │   └── Footer.vue              # Footer with links and info
│   ├── App.vue                     # Root component
│   ├── main.ts                     # App entry point
│   └── style.css                   # Global styles (scrollbar, transitions)
├── public/                         # Static assets
├── index.html                      # HTML entry point
├── package.json                    # Dependencies & scripts
├── tsconfig.json                   # TypeScript configuration
├── tailwind.config.js              # Tailwind CSS customization
├── vite.config.ts                  # Vite configuration
└── README.md                       # This file
```

## 🎨 Design System

### Colors
- **Soft Pink** — `#f78fb3` — Primary accent
- **Soft Purple** — `#9b5de5` — Secondary accent
- **Soft Blue** — `#7c3aed` — Tertiary accent

### Typography
- **Headings** — Black, 900 weight, leading tightness
- **Body Text** — Gray-600, light weight, relaxed leading
- **Buttons** — Bold, full-width on mobile, responsive

### Components
- **Outlined cards** with 2px borders and backdrop blur
- **Glassmorphism** effects for depth
- **Gradient backgrounds** for visual hierarchy
- **Smooth transitions** for interactivity
- **Custom scrollbar** with gradient glow effect

## 🔧 Key Components

### CameraView
The heart of VibeCam — handles camera stream, filter application, polaroid effect rendering, and image download.

**Features:**
- Real-time video stream from user device
- Canvas-based filter processing (5 different filter algorithms)
- Polaroid frame rendering with white border and shadow
- Auto-play handling and loading states
- Error handling for camera access denial

### FilterSection
Displays 5 aesthetic filter preview cards with polaroid styling and hover animations.

**Filters Implemented:**
- Vintage (sepia tone matrix)
- Warm (red/green boost, blue reduction)
- Cool (red reduction, blue boost)
- Noir (high contrast B&W with gamma adjustment)
- Soft (pastel effect with brightness boost)

### Aurora
WebGL-powered animated background creating smooth, flowing gradient waves using the ogl library.

## 🌐 Browser Support

- **Chrome/Edge** — Full support
- **Firefox** — Full support
- **Safari** — Full support (iOS 14.5+)
- **Mobile browsers** — Full support with responsive layout

**Camera Requirements:**
- HTTPS required (except localhost)
- Camera permission must be granted
- Modern browser with WebRTC support

## 💾 Local Storage

VibeCam uses **zero persistent storage**:
- No cookies
- No local storage
- No IndexedDB
- No data retention
- Session-only processing

All photo processing happens in memory and is cleared when you close the tab.

## 🎯 Performance

- **Bundle Size** — ~150KB gzipped
- **Load Time** — <2s on 4G
- **Filter Processing** — Real-time Canvas rendering
- **Memory** — Efficient stream cleanup on close

## 📝 Scripts

```bash
# Development
npm run dev          # Start dev server with HMR

# Production
npm run build        # Build for production
npm run preview      # Preview production build locally

# Type Checking
npm run type-check   # Run TypeScript compiler check
```

## 👨‍💻 Author

Created by Angela Bera

---

**Made with 💜 for everyone who loves capturing moments.**

Transform your everyday photos into memories. No uploads. No tracking. Just pure creative joy. 📸✨
