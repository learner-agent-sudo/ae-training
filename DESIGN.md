# Close the Deal — Game Redesign (MVP to 3D)

## Overview
Transform the current quiz-based training into an **RPG-lite experience** where the AE (player character) enters a room, meets an NPC mentor, and answers legal/commercial questions to "unlock treasures" (tips, tools, progression). **MVP is simple** (1 room, 1 NPC), but architecture leaves room for 3D, multiple rooms/NPCs later.

---

## MVP Scope (Phase 1–5)

### What we build:
- **1 room** (interior office/meeting space, fixed view)
- **1 NPC** (Legal Coach, always present)
- **Visual layer**: Simple 2D (CSS/SVG sprites, no complex animation)
- **Interaction**: Click NPC → dialog box opens → multiple-choice answers → NPC responds with feedback
- **Rewards**: Health/energy pickups, unlocked tips, progression bar
- **Faith system**: AE's "alignment" (relationship/risk/velocity) shown as stats, evolves with choices

### What we defer (Phase 6+):
- 3D graphics (Three.js)
- Multiple rooms/navigation
- Multiple NPCs
- Complex animations
- Inventory system
- Quest log

---

## Data Schema (MVP)

```javascript
// 1. World state (future: array of rooms)
world = {
  currentRoom: "office",
  rooms: {
    office: {
      id: "office",
      npcId: "legal-coach",
      backgroundImage: "assets/office.svg",
      // Future: items: [...], exits: [...]
    }
  }
};

// 2. NPCs (future: array)
npcs = {
  "legal-coach": {
    id: "legal-coach",
    name: "Morgan",
    sprite: "assets/npc-coach.svg",
    position: { x: 400, y: 300 },  // Fixed for MVP
    currentDialog: null,
    // Future: personality, bias, relationship history
  }
};

// 3. AE's Faith (alignment system, replaces generic meters)
faith = {
  relationship: 70,    // 0–100, how much NPC likes AE
  riskAversion: 20,    // 0–100, how cautious vs. bold AE is
  velocity: 60,        // 0–100, how fast AE pushes deals
  // These evolve as AE answers questions
};

// 4. Scenarios (EXISTING, no change)
// Use SCENARIOS array as-is; each scenario = one "conversation session" with NPC
// NPC asks questions from scenario.steps[i].title
// AE picks answers from scenario.steps[i].choices
// NPC responds with coach text + rewards

// 5. Session state
session = {
  scenarioId: "acme-corp",
  stepIndex: 0,
  treasuresUnlocked: [],  // Array of { type, label, reward }
  healthRecovered: 0,     // Total health pickups used
  scoreComposite: 0,      // Final grade (same formula as before)
};
```

---

## Architecture Layers

```
┌─────────────────────────────────────────┐
│  UI Layer (render.js)                   │
│  - DOM/Canvas rendering                 │
│  - Event handlers (click NPC, pick answer)
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│  Game Logic (game.js)                   │
│  - State transitions                    │
│  - Faith deltas (+ Sonnet LLM later)    │
│  - Treasure unlock logic                │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│  Data Layer (data/)                     │
│  - scenarios.js (YOUR EXISTING)         │
│  - world.js (rooms, NPCs)               │
│  - faith.js (alignment rules)           │
└─────────────────────────────────────────┘
```

**Why this matters for 3D**: The *render.js* is swappable. Today: DOM/CSS. Tomorrow: Canvas/Three.js. Everything else stays.

---

## User Flow (MVP)

```
[Home Screen]
  ↓
[Pick Scenario] → "Closing Acme Corp — $25K ARR"
  ↓
[Room renders] → NPC visible, waiting
  Player sees: Room background + NPC sprite + "Talk to Morgan" button
  ↓
[Click NPC]
  ↓
[Dialog Box opens]
  NPC: "Your goal is to close the deal. Acme Corp just sent their MSA..."
  (This is scenario.steps[0].body)
  ↓
[Multiple Choice Answers]
  - "Use their MSA as starting point" (+relationship, +risk)
  - "Propose our MSA instead" (+velocity, -risk)
  - "Escalate to legal" (-velocity, -risk, -relationship)
  ↓
[Pick Answer]
  ↓
[Feedback + Reward]
  NPC: "Good call! Here's why..."  (coach text)
  📦 Unlocked: "Paper Precedence Tip"
  🔴 Health: -5 (risk increased)
  ↓
[Next Question or End]
  If more steps: "Continue" → back to dialog
  If done: Grade + treasures earned
  ↓
[Result Screen]
  Grade: A / B / C / D / F
  Treasures: 3/6 unlocked
  Faith: Relationship 78, Risk 35, Velocity 65
  [Replay] [Back to Scenarios]
```

---

## Faith System (AE's "Alignment")

Instead of three abstract meters (velocity, risk, relation), we track **AE's philosophy**:

| Axis | Low (0) | Mid (50) | High (100) |
|------|---------|----------|-----------|
| **Relationship** | Cold, legal-first | Balanced | Sales-first, sweet deals |
| **Risk Aversion** | Wild west (trust nothing) | Pragmatic | Risk-averse (block deals) |
| **Velocity** | Slow, deliberate | Negotiator | Pusher, "sign now" |

**How it evolves**:
- Each choice has deltas: `{ relationship: +3, risk: -15, velocity: +8 }`
- Faith updates after each answer
- **Future (Phase 4+)**: NPC's tone shifts based on faith (e.g., if AE is too risk-averse, NPC says "You're overthinking this")

**Unlock rules**:
- "Paper Precedence Tip" unlocks if `velocity > 50` (AE moves fast)
- "Super-Cap Negotiation" unlocks if `risk < 30` (AE is cautious)
- Etc.

---

## Treasures / Rewards

Each scenario has **6 treasures** (one per step). Unlocking depends on faith + answer quality:

```javascript
treasures = {
  "acme-corp": [
    { step: 0, label: "Paper Precedence Tip", icon: "📄", unlockIf: (faith) => faith.velocity > 50 },
    { step: 1, label: "Super-Cap Negotiation", icon: "💰", unlockIf: (faith) => faith.riskAversion > 40 },
    { step: 2, label: "Auto-Renewal Compromise", icon: "🔄", unlockIf: (faith) => faith.relationship > 60 },
    // ...
  ]
};
```

**Health Pickups** (visual reward, optional):
- NPC offers a coffee ☕ or water 💧 as small morale boost
- No game mechanic yet; visual only

---

## Extension Points (for future phases)

**Phase 6: Multiple Rooms**
```javascript
world.rooms = {
  office: { /* current */ },
  negotiation_table: { /* new room */ },
  executive_suite: { /* new room */ }
};
// Rename to navigate
```

**Phase 7: Multiple NPCs**
```javascript
npcs = {
  "legal-coach": { /* current */ },
  "sales-mentor": { /* new NPC */ },
  "customer-contact": { /* new NPC */ }
};
// Click different NPCs for different dialog branches
```

**Phase 8: 3D Rendering**
```javascript
// Swap render.js:
// OLD: renderDOM() using HTML/CSS
// NEW: renderCanvas() or renderThreeJS()
// Game logic unchanged
```

**Phase 9: Navigation**
```javascript
// Add arrow keys or click-to-move
// Camera follows AE
// NPCs have multiple positions per room
```

---

## File Structure (After Phase 1)

```
index.html              (main entry point, imports modules)
├── js/
│   ├── game.js         (core game loop, state, faith system)
│   ├── render.js       (DOM rendering; swappable for Canvas later)
│   ├── data.js         (world.js + npcs.js combined)
│   └── scenarios.js    (YOUR EXISTING SCENARIOS, unchanged)
├── assets/
│   ├── office.svg      (room background)
│   ├── npc-coach.svg   (NPC sprite)
│   └── ...
└── DESIGN.md           (this file, for future devs)
```

---

## Phase Breakdown (Revised)

| Phase | Deliverable | What's New |
|-------|-------------|-----------|
| **1** | NPC dialog UI | Replace quiz layout with dialog box + choice buttons |
| **2** | Room + NPC sprite | Add SVG room background + NPC sprite (CSS positioned) |
| **3** | Click-to-talk interaction | Click NPC → dialog opens; can move on to next scenario |
| **4** | Faith + treasures | Unlock treasures based on faith; show stats on result screen |
| **5** | Polish | Animations, smooth transitions, visual juice |
| **6+** | Extensions (defer) | Multiple rooms, NPCs, 3D, navigation |

---

## Token Strategy by Phase

- **Phase 1** (Sonnet): Refactor UI (moderate)
- **Phase 2** (Sonnet): Add SVG/CSS (moderate)
- **Phase 3** (Haiku): Event handlers (light)
- **Phase 4** (Sonnet): Faith logic (moderate)
- **Phase 5** (Haiku): CSS polish (light)
- **Later phases**: Re-evaluate (Sonnet for architecture, Haiku for implementation)

---

## Open Questions for You

1. **NPC visual style**: Simple SVG silhouette, or something with more personality (illustrated face)?
2. **Room background**: Abstract/minimal, or realistic office photo?
3. **Faith display**: Always visible (top-right stats bar), or only shown at end?
4. **Treasures**: Show unlocked count per scenario ("3/6"), or hidden until end?
5. **Tone**: Keep it professional, or add humor/flavor text?

---

## Next Steps

- **You review** this design (offline, 0 tokens)
- **You give feedback**: "approve", or "change X, Y, Z"
- **Phase 0 done**: Commit DESIGN.md
- **Phase 1 starts**: Refactor quiz → NPC dialog UI (Sonnet, ~1000 tokens)
