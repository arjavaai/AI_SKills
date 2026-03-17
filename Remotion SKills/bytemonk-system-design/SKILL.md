---
name: bytemonk-system-design
description: >
  Generate ByteMonk-style system design explanation videos as Remotion React components using OnlyAI Academy's brand colors.
  ByteMonk's signature style: dark near-black background, animated architecture diagrams with color-coded service boxes,
  flowing data arrows between components, subtle dot-grid background, bold topic title with accent color highlight.
  Use this skill whenever the user wants to create a system design animation, architecture explainer video,
  tech concept breakdown, "how X works" video, or any educational coding/engineering topic video.
  Also trigger when user mentions: system design, architecture diagram, microservices, API flow, database design,
  "ByteMonk style", "explain how X works", tech explainer, or wants a Remotion video for a technical topic.
---

# ByteMonk System Design — Remotion Skill

Generate animated system design explainer videos in ByteMonk's signature style, powered by OnlyAI Academy's brand colors.

## What This Style Looks Like

ByteMonk's videos explain technical systems by progressively revealing architecture diagrams:
- Components (service boxes) **fade/slide in** one by one
- **Animated data flow lines** draw between connected services
- **Flowing particles/dots** travel along paths to show live data movement
- Bold **topic title** at the top with a bright accent strip
- Subtle **dot-grid background** for depth
- Color-coded boxes by service type for instant visual comprehension

## OnlyAI Academy Brand Colors

```
BACKGROUND      = "#080C1A"   // deep navy-black (main canvas)
ORANGE          = "#FF6B35"   // primary accent — producers, triggers, CTAs
WARM_ORANGE     = "#F7931E"   // gradient partner — secondary services
TEAL            = "#00D4FF"   // data flow lines, consumers, highlights
TEAL_SOFT       = "#00C4B4"   // secondary teal elements
WHITE           = "#FFFFFF"   // labels, titles, service names
PANEL_BG        = "rgba(255,255,255,0.06)"  // glassmorphic service boxes
PANEL_BORDER    = "rgba(255,255,255,0.12)"  // box borders
ORANGE_GLOW     = "rgba(255,107,53,0.3)"    // orange box shadow/glow
TEAL_GLOW       = "rgba(0,212,255,0.3)"     // teal glow for flow lines
```

**Service Color Coding (use consistently):**
| Service Type | Fill Color | Border |
|---|---|---|
| Producer / Trigger / Client | `#FF6B35` orange | `#FF6B35` |
| Broker / Queue / Bus | `#F7931E` warm orange | `#F7931E` |
| Consumer / Worker / Service | `#00D4FF` teal | `#00D4FF` |
| Database / Storage | `#1E3A8A` royal blue | `#00D4FF` |
| Cache / CDN / Fast layer | `#00C4B4` soft teal | `#00C4B4` |
| External API / Third Party | `rgba(255,255,255,0.15)` | white |

## Typography

```tsx
// Load fonts at the top of the component file:
import { loadFont as loadSpaceGrotesk } from "@remotion/google-fonts/SpaceGrotesk";
import { loadFont as loadInter } from "@remotion/google-fonts/Inter";

const { fontFamily: spaceGrotesk } = loadSpaceGrotesk();
const { fontFamily: inter } = loadInter();

// Usage:
// Topic title: SpaceGrotesk, weight 700, ~72–96px, white or orange
// Section labels: SpaceGrotesk, weight 600, ~28–36px
// Service box labels: Inter, weight 600, ~18–24px
// Sublabels / arrows: Inter, weight 400, ~14–16px, rgba(255,255,255,0.6)
```

## Component Architecture

### Composition Setup
```tsx
// 16:9 widescreen: 1920×1080, 30fps
// Duration depends on # of components: roughly 30f per service + 60f for title + 60f hold
// Instagram Reel (9:16): 1080×1920 — stack components vertically
```

### Standard Layout (16:9)

```
┌─────────────────────────────────────────────────────┐
│  [TITLE BAR] "How X Works"   [badge: system design] │  ← top 120px
├─────────────────────────────────────────────────────┤
│                                                     │
│   [Box A] ──────────▶ [Box B] ──────────▶ [Box C]  │  ← center diagram
│              dots●●●             dots●●●            │
│                   ↓                                 │
│              [Box D: DB]                            │
│                                                     │
└─────────────────────────────────────────────────────┘
│  [dot grid background throughout]                   │
└─────────────────────────────────────────────────────┘
```

## Animation Sequence

### Timing Template (30fps)
```
Frame 0–30:    Background + dot grid fade in
Frame 0–45:    Title bar slides down from top  (spring damping:200)
Frame 30–60:   First service box fades + scales in
Frame 60–90:   Second service box appears, connection arrow draws
Frame 90–120:  Third box, more arrows
Frame 120–150: Data flow particles begin animating along paths
Frame 150–180: Final hold with all elements visible + subtle pulse
```

For more services, add ~30 frames per additional box.

### Key Animation Patterns

**Service box entrance (use spring):**
```tsx
const boxOpacity = interpolate(frame, [startFrame, startFrame + 20], [0, 1]);
const boxScale = spring({ frame: frame - startFrame, fps, config: { damping: 200 } });
// Apply: opacity: boxOpacity, transform: `scale(${boxScale})`
```

**Arrow draw animation (strokeDashoffset trick):**
```tsx
// SVG line with strokeDasharray = totalLength, strokeDashoffset animates to 0
const drawProgress = interpolate(frame, [startFrame, startFrame + 25], [totalLength, 0], {
  extrapolateRight: "clamp",
});
// <line strokeDasharray={totalLength} strokeDashoffset={drawProgress} />
```

**Flowing data particles (dots traveling along a path):**
```tsx
// Multiple offset-phased dots on the same arrow path
// Use modulo to loop: const pos = ((frame * speed) + offset) % 1
// Lerp between start and end coordinates using pos
{[0, 0.33, 0.66].map((phase, i) => {
  const pos = ((frame * 0.015) + phase) % 1;
  const x = startX + (endX - startX) * pos;
  const y = startY + (endY - startY) * pos;
  const visible = frame > arrowStartFrame + 20;
  return (
    <circle key={i} cx={x} cy={y} r={5}
      fill="#00D4FF"
      opacity={visible ? interpolate(Math.sin(pos * Math.PI), [0,1],[0.4,1]) : 0}
    />
  );
})}
```

**Dot grid background:**
```tsx
const DotGrid: React.FC<{ opacity: number }> = ({ opacity }) => (
  <svg style={{ position: "absolute", inset: 0, opacity }} width="1920" height="1080">
    {Array.from({ length: 40 }).map((_, row) =>
      Array.from({ length: 70 }).map((_, col) => (
        <circle
          key={`${row}-${col}`}
          cx={col * 28 + 14}
          cy={row * 28 + 14}
          r={1.5}
          fill="rgba(255,255,255,0.08)"
        />
      ))
    )}
  </svg>
);
```

**Title bar:**
```tsx
// Slide down from top
const titleY = interpolate(
  spring({ frame, fps, config: { damping: 200 } }),
  [0, 1], [-120, 0]
);
// Structure: full-width bar, left: topic title, right: badge pill
<div style={{
  position: "absolute", top: 0, left: 0, right: 0, height: 110,
  background: "linear-gradient(90deg, rgba(255,107,53,0.15) 0%, rgba(0,0,0,0) 100%)",
  borderBottom: "1px solid rgba(255,107,53,0.3)",
  transform: `translateY(${titleY}px)`,
  display: "flex", alignItems: "center", justifyContent: "space-between",
  padding: "0 60px",
}}>
  <span style={{ fontFamily: spaceGrotesk, fontSize: 52, fontWeight: 700, color: "#FFFFFF" }}>
    How <span style={{ color: "#FF6B35" }}>X</span> Works
  </span>
  <div style={{
    background: "rgba(0,212,255,0.15)", border: "1px solid #00D4FF",
    borderRadius: 40, padding: "8px 24px",
    fontFamily: inter, fontSize: 18, color: "#00D4FF", fontWeight: 600,
  }}>System Design</div>
</div>
```

**Service box component:**
```tsx
const ServiceBox: React.FC<{
  label: string; sublabel?: string; icon?: string;
  color: string; x: number; y: number; w: number; h: number;
  startFrame: number; frame: number; fps: number;
}> = ({ label, sublabel, icon, color, x, y, w, h, startFrame, frame, fps }) => {
  const progress = spring({ frame: Math.max(0, frame - startFrame), fps, config: { damping: 200 } });
  const opacity = interpolate(frame, [startFrame, startFrame + 15], [0, 1], { extrapolateRight: "clamp" });
  return (
    <div style={{
      position: "absolute", left: x, top: y, width: w, height: h,
      background: `rgba(${hexToRgb(color)}, 0.12)`,
      border: `1.5px solid ${color}`,
      borderRadius: 16,
      boxShadow: `0 0 24px rgba(${hexToRgb(color)}, 0.25)`,
      display: "flex", flexDirection: "column",
      alignItems: "center", justifyContent: "center",
      transform: `scale(${progress})`, opacity,
    }}>
      {icon && <span style={{ fontSize: 32, marginBottom: 8 }}>{icon}</span>}
      <span style={{ fontFamily: inter, fontSize: 20, fontWeight: 600, color: "#FFFFFF" }}>{label}</span>
      {sublabel && <span style={{ fontFamily: inter, fontSize: 13, color: "rgba(255,255,255,0.5)", marginTop: 4 }}>{sublabel}</span>}
    </div>
  );
};
```

## Animation Types

| Type | Description | Duration |
|---|---|---|
| **System Flow** | Full architecture diagram with data flow | 6–8s (180–240f) |
| **Step-by-Step** | One component at a time, builds full picture | 8–10s |
| **Comparison** | Two approaches side by side | 6s |
| **Timeline** | Sequence of events top-to-bottom | 5–6s |
| **Request/Response** | Single API call traced through the system | 5s |

## Full Prompt Output Format

When a user provides a topic, output a Remotion prompt in this structure:

```
**Topic:** [What they asked for]
**Style:** ByteMonk System Design — OnlyAI Academy Colors
**Composition:** [width]×[height], [fps]fps, [frames] frames ([duration]s)

**Architecture to animate:**
1. [Service A] (Producer — orange #FF6B35)
2. [Service B] (Broker — warm orange #F7931E)
3. [Service C] (Consumer — teal #00D4FF)
4. [Service D] (Database — blue #1E3A8A)

**Flow:** A → B → C → D (with data packets)

**Animation timeline:**
- 0–45f: Background + title bar slides in
- 45–75f: Service A appears
- 75–105f: Arrow A→B draws + Service B appears
- 105–135f: Arrow B→C draws + Service C appears
- 135–165f: Arrow C→D draws + DB appears
- 165–240f: All visible + data particles loop

**Title bar:** "How [Topic] Works" | badge: "System Design"
**Background:** #080C1A + dot grid
**Fonts:** SpaceGrotesk 700 (title) + Inter 600 (labels)
**Special effects:** [any specific glow, pulse, or particle effects for this topic]
```

## Style Modes

| Mode | Use For | Background | Accent |
|---|---|---|---|
| **Flow** *(default)* | API calls, request/response, data pipelines | `#080C1A` | Teal flow lines |
| **Architecture** | Microservices, distributed systems | `#080C1A` | Orange nodes, teal edges |
| **Timeline** | Process steps, event sequences | `#080C1A` | Orange timeline spine |
| **Comparison** | Before/after, approach A vs B | `#080C1A` | Orange left, Teal right |

## Important Remotion Notes

- Always import `useCurrentFrame`, `useVideoConfig`, `spring`, `interpolate`, `AbsoluteFill` from `"remotion"`
- Use `useCurrentFrame()` and `useVideoConfig()` inside the component — never as module-level variables
- Arrow animations use SVG `<line>` or `<path>` with `strokeDasharray`/`strokeDashoffset`
- For particle dots, drive position with `frame` directly (no state/useEffect) — pure math
- All animations must be deterministic functions of `frame` for Remotion's rendering model
- Use `extrapolateLeft: "clamp"` and `extrapolateRight: "clamp"` on all `interpolate` calls
- Refer to the `remotion-best-practices` skill for detailed API patterns
