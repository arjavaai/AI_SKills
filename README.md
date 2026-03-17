# OnlyAI Academy — Remotion Video Skills

Three Claude Code skills for generating production-ready, branded Remotion videos — automatically, from a single prompt.

---

## What's Inside

```
AI_Skills/
└── Remotion SKills/
    ├── onlyai-remotion-prompt/      ← Brand identity + prompt generator skill
    ├── bytemonk-system-design/      ← ByteMonk-style architecture diagram skill
    └── remotion-best-practices/     ← Remotion API knowledge skill
```

### `onlyai-remotion-prompt`
Turns a topic/title into a complete Remotion component specification following OnlyAI Academy's brand system:
- Colors: Orange `#FF6B35`, Navy `#080C1A`, Teal `#00D4FF`
- Fonts: Space Grotesk (headings) + Inter (body)
- Layout: Logo bottom-left, badge top-right, content centered
- Topic-adaptive backgrounds (neural nodes, code overlays, glass cards)
- Supports: 16:9 widescreen and 9:16 Instagram Reel

### `bytemonk-system-design`
Generates ByteMonk-style system design explainer animations using OnlyAI Academy's brand colors:
- **ByteMonk's signature style**: dark background, architecture diagrams, animated data flow
- Color-coded service boxes (orange = producers, teal = consumers, blue = databases)
- Flowing particle dots along arrow paths showing live data movement
- Subtle dot-grid background, bold title bar with accent strip
- Progressive component reveal — builds the full architecture step by step
- Use for: "how X works" explanations, microservices, API flows, system architecture

### `remotion-best-practices`
Provides Remotion-specific coding knowledge so Claude writes correct React video code:
- `useCurrentFrame()`, `spring()`, `interpolate()` patterns
- `@remotion/google-fonts` loading
- Sequences, compositions, transitions, timing
- 35+ rule files covering animations, audio, charts, 3D, and more

---

## How to Use

### 1. Install the Skills

```bash
# Install both skills to Claude Code
npx skills add ./onlyai-remotion-prompt
npx skills add ./remotion-best-practices
```

Or copy the folders into your Claude skills directory:
- **Mac/Linux:** `~/.claude/skills/`
- **Windows:** `%APPDATA%\Claude\skills\`

### 2. Set Up a Remotion Project (one-time)

```bash
mkdir my-onlyai-videos
cd my-onlyai-videos
npm init -y
npm install remotion @remotion/cli @remotion/google-fonts react react-dom typescript @types/react @types/react-dom
```

Create `tsconfig.json`:
```json
{
  "compilerOptions": {
    "lib": ["dom", "ES2017"],
    "target": "ES2017",
    "jsx": "react",
    "module": "CommonJS",
    "strict": true,
    "esModuleInterop": true,
    "moduleResolution": "node",
    "skipLibCheck": true
  },
  "include": ["src/**/*.ts", "src/**/*.tsx", "remotion.config.ts"]
}
```

Add scripts to `package.json`:
```json
{
  "scripts": {
    "start": "remotion studio",
    "render": "remotion render"
  }
}
```

### 3. Add Your Logo

Place your logo image in the `public/` folder:
```
my-onlyai-videos/
└── public/
    └── logo.png   ← your OnlyAI Academy logo here
```

In your components, load it with:
```tsx
import { Img, staticFile } from "remotion";

<Img src={staticFile("logo.png")} style={{ width: 180 }} />
```

### 4. Create Videos with Claude

With both skills installed, just describe what you want:

```
"Create a Remotion intro video for 'What is Deep Learning'"
"Make an Instagram Reel for 'Prompt Engineering Basics'"
"Build a lower third for speaker John Smith, AI Engineer at OpenAI"
"Create a section intro card for Module 3: Fine-tuning LLMs, Dark UI style"
"Make a live class banner for 'Building AI Agents in n8n', Class Today at 6PM IST"
```

Claude will automatically:
1. Apply the OnlyAI brand system (colors, fonts, layout)
2. Match the background motif to your topic
3. Write production-ready Remotion React code
4. Use correct Remotion APIs throughout

### 5. Preview and Render

```bash
# Open Remotion Studio to preview
npm run start

# Render to MP4
npx remotion render <CompositionId> out/video.mp4

# Render as Instagram Reel (9:16)
npx remotion render <CompositionIdReel> out/reel.mp4
```

---

## Style Modes

| Mode | Background | Best For |
|------|-----------|----------|
| **AI Workspace** *(default)* | Dark navy + teal/orange glows | AI, ML, LLM topics |
| **Dark UI** | Near-black, minimal | Code, developer topics |
| **Light Glass** | White + glassmorphism | Business, announcements |
| **Gradient Neon** | Orange→Navy gradient | Bold promos, CTAs |

---

## Animation Types

| Type | Duration | Use Case |
|------|----------|----------|
| Course Intro Screen | 5–6s | Course/module openers |
| Live Class Banner | 5–6s | Scheduled live sessions |
| Section Intro | 4–5s | Module section cards |
| Lower Third | 2–3s | Speaker name overlays |
| Title Card | 3–4s | Topic headlines |
| Event Announcement | 5–6s | Upcoming sessions |
| Outro / CTA | 6–8s | End screens |

---

## Brand Colors Reference

```
VIBRANT_ORANGE  = "#FF6B35"   // headings, CTAs, gradient start
WARM_ORANGE     = "#F7931E"   // gradient end, secondary accents
DEEP_BACKGROUND = "#080C1A"   // main background
TEAL_GLOW       = "#00D4FF"   // AI robot glows, node connections
TEAL_SOFT       = "#00C4B4"   // secondary glow
WHITE           = "#FFFFFF"   // titles, body text
ROYAL_BLUE      = "#1E3A8A"   // supporting elements
```

---

## Example Output

Videos generated with these skills:
- `WhatIsMCP.mp4` — 1920×1080, 6s — "What is MCP?"
- `WhatIsML.mp4` — 1920×1080, 6s — "What is Machine Learning?"
- `WhatIsMLReel.mp4` — 1080×1920, 6s — Instagram Reel version

---

## Requirements

- **Node.js** 18+
- **Claude Code** with skills support
- **Remotion** 4.x

---

*Skills built for OnlyAI Academy — [onlyai.academy](https://onlyai.academy)*
