# Close the Deal — The Gatekeeper of the Oasis

## Overview

You are a **Traveler** seeking passage through the desert. At each oasis, you encounter **The Sphinx** — an ancient, wise guardian who holds the keys to closed deals. The Sphinx does not fight; she tests. By answering her questions with wisdom, preparation, and balance, you earn her respect and unlock the sacred scrolls and keys that grant you passage to the next oasis.

**MVP**: One desert oasis, one Sphinx, multiple-choice wisdom trials. **Future**: Multiple oases, expanded scrolls/artifacts, animations, 3D desert landscapes.

---

## The Three Pillars of Passage

The Sphinx judges the Traveler on three dimensions — the *foundations of a sustainable partnership*:

### 1. **Passage Key ⚡** (Speed of Credentialing)
*How quickly can you deliver clean, approved legal terms?*

- **What it measures**: Your ability to move confidently with proper preparation
- **Not about rushing** — it's about being credible and decisive
- **Low (0)**: "You move like a snail. Nothing is ready."
- **Mid (50)**: "You balance speed with preparation."
- **High (100)**: "You are credentialed. Your paperwork arrives clean."

**When it increases**: Choosing answers that show confidence, readiness, decisiveness
**When it decreases**: Choosing answers that show hesitation, delay, over-thinking

### 2. **Ritual of Trust 🤝** (Reputation & Reliability)
*Are you a reliable partner the Sphinx can count on?*

- **What it measures**: Your trustworthiness as a professional and partner
- **The Sphinx sees you as**: A "Reliable Partner" who won't send garbage paperwork
- **Low (0)**: "You are careless. Procurement won't trust you."
- **Mid (50)**: "You are competent. Procurement knows what to expect."
- **High (100)**: "You are trusted. Procurement requests you by name."

**When it increases**: Choosing answers that show respect, integrity, partnership mindset
**When it decreases**: Choosing answers that show dismissal, disrespect, arrogance

### 3. **Structural Integrity 🏛️** (Deal Durability)
*Does your deal hold up under scrutiny, or does it have cracks in the foundation?*

- **What it measures**: The soundness of your legal position (not just speed)
- **The Sphinx asks**: "Will this deal survive a lawyer's review? An audit? A dispute?"
- **Low (0)**: "Cracks everywhere. This deal will shatter."
- **Mid (50)**: "Adequate foundation. It will hold."
- **High (100)**: "Rock solid. Built to last."

**When it increases**: Choosing answers that protect the company legally, handle risky clauses properly
**When it decreases**: Choosing answers that skip legal safeguards, accept unlimited liability, ignore compliance

---

## Thematic Mapping

| Old Corporate Frame | Gatekeeper Theme | Actual Meaning |
|---|---|---|
| Velocity | **Passage Key ⚡** | Speed of credentialing; decisiveness backed by preparation |
| Relationship | **Ritual of Trust 🤝** | Reputation as a reliable, professional partner |
| Risk | **Structural Integrity 🏛️** | Deal durability; avoiding cracks in the foundation |

---

## Data Schema (MVP)

```javascript
// The Three Pillars (replaces old faith/meters)
pillars = {
  passageKey: 70,          // ⚡ How credible & decisive are you?
  ritualOfTrust: 70,       // 🤝 How much does Sphinx trust you?
  structuralIntegrity: 20, // 🏛️ How sound is your legal position?
};

// Sacred Scrolls (contract templates, wisdom)
scrolls = {
  "cloudforge-standard-msa": {
    id: "cloudforge-standard-msa",
    name: "Scrolls of CloudForge",
    icon: "📜",
    category: "sacred-scroll",
    description: "CloudForge's standard MSA — your foundation",
    startingScroll: true,
    unlockIf: null,
  },
  "super-cap-wisdom": {
    id: "super-cap-wisdom",
    name: "Wisdom of the Super-Cap",
    icon: "💰",
    category: "wisdom-tablet",
    description: "Liability cap negotiation: 2x–3x super-caps",
    unlockIf: (pillars) => pillars.structuralIntegrity > 40,
  },
  "auto-renewal-scroll": {
    id: "auto-renewal-scroll",
    name: "Scroll of Balanced Renewal",
    icon: "🔄",
    category: "sacred-scroll",
    description: "60-day opt-out compromise for auto-renewal",
    unlockIf: (pillars) => pillars.ritualOfTrust > 60,
  },
  "dpa-essence": {
    id: "dpa-essence",
    name: "Essence of Data Guardianship",
    icon: "🔐",
    category: "wisdom-tablet",
    description: "Data residency and DPA essentials",
    unlockIf: (pillars) => pillars.structuralIntegrity > 50,
  },
  "closure-scroll": {
    id: "closure-scroll",
    name: "Scroll of Swift Closure",
    icon: "⚡",
    category: "wisdom-tablet",
    description: "Close deals confidently without sacrificing terms",
    unlockIf: (pillars) => pillars.passageKey > 60,
  },
};

// Session state
session = {
  oasisId: "acme-oasis",
  riddleIndex: 0,
  scrollsUnlocked: ["cloudforge-standard-msa"],
  passageGranted: false,
  merit: null,  // ENLIGHTENED / WORTHY / PREPARED / NOVICE / UNWORTHY
};
```

---

## Architecture Layers

```
┌──────────────────────────────────────────────┐
│  Presentation Layer (render.js)              │
│  - Desert oasis visuals (DOM, future Canvas) │
│  - Three Pillars display (always visible)    │
│  - Sphinx visuals (future animations)        │
└──────────────────────┬───────────────────────┘
                       │
┌──────────────────────▼───────────────────────┐
│  Game Logic (game.js)                        │
│  - Riddle flow                               │
│  - Pillar deltas (passage key, trust, integrity) │
│  - Scroll unlock logic (based on pillars)    │
│  - Merit calculation (balance-aware)         │
└──────────────────────┬───────────────────────┘
                       │
┌──────────────────────▼───────────────────────┐
│  Data Layer (data/)                          │
│  - scenarios.js (your riddles)               │
│  - oases.js (world definition)               │
│  - sphinx.js (guardian definition)           │
│  - scrolls.js (sacred texts)                 │
│  - pillars.js (three pillar rules)           │
└──────────────────────────────────────────────┘
```

---

## User Flow (MVP)

```
[Traveler's Path]
  ↓
[Home: Choose an Oasis]
  "Acme Corp Oasis" / "NovaBuild Oasis"
  ↓
[Desert Oasis Loads]
  - Minimal desert background
  - The Sphinx (SVG silhouette)
  - The Three Pillars (always visible, top-right)
    ⚡ Passage Key: 70    [████░░]
    🤝 Ritual of Trust: 70 [████░░]
    🏛️ Structural Integrity: 20 [██░░░░]
  ↓
[Traveler approaches]
  ↓
[Sphinx poses riddle]
  "Greetings, Traveler. Here is my first test..."
  [Question from scenario]
  ↓
[Traveler chooses answer]
  - Answer A
  - Answer B
  - Answer C
  ↓
[Sphinx judges]
  "Ah, you chose [answer]. Here is why..."
  [Coach text]
  
  ✨ Unlocked: Sacred Scroll
  📊 Pillars shift:
     ⚡ Passage Key: +8
     🤝 Ritual of Trust: -15
     🏛️ Structural Integrity: +5
  ↓
[Next riddle or passage granted]
  ↓
[Result: Merit & Scrolls]
  Merit: WORTHY
  Scrolls: 4/6 unlocked
  Three Pillars (final)
  [Replay] [Back to Oases]
```

---

## Merit Levels (Three Pillar Judgment)

Merit is calculated based on **balance and overall performance**:

```javascript
MERIT = {
  ENLIGHTENED: {
    title: "Enlightened",
    description: "The Sphinx bows slightly. 'Swift, trustworthy, and sound. You walk the way of masters.'",
    requirement: "All three pillars high (65+), excellent balance"
  },
  WORTHY: {
    title: "Worthy",
    description: "The Sphinx nods. 'You have proven your judgment. You may pass.'",
    requirement: "At least two pillars solid (60+), no major weakness"
  },
  PREPARED: {
    title: "Prepared",
    description: "The Sphinx is satisfied. 'You will learn. Return when you understand more.'",
    requirement: "Mixed performance; some strengths, some gaps"
  },
  NOVICE: {
    title: "Novice",
    description: "The Sphinx is patient. 'You have much to learn. Return when you are ready.'",
    requirement: "One pillar decent, others weak"
  },
  UNWORTHY: {
    title: "Unworthy",
    description: "The Sphinx does not step aside. 'Study and return when you are ready.'",
    requirement: "Major failure across pillars"
  }
};
```

**The Sphinx's specific feedback**:
- "You are fast but reckless" → High Passage Key, low Structural Integrity
- "You are careful but too slow" → High Structural Integrity, low Passage Key
- "You move people well but skip legal steps" → High Ritual of Trust, low Structural Integrity

---

## Sacred Scrolls by Pillar

Scrolls unlock based on which pillar you demonstrate:

| Scroll | Requires | Why |
|--------|----------|-----|
| Scrolls of CloudForge | Starting tool | Foundation |
| Wisdom of the Super-Cap | Structural Integrity > 40 | Legal rigor required |
| Scroll of Balanced Renewal | Ritual of Trust > 60 | Relationship skills required |
| Essence of Data Guardianship | Structural Integrity > 50 | Compliance rigor required |
| Scroll of Swift Closure | Passage Key > 60 | Decisiveness & credibility required |

---

## File Structure (After Phase 1)

```
index.html
├── js/
│   ├── game.js         (riddle flow, three pillars, merit)
│   ├── render.js       (oasis rendering; swappable)
│   ├── data.js         (oases, sphinx, scrolls, pillars)
│   └── scenarios.js    (YOUR EXISTING SCENARIOS)
├── assets/
│   ├── oasis-desert.svg
│   ├── sphinx-silhouette.svg
│   └── ...
├── DESIGN.md
└── THEME.md (optional: lore, pillar meanings)
```

---

## Phase Breakdown

| Phase | Deliverable | What's New |
|-------|-------------|-----------|
| **1** | Riddle dialog UI | Sphinx asks, three pillars always visible top-right |
| **2** | Oasis + Sphinx | Desert background + Sphinx silhouette |
| **3** | Click-to-approach | Traveler engages Sphinx |
| **4** | Pillars + scrolls | Three pillars drive scroll unlocks |
| **5** | Polish | Sphinx feedback is specific to pillar imbalances |
| **6+** | Extensions | Multiple oases, animations, 3D |

---

## Token Strategy

- **Phase 1** (Sonnet 4.6): ~1000 tokens
- **Phase 2** (Sonnet 4.6): ~1200 tokens
- **Phase 3** (Haiku 4.5): ~800 tokens
- **Phase 4** (Sonnet 4.6): ~900 tokens
- **Phase 5** (Haiku 4.5): ~400 tokens
- **Total MVP: ~4.3K tokens**

---

## Design Principles

1. **Three Pillars, not metrics** — Each pillar has real meaning in partnerships
2. **Balance matters** — Sphinx respects well-rounded judgment
3. **Professional & mythic** — Legal training in timeless story
4. **Minimal aesthetics** — Desert, Sphinx, archway; no clutter
5. **Data-driven, expandable** — Render layer swaps for future 3D
6. **MVP mindset** — Multiple choice now; fancy animations later

---

---

## Phase 3D: Cinematic Character Dialogue (Blizzard-Tier Animation)

### Vision
Transform the static scene into a **living cinematic experience** where the Sphinx and Traveler (AE) are full 2D characters who **speak to each other** via parchment speech bubbles positioned at their mouths.

### Implementation
**All changes bundled in index.html** (CSS + SVG + JavaScript)

#### Characters
- **The Sphinx** (260×340px): Full illustrated form with face, eyes (animated glow), ceremonial beard, nemes headdress
  - Eyes animate with inner fire (glow pulse + blink cycle)
  - Body breathes with subtle float animation
  - On card hover: eyes intensify, breath quickens
  
- **The Traveler** (180×270px): AE character with lantern
  - Lantern flame flickers (multi-axis wobble + opacity variation)
  - Lantern glass pulses with glow aura
  - On card hover: lantern intensifies (brighter glow, faster flicker)
  - Body idle bob (2-3px vertical drift every 4.8s)

#### Speech Bubbles (Parchment Style)
- **Sphinx Speech**: Positioned top-right at mouth level (340×90px)
  - Background: Parchment gradient (#fffef5 → #f8f4e8)
  - Border: Leather brown (#8a6a48)
  - Pointer tail: 14px drop shadow, positioned below
  - Header: "🔮 The Sphinx poses:" (gold color)
  - Body: Current riddle text from SCENARIOS
  
- **Traveler Choices**: Positioned left side, staggered vertically
  - Choice label: "💭 Your move:"
  - Three choice bubbles (one per answer option)
  - Same parchment styling, slightly smaller
  - Hover: brightens + lifts (-2px Y)

#### Animation Sequence
**Upon riddle display:**
1. **0.0s**: Scene idle (dust particles float continuously)
2. **0.0s**: Sphinx speech bubble slides in from top-left + bounces (0.7s easeOutBack)
3. **0.1s**: Speech body text fades in (0.9s ease-out)
4. **0.35s**: Choice label slides up + fades in (0.6s ease-out, 0.35s delay)
5. **0.4s–0.7s**: Choice bubbles stagger-bounce in (easeOutElastic):
   - Choice 0: 0.4s delay + 0.6s duration
   - Choice 1: 0.55s delay + 0.6s duration
   - Choice 2: 0.7s delay + 0.6s duration

**Upon choice selection:**
6. **0.0s**: Particle burst effect (dust particles accelerate upward for 0.8s)
7. **0.0s**: Selected choice bubble glows gold border + stays full opacity
8. **0.0s**: Unselected choices fade to 0.55 opacity + disabled
9. **0.5s+**: Coach feedback renders (Sphinx judges the choice)
10. **1.0s+**: "Next riddle" button appears

#### Depth Layering (Z-Index)
```
z-index 1:  Sky aurora + moonlight shaft (ethereal glow)
z-index 2:  Distant mountains + pyramids (mirage shimmer)
z-index 3:  Dust particle system (foreground float)
z-index 4:  Palm trees + archway (mid-ground)
z-index 5:  Sand dune (foreground)
z-index 6:  Sphinx & Traveler characters (main focus)
z-index 6:  Choice bubbles (interactable overlays)
z-index 7:  Sphinx speech bubble (topmost speech)
```

#### CSS Animations Added
```css
@keyframes sphinxSpeak          /* 0.7s easeOutBack */
@keyframes textReveal           /* 0.9s ease-out */
@keyframes labelReveal          /* 0.6s ease-out, 0.35s delay */
@keyframes choiceReveal         /* 0.6s easeOutElastic, staggered 0.4/0.55/0.7s */
@keyframes dustBurst            /* 0.8s choice feedback, accelerate upward */
@keyframes sphinxCharBreathe    /* idle 5s float */
@keyframes eyeGlow              /* 4.2s inner fire pulse */
@keyframes eyeBlink             /* 6.5s + on-hover intensify to 3.8s */
@keyframes flameFlicker         /* 0.55s lantern wobble */
@keyframes lanternPulse         /* 2.4s glow aura */
```

#### JavaScript Wiring
- `updateCinematicScene()`: Populates speech bubbles from current SCENARIOS step
- `pickChoice(i)`: Triggers dust burst, disables unselected choices, highlights selection
- Animations synchronized with game state (hidden during intro, active on first riddle)
- Accessibility: `prefers-reduced-motion` disables all animations

#### Mobile Responsive
- Scene height: 260px (down from 400px)
- Characters scale: Sphinx 170×220, Traveler 115×175
- Speech bubble widths: 240px (down from 340px)
- Choice bubbles: 220px container
- Animations remain same (mobile-first easing curves)

### Quality Standards (Blizzard-Tier)
✓ **Character presence**: Full SVG-rendered faces with gradient lighting and shadow depth  
✓ **Cinematic timing**: Staggered reveals create rhythm (Sphinx → label → choices)  
✓ **Visual weight**: Multiple overlapping animations (character breathe + eyes glow + particles + speech)  
✓ **Easing mastery**: Mix of linear, ease-in-out, cubic-bezier, easeOutBack, easeOutElastic  
✓ **Accessibility**: All animations respect `prefers-reduced-motion` and have fallbacks  
✓ **Performance**: GPU-accelerated transforms (translate, scale), optimized filter effects  

---

## Next Steps

✅ **Phase 3D: Cinematic Character Dialogue Complete**
→ **Phase 3E**: Visual juice (particle bursts ✓, pillar bar fill animations)
→ **Phase 3G**: Epic verdict screen (Sphinx's final judgment cinematics)
→ **Phase 3H**: Audio layer (future: Sphinx speaks, scroll unlocks, choice feedback)
