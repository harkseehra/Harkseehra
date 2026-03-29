# Simple Flow — Android App Build Brief

## What Is It
Simple Flow is a calendar app for Android with an iOS design soul. Minimal, unhurried, elegant — defined by a swappable accent colour system with nature-inspired names. The attached HTML prototype (`simple-flow.html`) is the complete interactive reference. Build the Android app to match it exactly.

## Recommended Stack
- **Kotlin + Jetpack Compose** — native Android, best for animation fidelity and system integration (notifications, calendar provider, widgets).
- **Alternative: React Native + Expo** — spring animations map to `react-native-reanimated`, gestures to `react-native-gesture-handler`, sheets to `@gorhom/bottom-sheet`.

## Navigation Architecture
Drawer (left slide) + navigation stack. No bottom tab bar.

```
Drawer
├── Calendar (Monthly View) ← default home
├── Events List
├── Themes (Accent Colour)
└── Settings
```

Each destination is full-screen. Drawer slides from left (80% width, max 320dp) with spring animation and dimmed scrim. Hamburger icon (3 equal bars, centered, `--l1` colour) opens/closes it, morphing to × when open.

## Screens

### A. Monthly Calendar View (Home)
- **Header**: iOS large-title layout:
  - Top row (44dp): hamburger (left), today button (right — circle with current date number in accent)
  - Below: Large title (34px, 700 weight, left-aligned) — "Month Year"
- **Day labels**: S M T W T F S — 13px, secondary label
- **Calendar grid**: 7×6, each cell a perfect square. Date number centered in 40dp circle. Cells tile seamlessly (0 gap).
- **Cell states**:
  - Default: primary label, transparent bg
  - Today: accent filled circle, white number
  - Selected: accent surface circle, accent number, pop-in animation (scale 0.5→1.08→1)
  - Other month: quaternary label (very faded)
  - Has events: up to 3 dots (4.5dp) below number in accent
- **Day preview**: Below grid. Each event is a **separate pill card** (12dp radius, bg2 background) with 3dp accent left-bar, title, time. Up to 3 shown with "+N more". Staggered slide-up.
- **Month navigation**: Horizontal swipe. 50px slide travel, direction-matching title animation.
- **Bottom**: Full-width accent CTA "＋ New Event" (52dp, 14dp radius, subtle glow)

### B. Day View
- Full-screen modal, slides up from bottom (0.48s spring)
- Compact centred nav title with full date, back chevron in accent
- **Empty**: Cascading float-in — calendar icon, "No Events", "Your day is open.", CTA
- **Events exist**: Timeline — time label (58dp column) → accent bar → event card. Staggered 0.07s delays.
- "＋ Create Another Event" text button below

### C. Left Drawer
- Spring slide-in (0.42s), scrim fades
- Header: calendar icon (accent) + "Simple Flow" (24px, 700) + current date
- Menu items: 52dp rows, 10dp rounded, active = accent + accent surface bg
- Footer: "Simple Flow v1.0"

### D. Event Creation / Edit Sheet
- Bottom sheet to 92% height, spring (0.46s), drag pill handle
- Cancel / title / Save (disabled until title entered)
- iOS inset-grouped form cards (bg3, 12dp radius, hairline separators):
  - Title (auto-focus, 600 weight) + Location (pin icon)
  - Date, Starts/Ends time, All Day toggle (iOS style 51×31dp, spring knob)
  - Reminder picker (expandable inline list)
  - Notes textarea
  - Delete (edit only, red, inline confirm)
- Form groups stagger in (0.04s delay each)

### E. Success Banner
- Dark banner drops from top with bounce, accent checkmark (stroke-draw animation)
- Auto-dismiss 3.5s, tap to dismiss

### F. Events List
- Grouped: Today, Tomorrow, This Week, Next Week, This Month, Later
- Sticky headers with blur background
- Rows: accent bar + title + time + location + chevron, staggered in
- Empty state with CTA

### G. Themes Screen (Accent Colour)
- Title: "Accent Colour" + subtitle
- **4-column grid** of colour circles (44dp) with name below (11px, 600 weight)
- Active colour gets accent border on card + white checkmark centred on circle
- Tap triggers: ripple animation + card bounce + 0.45s accent transition across entire app
- Circles stagger in on load

### H. Settings Screen
- **Appearance**: 3-segment iOS picker (Light / Dark / Auto) with mini phone previews
- **About**: Version, current accent name, current appearance mode

## Accent Colour System (8 colours)

Only the accent layer changes. Neutrals stay fixed.

| ID | Name | Primary | Pressed | Surface (light) | Glow |
|---|---|---|---|---|---|
| river | River Blue | #2196D4 | #1A7BB8 | #EAF4FB | rgba(33,150,212,0.12) |
| midnight | Midnight | #2D3E94 | #232F73 | #ECEEF8 | rgba(45,62,148,0.12) |
| autumn | Autumn | #C43A2F | #A32F25 | #FBEEED | rgba(196,58,47,0.12) |
| forest | Forest | #3A7D44 | #2D6335 | #ECF5ED | rgba(58,125,68,0.12) |
| sunset | Sunset | #E8652D | #C85424 | #FDF0EB | rgba(232,101,45,0.12) |
| moonstone | Moonstone | #4C5D8A | #3D4B70 | #EDEEF3 | rgba(76,93,138,0.12) |
| rose | Rose | #D45B7A | #B64A65 | #FAEEF1 | rgba(212,91,122,0.12) |
| amber | Amber | #C4913B | #A67A30 | #F8F2E8 | rgba(196,145,59,0.12) |

In dark mode, surface tints become the accent at ~10% opacity (`accent + "1A"`).

Default accent: `river` (River Blue).

## Light Mode Neutrals
- Background: #FFFFFF
- Secondary bg: #F2F2F7
- Elevated surface (cards/forms): #FFFFFF
- Separator: #E5E5EA (0.33px)
- Primary label: #000000
- Secondary label: rgba(60,60,67, 0.6)
- Tertiary label: rgba(60,60,67, 0.3)
- Quaternary label: rgba(60,60,67, 0.18)
- Navbar: rgba(255,255,255, 0.72) + saturate(180%) blur(20px)

## Dark Mode Neutrals
- Background: #000000 (true black, OLED)
- Secondary bg: #1C1C1E
- Elevated surface: #2C2C2E
- Separator: rgba(56,56,58, 1)
- Primary label: #FFFFFF
- Secondary label: rgba(235,235,245, 0.6)
- Tertiary/Quaternary: same pattern at 0.3 / 0.18
- Navbar: rgba(0,0,0, 0.72) + blur
- Toggle off: #39393D

## Appearance Modes
- **Light**: Force light
- **Dark**: Force dark
- **Auto**: Follow system dark mode setting

## Typography
- **Font**: Inter (Google Fonts, fallback to system)
- **Large Title**: 34px / 700 / -0.7px — month headers, screen titles
- **Title 2**: 22-24px / 600-700 — section headers, empty states
- **Headline**: 17px / 600 — event titles, nav titles
- **Body**: 17px / 400 — descriptions, inputs
- **Subhead**: 15px / 500 — time labels, metadata
- **Caption**: 11-13px / 400-600 — day labels, locations, supplementary
- All text: letter-spacing -0.01em to -0.03em

## Animation Curves
- **Spring modal** (sheets, day view): `cubic-bezier(0.32, 0.72, 0, 1)` — 0.46-0.48s
- **Spring overshoot** (pop-in, press): `cubic-bezier(0.175, 0.885, 0.32, 1.1)` — 0.32s
- **Ease out** (slides, fades): `cubic-bezier(0.25, 1, 0.5, 1)` — 0.35s
- **Button press**: scale 0.82-0.96, ~0.12-0.15s
- **Date pop**: scale 0.5 → 1.08 → 1.0
- **Banner drop**: translateY(-90) scale(0.9) → bounce 70% → settle
- **Month swipe**: 50px travel + direction-matching title
- **Staggered lists**: 0.04-0.07s delay per item
- **Accent swap**: 0.45s ease-out, scoped to colour/background properties only

## Data Persistence
- Events: Local database (Room on Android, AsyncStorage on RN)
- Accent theme: localStorage key `sf-ink`, value = accent ID string. Default: `river`
- Appearance mode: localStorage key `sf-mode`, value = `light` | `dark` | `auto`. Default: `auto`

## Event Data Model
```
Event {
  id: String (unique)
  title: String (required)
  date: String
  startHour: Int (0-23)
  startMinute: Int (0, 15, 30, 45)
  endHour: Int (0-23)
  endMinute: Int (0, 15, 30, 45)
  location: String (optional)
  notes: String (optional)
  allDay: Boolean
  reminder: String ("None" | "At time of event" | "5 minutes before" | "15 minutes before" | "30 minutes before" | "1 hour before" | "1 day before")
}
```

## Design Principles
1. **iOS soul on Android** — no Material ripples, FABs, or bottom nav. Use iOS grouped lists, toggles, modal sheets, frosted glass navbars.
2. **Only the accent changes** — colour themes swap the accent system-wide but never touch typography, spacing, corners, or neutrals.
3. **Every interaction animated** — springs on opens, press-scale on taps, cascading staggers on lists, direction-aware slides.
4. **Generous spacing** — 40dp date circles, 52dp buttons, 14dp card radii, 20dp padding. Let it breathe.
5. **Hairline details** — 0.33px separators, subtle glow shadows, not hard drops.

## Reference File
The HTML prototype `simple-flow.html` is the single source of truth. Open it in a mobile browser to experience every screen, animation, and interaction.
