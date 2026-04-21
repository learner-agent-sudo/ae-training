# Close the Deal — The Gatekeeper of the Oasis

## Overview

You are a **Traveler** seeking passage through the desert. At each oasis, you encounter **The Sphinx** — an ancient, wise guardian who holds the keys to closed deals. The Sphinx does not fight; she tests. By answering her questions with wisdom, preparation, and balance, you earn her respect and unlock the sacred scrolls and keys that grant you passage to the next oasis.

**MVP**: One desert oasis, one Sphinx, multiple-choice wisdom trials. **Future**: Multiple oases, expanded scrolls/artifacts, animations, 3D desert landscapes.

---

## MVP Scope (Phase 1–5)

### What we build:
- **1 Oasis** (minimal desert background: sand, palm trees, water, archway)
- **1 Guardian**: The Sphinx (wise, ancient, unimpressed, tests your worth)
- **Visual layer**: Simple 2D (CSS/SVG silhouette Sphinx, minimal oasis, no complex animation yet)
- **Interaction**: Approach Sphinx → she poses a riddle/question → you choose your answer → she responds with wisdom → you unlock sacred scrolls
- **Sacred Scrolls**: Contract templates, negotiation wisdom, clause playbooks (earned by wise choices)
- **Respect System**: The Sphinx's respect meter (trust/caution/speed) — always visible, evolves with your answers
- **Passage Keys**: Unlock deeper understanding (treasures) that prepare you for the next oasis

### What we defer (Phase 6+):
- Multiple oases (different deal scenarios = different locations)
- Complex UI animations / branching dialogue trees
- 3D desert rendering (Three.js)
- Voice/audio dialogue from the Sphinx
- Inventory/quest log

---

## Thematic Mapping

| Corporate Frame | Gatekeeper Frame |
|---|---|
| AE | Traveler |
| Customer/Procurement | The Sphinx (recurring guardian) |
| Office | Desert oasis |
| Close deal | Earn passage through archway; unlock the oasis |
| Contract template | Sacred Scroll / Papyrus |
| Legal tip | Wisdom Tablet |
| Grade/Score | Merit level: Unworthy / Novice / Prepared / Worthy / Enlightened |
| Meter: "Relationship" | Sphinx's Trust |
| Meter: "Risk Aversion" | Sphinx's Respect for caution |
| Meter: "Velocity" | Sphinx's appreciation for speed |

---

## Data Schema (MVP)

```javascript
// 1. World state (future: array of oases)
world = {
  currentOasis: "acme-oasis",
  oases: {
    "acme-oasis": {
      id: "acme-oasis",
      name: "Acme Corp Oasis",
      guardianId: "the-sphinx",
      backgroundImage: "assets/oasis-desert.svg",
      // Palm trees, water, archway, sand dunes
    },
    "novabuild-oasis": {
      id: "novabuild-oasis",
      name: "NovaBuild Oasis",
      guardianId: "the-sphinx",
      backgroundImage: "assets/oasis-desert.svg",
      // Same Sphinx, same oasis aesthetic, different riddles
    }
  }
};

// 2. Guardian (singular, recurring character)
guardians = {
  "the-sphinx": {
    id: "the-sphinx",
    name: "The Sphinx",
    title: "Keeper of the Oases, Tester of Travelers",
    sprite: "assets/sphinx-silhouette.svg",
    position: { x: 400, y: 300 },  // Fixed for MVP
    currentDialogue: null,
  }
};

// 3. Respect System (replaces generic faith)
respect = {
  trust: 70,           // 0–100, does Sphinx trust your judgment?
  caution: 20,         // 0–100, does Sphinx respect your carefulness?
  speed: 60,           // 0–100, does Sphinx appreciate your decisiveness?
};

// 4. Sacred Scrolls (contract templates, wisdom)
scrolls = {
  "cloudforge-standard-msa": {
    id: "cloudforge-standard-msa",
    name: "Scrolls of CloudForge",
    icon: "📜",
    category: "sacred-scroll",
    description: "CloudForge's standard MSA — your foundation",
    startingScroll: true,
  },
  "super-cap-wisdom": {
    id: "super-cap-wisdom",
    name: "Wisdom of the Super-Cap",
    icon: "💰",
    category: "wisdom-tablet",
    description: "Liability cap negotiation: 2x–3x super-caps",
    unlockIf: (respect) => respect.caution > 40,
  },
  // ... more scrolls
};

// 5. Scenarios (EXISTING, reframed as riddles)
// SCENARIOS array stays the same; each step = a riddle the Sphinx poses

// 6. Session state
session = {
  oasisId: "acme-oasis",
  riddleIndex: 0,
  scrollsUnlocked: ["cloudforge-standard-msa"],
  passageGranted: false,
  scoreComposite: 0,
};
```

---

## Architecture Layers

```
┌──────────────────────────────────────────────┐
│  Presentation Layer (render.js)              │
│  - Desert oasis visuals (DOM, future Canvas) │
│  - Sphinx visuals (future animations)        │
│  - Respect meter display                     │
└──────────────────────┬───────────────────────┘
                       │
┌──────────────────────▼───────────────────────┐
│  Game Logic (game.js)                        │
│  - Riddle flow                               │
│  - Respect deltas                            │
│  - Scroll unlock logic                       │
│  - Merit calculation                         │
└──────────────────────┬───────────────────────┘
                       │
┌──────────────────────▼───────────────────────┐
│  Data Layer (data/)                          │
│  - scenarios.js (your riddles)               │
│  - oases.js (world definition)               │
│  - sphinx.js (guardian)                      │
│  - scrolls.js (sacred texts)                 │
│  - respect.js (alignment rules)              │
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
  - Respect meter (always visible, top-right)
    Sphinx's Trust: 70
    Caution Respect: 20
    Speed Respect: 60
  ↓
[Traveler approaches Sphinx]
  ↓
[Sphinx poses riddle]
  "Greetings, Traveler. Here is my first test..."
  [Question from scenario.steps[0]]
  ↓
[Traveler chooses answer]
  - Answer A
  - Answer B
  - Answer C
  ↓
[Sphinx judges]
  "Ah, you chose [answer]. Here is why..."
  ✨ Unlocked: Sacred Scroll
  📊 Respect updates
  ↓
[Next riddle or passage granted]
  If more: Continue to next riddle
  If done: "You have earned passage."
  ↓
[Result: Merit & Scrolls]
  Merit: WORTHY
  Scrolls Earned: 4/6
  Respect Stats displayed
  [Replay] [Back to Oases]
```

---

## The Sphinx's Personality

The Sphinx is:
- Wise but not patronizing
- Patient (she's tested a thousand travelers)
- Clear about trade-offs
- Fair and honest
- Capable of dry humor

Her tone never changes; her respect meter does.

---

## Respect System (Always Visible)

| Axis | Low (0) | Mid (50) | High (100) |
|------|---------|----------|-----------|
| **Trust** | Reckless | Balanced | Wise |
| **Caution** | Gambler | Pragmatic | Protects what matters |
| **Speed** | Too slow | Realistic | Closes when it counts |

---

## Sacred Scrolls & Wisdom Tablets

**Sacred Scrolls** = Core contract templates (MSAs, DPAs)
**Wisdom Tablets** = Specific clause guidance, tactics

Each represents knowledge the Traveler gains from the Sphinx's test.

---

## Merit Levels (Replacing Grades)

```javascript
MERIT = {
  ENLIGHTENED: "The Sphinx bows slightly. 'You walk the way of masters.'",
  WORTHY: "The Sphinx nods. 'You have proven your judgment.'",
  PREPARED: "The Sphinx is satisfied. 'You will learn. Return when you understand more.'",
  NOVICE: "The Sphinx is patient. 'You have much to learn. Return when you are ready.'",
  UNWORTHY: "The Sphinx does not step aside. 'You are not ready. Return when prepared.'",
};
```

---

## File Structure (After Phase 1)

```
index.html
├── js/
│   ├── game.js         (riddle flow, respect, merit)
│   ├── render.js       (oasis rendering; swappable)
│   ├── data.js         (oases, sphinx, scrolls, respect)
│   └── scenarios.js    (YOUR EXISTING SCENARIOS)
├── assets/
│   ├── oasis-desert.svg
│   ├── sphinx-silhouette.svg
│   └── ...
├── DESIGN.md
└── THEME.md (optional: world-building, lore)
```

---

## Phase Breakdown

| Phase | Deliverable | Outcome |
|-------|-------------|---------|
| **1** | Riddle dialog UI | Sphinx asks, Traveler answers |
| **2** | Oasis + Sphinx | Visual oasis appears |
| **3** | Click-to-approach | Traveler can engage Sphinx |
| **4** | Respect + scrolls | Traveler earns wisdom |
| **5** | Polish | Sphinx feels alive |
| **6+** | Extensions | More oases, animations, 3D |

---

## Token Strategy

- Phase 1–2 (Sonnet): ~2200 tokens
- Phase 3–4 (Sonnet/Haiku): ~1700 tokens
- Phase 5 (Haiku): ~400 tokens
- **Total MVP: ~4.3K tokens**

---

## Design Principles

1. **One wise mentor** — The Sphinx recurs across all scenarios
2. **Test, not fight** — Riddles are challenges; skill and preparation matter
3. **Professional, but mythic** — Legal training wrapped in timeless story
4. **Minimal aesthetics** — Desert, Sphinx, archway; no clutter
5. **Data-driven, expandable** — JSON-driven; render layer swaps for future 3D
6. **MVP mindset** — Multiple choice now; fancy animations later

---

## Next Steps

✅ **Gatekeeper Theme Approved**
→ **Phase 1**: Refactor quiz → Sphinx riddle dialogue (Sonnet 4.6, ~1000 tokens)

Ready for Phase 1?
