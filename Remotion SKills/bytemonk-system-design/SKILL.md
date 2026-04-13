---
name: bytemonk-system-design
description: >
  Generate ByteMonk-style system design explanation videos as Remotion React components using OnlyAI Academy's v2 brand (orange #FF5500, warm dark #0d0b08, Playfair Display + Inter fonts, warm amber glassmorphism, orange sphere, animated mesh background).
  ByteMonk's signature style: dark warm background, animated architecture diagrams with warm amber-glass service boxes,
  flowing orange data arrows between components, subtle warm dot-grid background, bold Playfair Display topic title.
  Use this skill whenever the user wants to create a system design animation, architecture explainer video,
  tech concept breakdown, "how X works" video, or any educational coding/engineering topic video.
  Also trigger when user mentions: system design, architecture diagram, microservices, API flow, database design,
  "ByteMonk style", "explain how X works", tech explainer, or wants a Remotion video for a technical topic.
---

# ByteMonk System Design — Remotion Skill (v2 Design)

Generate animated system design explainer videos in ByteMonk's signature style, powered by OnlyAI Academy's v2 brand design system.

## What This Style Looks Like

ByteMonk's videos explain technical systems by progressively revealing architecture diagrams:
- Components (service boxes) **fade/slide in** one by one with warm glass styling
- **Animated data flow arrows** draw between connected services in orange/amber
- **Flowing orange particles/dots** travel along paths to show live data movement
- Bold **Playfair Display topic title** at the top with orange italic accent
- Subtle **warm dot-grid background** for depth
- **Orange sphere** glowing in the bottom-right corner (brand signature)
- Color-coded warm-glass boxes by service type for instant visual comprehension

---

## OnlyAI Academy Brand Colors (v2)

```
// Core palette
PAGE_BG         = "#0d0b08"                    // warm dark background (NOT blue-black)
ORANGE          = "#FF5500"                    // primary accent — triggers, CTAs, highlights
WARM_ORANGE     = "#ff8040"                    // secondary accent — labels, pills, sublabels

// Glass surfaces (warm amber — NO teal/cyan)
GLASS_BG        = "rgba(18,12,8,0.62)"         // standard glass card fill
GLASS_BG2       = "rgba(24,16,10,0.72)"        // denser glass surface
GLASS_BORDER    = "rgba(255,180,100,0.18)"     // warm amber card border
GLASS_SHINE     = "rgba(255,210,160,0.25)"     // top-edge shine highlight
GLASS_SHADOW    = "0 20px 60px rgba(0,0,0,0.60)"

// Orange-glass (active/highlighted boxes)
OR_SOFT         = "rgba(255,85,0,0.18)"        // orange-glass fill
OR_BORDER       = "rgba(255,85,0,0.40)"        // orange-glass border
OR_SHINE        = "rgba(255,160,80,0.50)"      // orange-glass top shine
OR_GLOW         = "rgba(255,85,0,0.15)"        // orange ambient glow

// Text
WHITE           = "#ffffff"
FG_MUTED        = "rgba(255,255,255,0.68)"     // secondary text
FG_DIM          = "rgba(255,255,255,0.50)"     // hint/dim text

// Code blocks
CODE_BG         = "rgba(20,12,6,0.75)"
CODE_TEXT       = "rgba(220,210,195,0.85)"

// Status colors (for comparison / result states)
SUCCESS         = "#2ed573"
WARNING         = "#f59e0b"
PURPLE          = "#a78bfa"
ERROR           = "rgba(231,100,90,0.90)"
```

> ⚠️ **Teal (`#00D4FF`) is no longer used.** All flow lines, particles, and consumer boxes now use warm orange/amber tones.

---

## Service Color Coding (v2 — Warm Palette)

| Service Type | Box Fill | Border | Glow | Role Color Name |
|---|---|---|---|---|
| Producer / Trigger / Client | `rgba(255,85,0,0.18)` | `rgba(255,85,0,0.40)` | `rgba(255,85,0,0.20)` | **ORANGE** |
| Broker / Queue / Bus | `rgba(255,128,64,0.18)` | `rgba(255,128,64,0.40)` | `rgba(255,128,64,0.18)` | **WARM ORANGE** |
| Consumer / Worker / Service | `rgba(245,158,11,0.15)` | `rgba(245,158,11,0.40)` | `rgba(245,158,11,0.15)` | **AMBER** |
| Database / Storage | `rgba(139,92,246,0.12)` | `rgba(139,92,246,0.35)` | `rgba(139,92,246,0.15)` | **PURPLE** |
| Cache / CDN / Fast layer | `rgba(46,213,115,0.10)` | `rgba(46,213,115,0.30)` | `rgba(46,213,115,0.12)` | **GREEN** |
| External API / Third Party | `rgba(255,255,255,0.06)` | `rgba(255,180,100,0.18)` | none | **GLASS NEUTRAL** |

Flow arrows between boxes: `#FF5500` or `rgba(255,160,80,0.80)` amber — never teal.
Data particles traveling along arrows: `rgba(255,85,0,0.80)` orange dots.

---

## Typography (v2)

```tsx
// Load fonts at the top of the component file:
import { loadFont as loadPlayfairDisplay } from "@remotion/google-fonts/PlayfairDisplay";
import { loadFont as loadInter } from "@remotion/google-fonts/Inter";

const { fontFamily: playfairDisplay } = loadPlayfairDisplay();
const { fontFamily: inter } = loadInter();

// Usage:
// Topic title:     PlayfairDisplay, weight 700, ~72–96px, white — key word italic + #FF5500
// Section labels:  PlayfairDisplay, weight 600, ~28–36px, white
// Service labels:  Inter, weight 500–600, ~18–24px, white
// Sublabels:       Inter, weight 300–400, ~13–16px, rgba(255,255,255,0.68)
// Badge pills:     Inter, weight 500, ~14–18px, #ff8040, uppercase
```

> ⚠️ **Space Grotesk is no longer used.** All display text uses **Playfair Display**.

---

## Component Architecture

### Composition Setup
```tsx
// 16:9 widescreen: 1920×1080, 30fps
// Duration: ~30f per service box + 60f title + 60f final hold
// Instagram Reel (9:16): 1080×1920 — stack components vertically
```

### Standard Layout (16:9)

```
┌──────────────────────────────────────────────────────────┐
│  [TITLE BAR] "How X Works"    [badge pill: System Design] │  ← top 120px
│  Playfair Display 700, white + italic orange word         │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  [Box A: ORANGE]  ──orange──▶  [Box B: AMBER]  ──▶ ...  │  ← center diagram
│      dots●●●                       dots●●●               │
│                        ↓                                 │
│               [Box D: PURPLE — DB]                       │
│                                                          │
└──────────────────────────────────────────────────────────┘
   [warm dot-grid bg]          [orange sphere bottom-right]
```

---

## Animation Sequence

### Timing Template (30fps)
```
Frame 0–20:    PAGE_BG fade in + mesh bg gradients build
Frame 0–30:    Warm dot-grid fades in (opacity 0→0.06)
Frame 0–45:    Orange sphere enters (scale 0→1, spring damping:200)
Frame 0–45:    Title bar slides down from top (spring damping:200)
Frame 30–60:   First service box scales in
Frame 60–90:   Arrow A→B draws (strokeDashoffset) + Box B appears
Frame 90–120:  Arrow B→C draws + Box C appears
Frame 120–150: Data flow orange particles begin looping along arrows
Frame 150–180: Final hold — all visible + sphere subtle pulse continues
```

For more services, add ~30 frames per additional box.

---

## Key Animation Patterns (v2 — Updated Code)

**Service box entrance (warm glass):**
```tsx
const boxOpacity = interpolate(frame, [startFrame, startFrame + 20], [0, 1], { extrapolateRight: "clamp" });
const boxScale = spring({ frame: Math.max(0, frame - startFrame), fps, config: { damping: 200 } });

<div style={{
  position: "absolute", left: x, top: y, width: w, height: h,
  background: boxBg,                // e.g. "rgba(255,85,0,0.18)"
  border: `1.5px solid ${boxBorder}`,  // e.g. "rgba(255,85,0,0.40)"
  borderTop: `1.5px solid ${boxShine}`,
  borderRadius: 22,
  backdropFilter: "blur(28px) saturate(200%)",
  boxShadow: `0 20px 40px ${boxGlow}, inset 0 1px 0 ${boxShine}`,
  display: "flex", flexDirection: "column",
  alignItems: "center", justifyContent: "center",
  transform: `scale(${boxScale})`, opacity: boxOpacity,
}}>
  {icon && <span style={{ fontSize: 32, marginBottom: 8 }}>{icon}</span>}
  <span style={{ fontFamily: inter, fontSize: 20, fontWeight: 600, color: "#ffffff" }}>{label}</span>
  {sublabel && <span style={{ fontFamily: inter, fontSize: 13, fontWeight: 300, color: "rgba(255,255,255,0.68)", marginTop: 4 }}>{sublabel}</span>}
</div>
```

**Arrow draw animation (orange, strokeDashoffset):**
```tsx
const drawProgress = interpolate(frame, [startFrame, startFrame + 25], [totalLength, 0], {
  extrapolateLeft: "clamp", extrapolateRight: "clamp",
});
// SVG line — orange color, no teal
<line
  x1={x1} y1={y1} x2={x2} y2={y2}
  stroke="#FF5500"
  strokeWidth={2}
  strokeDasharray={totalLength}
  strokeDashoffset={drawProgress}
  markerEnd="url(#arrowOrange)"
/>
// Arrow marker definition:
<defs>
  <marker id="arrowOrange" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
    <path d="M0,0 L0,6 L8,3 z" fill="#FF5500" />
  </marker>
</defs>
```

**Flowing data particles (orange dots on arrows):**
```tsx
// Orange particles — no teal
{[0, 0.33, 0.66].map((phase, i) => {
  const pos = ((frame * 0.015) + phase) % 1;
  const x = startX + (endX - startX) * pos;
  const y = startY + (endY - startY) * pos;
  const visible = frame > arrowStartFrame + 20;
  return (
    <circle key={i} cx={x} cy={y} r={5}
      fill="rgba(255,85,0,0.80)"
      opacity={visible ? interpolate(Math.sin(pos * Math.PI), [0,1],[0.4,1], { extrapolateLeft: "clamp", extrapolateRight: "clamp" }) : 0}
    />
  );
})}
```

**Warm dot-grid background:**
```tsx
const WarmDotGrid: React.FC<{ opacity: number }> = ({ opacity }) => (
  <svg style={{ position: "absolute", inset: 0, opacity }} width="1920" height="1080">
    {Array.from({ length: 40 }).map((_, row) =>
      Array.from({ length: 70 }).map((_, col) => (
        <circle
          key={`${row}-${col}`}
          cx={col * 28 + 14}
          cy={row * 28 + 14}
          r={1.5}
          fill="rgba(255,180,100,0.08)"  // warm amber dots, not white
        />
      ))
    )}
  </svg>
);
```

**Orange sphere (bottom-right — brand signature):**
```tsx
const spherePulse = interpolate(
  (frame % 240) / 240,
  [0, 0.5, 1],
  [1, 1.05, 0.97]
);
const sphereX = interpolate((frame % 240) / 240, [0, 0.5, 1], [0, -12, 8]);
const sphereY = interpolate((frame % 240) / 240, [0, 0.5, 1], [0, -8, 12]);

<div style={{
  position: "absolute",
  width: 680, height: 680,
  right: -160, bottom: -160,
  borderRadius: "50%",
  background: `radial-gradient(circle at 38% 38%,
    rgba(255,110,20,0.92) 0%,
    rgba(220,65,0,0.78) 28%,
    rgba(160,35,0,0.50) 52%,
    rgba(90,15,0,0.22) 72%,
    transparent 88%)`,
  filter: "blur(2px)",
  transform: `scale(${spherePulse}) translate(${sphereX}px, ${sphereY}px)`,
}} />
```

**Title bar (Playfair Display, v2 style):**
```tsx
const titleY = interpolate(
  spring({ frame, fps, config: { damping: 200 } }),
  [0, 1], [-120, 0]
);

<div style={{
  position: "absolute", top: 0, left: 0, right: 0, height: 110,
  background: "linear-gradient(90deg, rgba(255,85,0,0.18) 0%, rgba(0,0,0,0) 100%)",
  borderBottom: "1px solid rgba(255,85,0,0.40)",
  backdropFilter: "blur(16px)",
  transform: `translateY(${titleY}px)`,
  display: "flex", alignItems: "center", justifyContent: "space-between",
  padding: "0 60px",
}}>
  <span style={{ fontFamily: playfairDisplay, fontSize: 52, fontWeight: 700, color: "#ffffff" }}>
    How <em style={{ color: "#FF5500", fontStyle: "italic" }}>X</em> Works
  </span>
  {/* Badge pill — orange-glass, no teal */}
  <div style={{
    background: "rgba(255,85,0,0.18)",
    border: "1px solid rgba(255,85,0,0.40)",
    borderTop: "1px solid rgba(255,160,80,0.50)",
    borderRadius: 999, padding: "7px 18px",
    fontFamily: inter, fontSize: 14, fontWeight: 500,
    color: "#ff8040", letterSpacing: "1.4px", textTransform: "uppercase",
  }}>System Design</div>
</div>
```

**ServiceBox component (reusable, v2):**
```tsx
interface ServiceBoxProps {
  label: string; sublabel?: string; icon?: string;
  boxBg: string; boxBorder: string; boxShine: string; boxGlow: string;
  x: number; y: number; w: number; h: number;
  startFrame: number; frame: number; fps: number;
}

const ServiceBox: React.FC<ServiceBoxProps> = ({
  label, sublabel, icon, boxBg, boxBorder, boxShine, boxGlow,
  x, y, w, h, startFrame, frame, fps
}) => {
  const progress = spring({ frame: Math.max(0, frame - startFrame), fps, config: { damping: 200 } });
  const opacity = interpolate(frame, [startFrame, startFrame + 15], [0, 1], { extrapolateRight: "clamp" });
  return (
    <div style={{
      position: "absolute", left: x, top: y, width: w, height: h,
      background: boxBg,
      border: `1.5px solid ${boxBorder}`,
      borderTop: `1.5px solid ${boxShine}`,
      borderRadius: 22,
      backdropFilter: "blur(28px) saturate(200%)",
      boxShadow: `0 20px 40px ${boxGlow}, inset 0 1px 0 ${boxShine}`,
      display: "flex", flexDirection: "column",
      alignItems: "center", justifyContent: "center",
      transform: `scale(${progress})`, opacity,
    }}>
      {icon && <span style={{ fontSize: 32, marginBottom: 8 }}>{icon}</span>}
      <span style={{ fontFamily: inter, fontSize: 20, fontWeight: 600, color: "#ffffff" }}>{label}</span>
      {sublabel && <span style={{ fontFamily: inter, fontSize: 13, fontWeight: 300, color: "rgba(255,255,255,0.68)", marginTop: 4 }}>{sublabel}</span>}
    </div>
  );
};
```

---

## Animation Types

| Type | Description | Duration |
|---|---|---|
| **System Flow** | Full architecture diagram with orange data flow | 6–8s (180–240f) |
| **Step-by-Step** | One component at a time, builds full picture | 8–10s |
| **Comparison** | Two approaches side by side (orange left, amber right) | 6s |
| **Timeline** | Sequence of events top-to-bottom with orange spine | 5–6s |
| **Request/Response** | Single API call traced through the system | 5s |

---

## Full Prompt Output Format

When a user provides a topic, output a Remotion prompt in this structure:

```
**Topic:** [What they asked for]
**Style:** ByteMonk System Design — OnlyAI Academy v2 (Warm Dark)
**Composition:** [width]×[height], [fps]fps, [frames] frames ([duration]s)

**Architecture to animate:**
1. [Service A] (Producer — ORANGE: rgba(255,85,0,0.18) / #FF5500 border)
2. [Service B] (Broker — WARM ORANGE: rgba(255,128,64,0.18) / #ff8040 border)
3. [Service C] (Consumer — AMBER: rgba(245,158,11,0.15) / #f59e0b border)
4. [Service D] (Database — PURPLE: rgba(139,92,246,0.12) / #a78bfa border)

**Flow:** A → B → C → D (orange arrows + orange particle dots)

**Animation timeline:**
- 0–45f: PAGE_BG + warm dot-grid + orange sphere + title bar all enter
- 45–75f: Service A scales in
- 75–105f: Orange arrow A→B draws + Service B scales in
- 105–135f: Arrow B→C draws + Service C appears
- 135–165f: Arrow C→D draws + DB box appears
- 165–240f: All visible + orange particles loop + sphere pulses

**Title bar:** "How [Topic] Works" (Playfair Display italic orange accent) | badge: "System Design"
**Background:** #0d0b08 + warm dot-grid (rgba(255,180,100,0.08)) + orange sphere bottom-right
**Fonts:** PlayfairDisplay 700 (title) + Inter 500 (labels) — NO Space Grotesk
**Special effects:** [topic-specific glows, pulses, particle effects]
```

---

## Style Modes (v2)

| Mode | Use For | Background | Accent |
|---|---|---|---|
| **Warm Dark** *(default)* | All system design topics | `#0d0b08` + orange sphere + mesh | Orange/amber flow lines |
| **Dark Minimal** | Clean diagrams, no decoration | `#080604`, no sphere/mesh | Sharp `#FF5500` orange lines |
| **Comparison** | Before/after, approach A vs B | `#0d0b08` | Orange left, Amber right |
| **Timeline** | Process steps, event sequences | `#0d0b08` | Orange spine with amber steps |

---

## Important Remotion Notes

- Always import `useCurrentFrame`, `useVideoConfig`, `spring`, `interpolate`, `AbsoluteFill` from `"remotion"`
- Use `useCurrentFrame()` and `useVideoConfig()` inside the component — never as module-level variables
- Arrow animations use SVG `<line>` or `<path>` with `strokeDasharray` / `strokeDashoffset`
- Arrow and particle color is **always orange** (`#FF5500` or `rgba(255,85,0,0.80)`) — never teal
- Orange sphere is a pure CSS `div` with `radial-gradient` — animate with `interpolate` on frame
- For particle dots, drive position with `frame` directly (no state/useEffect) — pure math
- All animations must be deterministic functions of `frame` for Remotion's rendering model
- Use `extrapolateLeft: "clamp"` and `extrapolateRight: "clamp"` on all `interpolate` calls
- Glass cards: `backdropFilter: "blur(28px) saturate(200%)"` — use `style` prop, not className
- Refer to the `remotion-best-practices` skill for detailed API patterns
