# Meeting Cost Calculator - Design Specification

> **Design Philosophy:** Every second should feel expensive. The interface should create visceral anxiety about wasted time and money. Think "your wallet is on fire" energy.

---

## 1. Color Palette

### Primary Colors

| Name | Hex | Usage |
|------|-----|-------|
| **Void Black** | `#0D0D0D` | Main background - deep, endless void where money disappears |
| **Ember Red** | `#FF4136` | Primary accent - danger, urgency, STOP |
| **Burn Orange** | `#FF6B35` | Secondary accent - fire, heat, destruction |
| **Warning Yellow** | `#FFD700` | Highlights - caution, attention |

### Text Colors

| Name | Hex | Usage |
|------|-----|-------|
| **Ghost White** | `#F8F8F8` | Primary text |
| **Smoke Gray** | `#A0A0A0` | Secondary text, labels |
| **Ash Gray** | `#666666` | Disabled states, hints |

### Surface Colors

| Name | Hex | Usage |
|------|-----|-------|
| **Charcoal** | `#1A1A1A` | Card backgrounds |
| **Coal** | `#252525` | Elevated surfaces, inputs |
| **Ember Glow** | `rgba(255, 65, 54, 0.15)` | Danger state backgrounds |

### Gradient - "Money Burning"

```css
background: linear-gradient(135deg, #FF4136 0%, #FF6B35 50%, #FFD700 100%);
```

Used for the main cost display to create a "fire" effect.

---

## 2. Typography

### Google Fonts

```html
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
```

### Type Scale

| Element | Font | Size | Weight | Notes |
|---------|------|------|--------|-------|
| **Hero Cost** | JetBrains Mono | 72px / 96px | 700 | The BIG number - must dominate |
| **Headline** | Space Grotesk | 32px | 700 | Page title |
| **Subhead** | Space Grotesk | 20px | 500 | Section titles |
| **Body** | Space Grotesk | 16px | 400 | General text |
| **Label** | Space Grotesk | 14px | 500 | Form labels, small text |
| **Mono Data** | JetBrains Mono | 18px | 400 | All numbers, costs, times |

### Why These Fonts?

- **Space Grotesk:** Modern, technical, slightly unsettling geometric shapes
- **JetBrains Mono:** Numbers look expensive and precise. Every digit matters.

---

## 3. Layout

### Page Structure (Single Page)

```
┌─────────────────────────────────────────────────────────┐
│                        HEADER                           │
│    🔥 "This Meeting Is Costing You..." 🔥               │
├─────────────────────────────────────────────────────────┤
│                                                         │
│              ████████████████████████████               │
│              █                          █               │
│              █     $4,287.53           █               │
│              █     ▲ $12.34/sec        █               │
│              █                          █               │
│              ████████████████████████████               │
│                    HERO COST DISPLAY                    │
│                                                         │
│                 ⏱️ 00:42:17 elapsed                     │
│            [ ▶ START ]  [ ⏸ PAUSE ]  [ ↺ RESET ]       │
│                                                         │
├─────────────────────────────────────────────────────────┤
│                   ATTENDEE SECTION                      │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────┐    │
│  │ 👤 Sarah │ │ 👤 Mike  │ │ 👤 CEO   │ │  + Add │    │
│  │ $150/hr  │ │ $120/hr  │ │ $500/hr  │ │        │    │
│  │    ✕     │ │    ✕     │ │    ✕     │ └────────┘    │
│  └──────────┘ └──────────┘ └──────────┘                │
├─────────────────────────────────────────────────────────┤
│                 COMPARISON CALLOUTS                     │
│  ┌─────────────────┐  ┌─────────────────┐              │
│  │ 🍕 = 214 Pizzas │  │ 💵 = 2 mo rent  │              │
│  └─────────────────┘  └─────────────────┘              │
│  ┌─────────────────┐  ┌─────────────────┐              │
│  │ ✈️ = 3 flights  │  │ 💻 = 2 MacBooks │              │
│  └─────────────────┘  └─────────────────┘              │
├─────────────────────────────────────────────────────────┤
│                     SHARE SECTION                       │
│         [ 📋 Copy Results ]  [ 🐦 Tweet This ]         │
│                                                         │
│         "Share the damage with your team"               │
└─────────────────────────────────────────────────────────┘
```

### Section Spacing

- **Section gap:** 48px
- **Card padding:** 24px
- **Element gap within sections:** 16px

---

## 4. Counter Animation - "The Money Burn"

### Core Behavior

The cost display should feel **alive and accelerating**. Money doesn't just increase—it *escapes*.

### Animation Specs

```css
/* Base ticking animation */
.cost-display {
  /* Slight pulse on each tick */
  animation: cost-pulse 0.1s ease-out;
}

@keyframes cost-pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.02); }
  100% { transform: scale(1); }
}

/* Glow intensifies as cost rises */
.cost-display {
  text-shadow: 
    0 0 20px rgba(255, 65, 54, 0.5),
    0 0 40px rgba(255, 107, 53, 0.3),
    0 0 60px rgba(255, 215, 0, 0.2);
}
```

### Tick Behavior

| Cost Range | Update Frequency | Visual Effect |
|------------|------------------|---------------|
| $0 - $100 | Every 1 second | Calm pulse |
| $100 - $1000 | Every 500ms | Faster pulse |
| $1000 - $5000 | Every 250ms | Add subtle shake |
| $5000+ | Every 100ms | INTENSE - glow brightens, slight screen shake |

### Number Rolling Effect

Digits should **roll up** like a slot machine, not jump:

```
$1,234.56
    ↓ (0.1 sec later)
$1,234.67  ← The cents roll smoothly
```

Use CSS counter or JS library like `odometer.js` for smooth digit transitions.

### Per-Second Rate Display

Show the burn rate prominently:

```
▲ $12.34/sec
```

- Updates dynamically based on attendees
- Pulses RED when new attendee added
- Arrow animates upward continuously

---

## 5. Component Specifications

### 5.1 Attendee Cards

```
┌─────────────────────────┐
│  👤  Sarah              │  ← Name (editable)
│  ─────────────────────  │
│  $ [  150  ] /hr        │  ← Hourly rate input
│                         │
│         [ ✕ Remove ]    │  ← Danger action
└─────────────────────────┘
```

**Specs:**
- Width: 180px (desktop), full-width (mobile)
- Background: `#1A1A1A`
- Border: 1px solid `#333`
- Border-radius: 12px
- Hover: Border glows `#FF4136`
- Remove button: Only visible on hover

**Add Card:**
```
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
│                         │
│         +  Add          │  ← Dashed border, ghost state
│       Attendee          │
│                         │
└ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┘
```

### 5.2 Timer Controls

```
┌─────────────────────────────────────────────┐
│                                             │
│     ⏱️  00:42:17                            │  ← Big mono font
│                                             │
│   ┌─────────┐ ┌─────────┐ ┌─────────┐      │
│   │ ▶ START │ │ ⏸ PAUSE │ │ ↺ RESET │      │
│   └─────────┘ └─────────┘ └─────────┘      │
│                                             │
└─────────────────────────────────────────────┘
```

**Button States:**

| Button | Default | Active | Hover |
|--------|---------|--------|-------|
| START | Green outline `#2ECC40` | Solid green | Glow |
| PAUSE | Yellow outline `#FFD700` | Solid yellow | Glow |
| RESET | Red outline `#FF4136` | Solid red | Glow + confirm prompt |

**Button Size:** 120px × 48px, border-radius 8px

### 5.3 Cost Display (Hero Element)

```
┌─────────────────────────────────────────────┐
│                                             │
│              $12,847.53                     │
│                                             │
│           ▲ $47.23 per second               │
│                                             │
│      ════════════════════════════           │  ← Progress bar showing
│      █████████████░░░░░░░░░░░░░░           │     % of avg salary burned
│      ════════════════════════════           │
│                                             │
└─────────────────────────────────────────────┘
```

**Specs:**
- Container: 600px max-width, centered
- Background: Gradient border (fire colors)
- Cost font: 72px → 96px on desktop
- Per-second: 24px, animated arrow
- Internal padding: 48px

### 5.4 Comparison Badges

```
┌──────────────────────────┐
│  🍕  = 214 Pizzas        │
│       ($20 each)         │
└──────────────────────────┘
```

**Specs:**
- Background: `#1A1A1A`
- Border-left: 4px solid `#FF6B35`
- Padding: 16px 24px
- Grid: 2×2 on desktop, 1 column on mobile
- Animate in as cost hits thresholds

**Comparison Items:**
| Emoji | Item | Unit Cost | Anxiety Level |
|-------|------|-----------|---------------|
| 🍕 | Pizzas | $20 | Low |
| ☕ | Fancy Coffees | $7 | Low |
| 💵 | Monthly Rent | $2000 | Medium |
| ✈️ | Flights to NYC | $400 | Medium |
| 💻 | MacBook Pros | $2500 | High |
| 🚗 | Used Cars | $15000 | Extreme |
| 🏠 | Median Homes | $400000 | CEO meetings only |

---

## 6. Mobile Responsive Breakpoints

### Breakpoints

| Name | Width | Key Changes |
|------|-------|-------------|
| Mobile | < 480px | Single column, smaller fonts |
| Tablet | 481px - 768px | 2-column attendees |
| Desktop | > 768px | Full layout |

### Mobile Specific Changes

```css
@media (max-width: 480px) {
  /* Hero cost shrinks but stays dominant */
  .cost-display { font-size: 48px; }
  
  /* Attendee cards stack vertically */
  .attendee-grid { 
    grid-template-columns: 1fr;
  }
  
  /* Timer controls become icon-only */
  .timer-btn span { display: none; }
  
  /* Comparisons: 1 column */
  .comparison-grid {
    grid-template-columns: 1fr;
  }
  
  /* Share buttons full-width */
  .share-btn { width: 100%; }
}
```

### Touch Considerations

- Minimum tap target: 48px × 48px
- Swipe to delete attendee cards
- Pull-down to reset (with confirmation)

---

## 7. ASCII Wireframe - Full Page

```
╔═══════════════════════════════════════════════════════════════╗
║  🔥 THIS MEETING IS COSTING YOU...                     [?]    ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║                                                               ║
║         ██████████████████████████████████████████           ║
║         ██                                      ██           ║
║         ██         $  1 2 , 4 8 7 . 5 3        ██           ║
║         ██                                      ██           ║
║         ██           ▲ $47.23/sec              ██           ║
║         ██                                      ██           ║
║         ██████████████████████████████████████████           ║
║                        │                                      ║
║                        │                                      ║
║              ⏱️  0 0 : 4 2 : 1 7                              ║
║                                                               ║
║      ┌──────────┐  ┌──────────┐  ┌──────────┐                ║
║      │ ▶ START  │  │ ⏸ PAUSE  │  │ ↺ RESET  │                ║
║      └──────────┘  └──────────┘  └──────────┘                ║
║                                                               ║
╠═══════════════════════════════════════════════════════════════╣
║  WHO'S IN THIS MEETING?                                       ║
║                                                               ║
║  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────┐ ║
║  │ 👤 Sarah    │ │ 👤 Mike     │ │ 👤 CEO      │ │         │ ║
║  │             │ │             │ │             │ │   + Add │ ║
║  │ $150/hr    │ │ $120/hr    │ │ $500/hr    │ │         │ ║
║  │    [✕]     │ │    [✕]     │ │    [✕]     │ └─────────┘ ║
║  └─────────────┘ └─────────────┘ └─────────────┘             ║
║                                                               ║
╠═══════════════════════════════════════════════════════════════╣
║  💸 THIS MEETING COULD HAVE BOUGHT...                         ║
║                                                               ║
║  ┌─────────────────────┐    ┌─────────────────────┐          ║
║  │  🍕 624 Pizzas      │    │  ✈️ 31 Flights      │          ║
║  │     ($20 each)      │    │     (NYC roundtrip) │          ║
║  └─────────────────────┘    └─────────────────────┘          ║
║                                                               ║
║  ┌─────────────────────┐    ┌─────────────────────┐          ║
║  │  💻 5 MacBook Pros  │    │  🚗 0.8 Used Cars   │          ║
║  │     ($2500 each)    │    │     ($15000 each)   │          ║
║  └─────────────────────┘    └─────────────────────┘          ║
║                                                               ║
╠═══════════════════════════════════════════════════════════════╣
║                     SHARE THE DAMAGE                          ║
║                                                               ║
║       ┌───────────────────┐  ┌───────────────────┐           ║
║       │  📋 Copy Results  │  │  🐦 Tweet This    │           ║
║       └───────────────────┘  └───────────────────┘           ║
║                                                               ║
║            "Make your team feel the burn too"                 ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

---

## 8. Micro-Interactions & Polish

### Sound Design (Optional)

- **Tick sound:** Subtle cash register "cha-ching" every $100
- **Milestone sound:** Alarm bell at $1000, $5000, $10000
- **Start sound:** Ominous clock ticking begins

### Easter Eggs

- At $10,000+: Background subtly pulses red
- At $50,000+: Text reads "EMERGENCY MEETING DETECTED"
- If CEO is added: All costs turn GOLD

### Loading State

```
     💀
  Loading...
"Calculating the damage"
```

### Empty State (No Attendees)

```
     👻
  No attendees yet
"Add someone to start burning money"
      [+ Add First Attendee]
```

---

## 9. Accessibility

- High contrast ratios (7:1 minimum for cost display)
- Keyboard navigable (Tab through all controls)
- Screen reader: "Meeting cost is twelve thousand four hundred eighty seven dollars and fifty three cents, increasing at forty seven dollars per second"
- Reduced motion preference: Disable shake, reduce pulse intensity
- Focus states: Visible 3px orange outline

---

## 10. Implementation Notes

### Recommended Stack

- **Framework:** React or Svelte (reactive updates are key)
- **Animation:** Framer Motion or GSAP
- **Numbers:** `react-countup` or custom odometer
- **Styling:** Tailwind CSS or CSS Modules

### Performance

- Request animation frame for smooth counter
- Debounce attendee rate changes
- CSS `will-change` on animated elements

---

*"Time is money. This tool shows you exactly how much."* 🔥💸
