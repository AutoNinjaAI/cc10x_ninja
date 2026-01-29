---
name: design-system
description: "Internal skill. Use cc10x-router for all development tasks."
allowed-tools: Read, Grep, Glob
---

# Design System — AutoNinja Aesthetic

## Design Philosophy
- Cyberpunk meets aerospace lab
- Glass surfaces floating over dark voids
- Neon accents that glow, not just color
- Dark mode is the primary canvas; light mode is the clean inverse
- Every surface has depth (blur, shadow, noise texture, spotlight)
- Typography creates hierarchy through weight and tracking, not just size

## Color Tokens

### Neon Accent Palette
- neon-green: #00ffa3 (primary actions, success, active states)
- neon-purple: #bd00ff (AI/intelligence, secondary accent, charts)
- neon-blue: #00f0ff (info, links, system health, tertiary)

### Surface Colors
- Dark canvas: #030014 (body background)
- Dark card: rgba(255,255,255,0.03) with backdrop-blur(16px)
- Light canvas: #f1f5f9 (slate-100)
- Light card: rgba(255,255,255,0.7) with backdrop-blur(20px)

### Text Hierarchy
- Primary: slate-900 / white
- Secondary: slate-500 / gray-400
- Tertiary: slate-400 / gray-500
- Mono/label: 10px uppercase tracking-[0.2em] font-mono

## Typography
- Body: Inter (300-700 weights)
- Mono/Code/Labels: JetBrains Mono (400, 700)
- Headings: font-black, tracking-tight
- Labels: text-[10px] uppercase tracking-wider font-mono font-bold
- Stats: text-4xl or text-5xl font-bold

## Glass Panel Pattern (CORE)

Every card/container uses this:
- Background: translucent white (3% dark / 70% light)
- Backdrop blur: 16-20px
- Border: 1px white at 5% opacity (dark) / 50% (light)
- rounded-2xl
- Noise texture overlay at 3% opacity (dark) / 20% (light)
- Spotlight effect: radial gradient follows cursor on hover

## Glow & Shadow System
- Neon glow: shadow-[0_0_15px_rgba(color,0.3)] on buttons/badges
- Inner glow on status pills: shadow-[0_0_10px_inset_currentColor]
- Card hover: shadow-xl + scale-[1.01]
- Dark mode: shadows use neon colors; light mode: use standard opacity shadows

## Animation Patterns
- Page entry: fadeIn 0.5-0.8s ease-out (translateY 10px -> 0)
- Modal: scale-95 + translateY-8 + rotate-1 -> scale-100 + translateY-0
- Hover transitions: duration-300 for colors, duration-500 for transforms
- Pulse: status indicators, live badges
- Shimmer: gradient line animation on modal headers (bg-[length:200%_100%])
- Spin slow: orbital decorative elements (15-20s)
- Custom cubic-bezier for modal spring: cubic-bezier(0.34, 1.56, 0.64, 1)

## Interactive States (EVERY element)
- Default -> Hover -> Active -> Focus -> Disabled -> Loading
- Hover: color shift + subtle translate or scale
- Active: translate-y-0 (press down)
- Focus: ring-2 with accent color at 20% opacity
- Disabled: opacity-50 cursor-not-allowed
- Loading: spinner + text change ("Processing...")

## Component Anatomy Patterns

### Buttons
- Primary: gradient bg (neon accent), text-white/black, rounded-xl, hover:shadow-lg + hover:-translate-y-0.5
- Secondary: bg-white/5, border, text-muted, hover:bg-white/10
- Danger: hover:text-red-500 + hover:bg-red-50
- All buttons: flex items-center gap-2, include icon where appropriate

### Glass Cards (GlassCard)
- Props: children, className, hoverEffect, glowColor (green|purple|blue|none), noPadding
- Spotlight effect: radial gradient follows mouse position
- Noise texture overlay
- Border highlight in dark mode (border-white/5)

### Modals
- Gradient accent line at top (emerald -> purple -> blue, animated shimmer)
- Window control dots (minimize, maximize, close) in header
- Header: icon in rounded-xl container with glow + title + monospace subtitle
- Body: p-6, max-h-[70vh] overflow-y-auto
- Backdrop: blur-sm + bg-black/80 (dark)
- Entry animation: spring scale + translate + slight rotation

### Tables
- Header: bg-slate-50/white-5%, text-[10px] uppercase tracking-wider font-mono
- Rows: hover:bg-white/5, group for revealing action buttons
- Actions: opacity-0 -> opacity-100 on row hover, translateX animation
- Status pills: colored bg + border + rounded-full + text-[10px]
- Health bars: h-1.5 rounded-full with glow shadow
- Pagination: numbered buttons with active glow state
- Decorative corner lines on table container (border-emerald-500/50)

### Forms
- Labels: text-[10px] font-bold uppercase tracking-wider text-slate-400 mb-1.5
- Inputs: bg-white/bg-black-40%, border rounded-xl, focus:ring-2 focus:border-accent
- Group hover: border color shift on parent hover
- Recipient fields: avatar + name + email in row
- Textareas: resize-none, leading-relaxed

### Dropdowns
- Custom animated (not native select)
- Trigger: button with icon + chevron that rotates 180deg on open
- Menu: absolute, rounded-xl, shadow-2xl, animated origin-top scale
- Items: hover glow sweep effect (translate-x shimmer)
- Selected: checkmark + accent background
- Footer: "Create New" action at bottom with border-t

### Sidebar Navigation
- Fixed left, w-64, glass background
- Logo section with glow behind icon + animated pulse
- Menu items: rounded-xl py-3.5, active: left border accent + bg-white/5 + shadow glow
- Sub-items: collapsible with max-h animation, dot indicators
- Footer: theme toggle (custom switch), settings, disconnect

### Stat Cards
- Label: mono uppercase tracking-widest
- Value: text-4xl font-bold
- Change badge: small bg pill with +/- percentage
- Optional: progress bar, bar chart visualization, icon in rounded-full

### Diagnostics/Alerts
- Left colored border (1px) indicating severity
- Icon in rounded-full colored bg
- Action button: "Fix" aligned right
- Success variant: emerald bg with checkmark

## Dark/Light Mode Rules
- ALWAYS provide both: dark: prefix classes
- Dark = primary canvas, neon accents glow
- Light = clean slate bg, emerald/blue accents are solid
- Borders: slate-200 (light) / white/5-10% (dark)
- Text: slate-900 (light) / white (dark)
- Cards: solid white translucent (light) / dark translucent (dark)

## Anti-Patterns (DO NOT)
- No flat, lifeless cards without blur/texture
- No native HTML selects — build custom dropdowns
- No buttons without hover + active states
- No modals without entry animation
- No tables without row hover actions
- No forms without focus ring styling
- No status indicators without glow/pulse
- No color without its dark/light variant
- No layout without responsive breakpoints (grid-cols-1 -> lg:grid-cols-3)
