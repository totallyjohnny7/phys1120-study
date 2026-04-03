# PHYS 1120 Study Guide — Learning Profile Overhaul

**Date:** 2026-04-02
**Project:** `C:\Users\johnn\Desktop\phys1120-study`
**Deploy:** Cloudflare Pages → https://phys1120-study.pages.dev/
**File:** `public/index.html` (single-file SPA, ~1858 lines)

---

## Goal

Restructure the entire study guide to match this learning profile:

| Mode | Weight | Current Support | Target |
|------|--------|----------------|--------|
| Verbal-Logical | 70% | ~5% (terse labels) | Primary mode for all content |
| Analogy-First | 50% | 0% (no analogies) | Every problem gets an everyday analogy |
| Concept Map | 63% | 0% (no connections) | New tab + cross-references in every card |
| Everyday Life | 63% | 0% | Analogies grounded in daily experience |
| Conceptual Narrative | 50% | ~10% (mnemonics only) | Story-first card structure |

**Approach:** Full restructure (Approach B) — rewrite card internals, add Concept Map tab, expand variations, enhance quiz. No toggle/dual-mode.

---

## 1. Card Restructure — New Problem Card Anatomy

Every problem card (Q1-Q16, L1-L6, + 3 new exam problems = 25 cards) changes from:

**Current:** CONCEPT → METHOD → math → diagram → mnemonic → warning

**New (6-part structure):**

### 1.1 "The Story" (new)
- 2-3 sentence plain-English narrative
- No symbols, no formulas
- Explains what's physically happening and why
- Written conversationally — like explaining to a friend
- Visually: warm background (`rgba(212,168,38,.04)`), 15px font, no border

### 1.2 "Think of it like..." (new)
- One everyday-life analogy grounding the physics
- Sources: shopping carts, water hoses, traffic, cooking, phone charging, etc.
- Visually: left border accent in purple, italic text

### 1.3 "Cause → Effect Chain" (new)
- Step-by-step verbal reasoning, each step a sentence
- Connected by gold arrows (→)
- No formulas — pure cause-and-effect logic
- Visually: indented, prose font with monospace arrows

### 1.4 "The Math Confirms It" (restructured from existing)
- Existing equations presented as confirmation, not primary explanation
- Each equation gets a one-line verbal translation underneath
- Example: `R = mv/(qB)` → "Radius equals momentum divided by how strongly the field grabs the charge"
- Keeps existing `.eq` styling

### 1.5 Diagram (unchanged)
- All 43 existing inline SVGs preserved exactly
- New: each gets a 1-sentence caption connecting it to the story
- Diagram containers unchanged

### 1.6 "Connects to..." (new)
- 2-3 clickable cross-references per card
- Uses pill/badge styling (existing `.related-badge` pattern)
- Click scrolls to target card and briefly highlights it (CSS animation)
- Each link has a one-line explanation of the relationship

### Card content — all 22 cards

Each card below gets the full 6-part treatment. Analogies and stories listed here are the planned content:

**Q1 — Force Direction on Proton (RHR 2)**
- Story: A moving charge in a magnetic field gets shoved sideways — always sideways, never speeding up or slowing down.
- Analogy: Like a crosswind hitting a jogger — pushes you sideways but doesn't slow you down.
- Chain: Charge moves → B field present → Force appears perpendicular to both v and B → RHR gives direction → Negative charge? Flip it.
- Variations: electron instead of proton, v parallel to B (F=0 trap), v at angle θ to B, find θ given F
- Connects to: Q2 (reverse problem), Q4 (what the force does over time), L2 (lecture version)

**Q2 — Find B from Known v and F (RHR 2 Reverse)**
- Story: You know something is pushing a charge, and you know which way it's moving — now figure out which direction the invisible field must be pointing.
- Analogy: Like figuring out which direction the wind is blowing by watching which way a flag leans.
- Chain: Know F direction → Know v direction → RHR in reverse → Palm faces F, fingers along v → Curl tells you B → Negative charge? Flip B.
- Variations: electron instead of proton, 3D direction combos (into/out of page), find magnitude of B
- Connects to: Q1 (forward problem), Q10 (velocity selector uses this logic)

**Q3 — Forces Between Parallel Wires**
- Story: Two wires carrying current create magnetic fields that reach each other. Those fields push on the other wire's current. Same direction = they snuggle together. Opposite = they shove apart.
- Analogy: Like two people walking the same direction on a narrow sidewalk — they drift together. Walking opposite directions? They dodge apart.
- Chain: Wire 1 carries current → Creates B field around it → Wire 2 sits in that B → Wire 2's current feels force from Wire 1's B → Direction depends on whether currents agree or disagree.
- Variations: different current magnitudes, find force per unit length, find current given force, three parallel wires, angled wires
- Connects to: Q1 (force on current-carrying wire), L5 (finding where B=0 between wires)

**Q4 — Positive Charge Circular Motion in B**
- Story: A positive charge enters a magnetic field and gets constantly shoved sideways. Since the shove is always perpendicular to motion, the charge curves — and keeps curving — tracing a perfect circle.
- Analogy: Like a car on an icy road turning the steering wheel — tires push sideways (centripetal), changing direction without changing speed.
- Chain: Charge enters B → Force perpendicular to velocity → Charge curves → Force still perpendicular (now in new direction) → Continuous curving = circle → Bigger speed = wider circle.
- Variations: negative charge (CCW), find period T, find frequency f, B into page vs out of page, charge enters at angle to B (helix), what if B doubles?
- Connects to: Q6 (KE stays constant — same physics), Q9 (what if speed changes?), L3 (lecture derivation), L4 (tangential vs centripetal)

**Q5 — Variable Resistor → Induced Current in Loop A**
- Story: You slide a resistor dial and that changes the current in a circuit. The changing current changes the magnetic field. A nearby loop "notices" the changing field and fights back by creating its own current.
- Analogy: Like a thermostat — when the room gets warmer, it kicks on the AC to push back. Nature resists change.
- Chain: Slider moves → R changes → I changes (V=IR) → B changes (B ∝ I) → Φ through loop changes → Lenz's Law: loop creates current to oppose the change.
- Variations: slider moves other direction (R increases), battery removed suddenly, switch opened/closed instead of slider, loop moved closer/farther, loop rotated
- Connects to: Q7 (steady current = no induction — the trap), Q8 (moving magnet version), Q11 (changing area version)

**Q6 — KE of Particle in Circular Path**
- Story: The magnetic force only pushes sideways — it never pushes along the direction of motion. Since it never pushes "with" or "against" the charge, it does zero work. Zero work means zero change in kinetic energy.
- Analogy: Like a Ferris wheel motor — it constantly redirects your seat sideways, but you're not going faster or slower at the top vs the bottom. The motor changes your direction, not your speed.
- Chain: F always ⊥ to v → Work = F·d·cos(90°) = 0 → W=0 means ΔKE=0 → Speed constant → Only direction changes.
- Variations: "does velocity change?" (yes — direction!), "does momentum change?" (yes — direction!), "is acceleration zero?" (NO — centripetal acceleration exists), electric field added (then KE does change)
- Connects to: Q4 (the circular path this refers to), Q9 (speed changes = radius changes), L4 (tangential vs centripetal trap)

**Q7 — Steady Current in Solenoid → Loop**
- Story: A solenoid has a constant, unchanging current flowing through it. That creates a constant magnetic field. A nearby loop sits in that steady field and feels... nothing. No change = no induction.
- Analogy: Like standing in a room with the lights on — you don't notice the light until someone flickers it. Steady state is invisible to induction.
- Chain: Current is steady → B is steady → Φ is steady → dΦ/dt = 0 → ε = 0 → No induced current.
- Variations: current increasing (now there IS induction), current decreasing, current oscillating (AC), what if the loop is moving toward the solenoid?
- Connects to: Q5 (changing current = induction), Q8 (moving magnet = changing Φ), Faraday's Law conceptually

**Q8 — Bar Magnet Through Loop**
- Story: A magnet falls toward a loop. As it approaches, the flux through the loop grows — and the loop fights back by creating a current that repels the magnet. Once the magnet passes through and moves away, flux decreases — and the loop switches its current to attract the magnet, still fighting the change.
- Analogy: Like a bouncer at a door — pushes you away when you're coming in, grabs you when you're leaving. Always opposes your motion.
- Chain: Phase 1: Magnet approaches → Φ increases → Lenz opposes increase → Loop repels → CCW current. Phase 2: Magnet leaves → Φ decreases → Lenz opposes decrease → Loop attracts → CW current.
- Variations: S-pole approaching instead of N-pole, magnet stationary + loop moves, horizontal setup, magnet spinning near coil, two coils (transformer preview), what happens at the exact center? (momentary zero current)
- Connects to: Q5 (different cause, same Lenz's Law), Q7 (what if magnet stops moving — current vanishes), Q12 (transformer = this principle industrialized)

**Q9 — 4× Speed → 4R Radius**
- Story: A faster charge has more momentum, so the magnetic field has a harder time bending its path. Double the speed, double the radius. Quadruple the speed, quadruple the radius.
- Analogy: Like driving a car around a curve — the faster you go, the wider you swing out. Same road (same B), but your speed determines how tight the turn is.
- Chain: v increases → mv increases → Field can't bend it as sharply → R = mv/(qB) → R scales linearly with v.
- Variations: speed halved, mass doubled, B doubled, charge doubled, speed and B both doubled (cancel out — R unchanged), find new period (T = 2πm/qB — independent of v!)
- Connects to: Q4 (the base formula), Q6 (KE relationship), Q10 (velocity selector determines v)

**Q10 — Solenoid + Battery → Force on Nearby Bar Magnet**
- Story: A battery powers a solenoid, turning it into an electromagnet. A bar magnet sits nearby. The solenoid has its own N and S poles (determined by current direction). If the solenoid's pole facing the magnet matches the magnet's pole — they repel. Opposite poles — they attract.
- Analogy: Like two refrigerator magnets — flip one around and it switches from sticking to pushing away. The solenoid is just an electromagnet you can control with a switch.
- Chain: Current flows through solenoid → RHR1 determines which end is N → Compare solenoid's pole with bar magnet's facing pole → Like poles repel, unlike attract → Force direction follows.
- Source: notes-21 p1 (solenoid B field), notes-19 pp2-4 (force between magnetic poles)
- Variations: reverse battery polarity (poles flip), different bar magnet orientation, find B inside solenoid (B = μ₀nI), what if current increases/decreases
- Connects to: L1 (solenoid = electromagnet), Q3 (force between magnetic objects), Q5 (changing solenoid current → induction nearby)

**Q11 — Electrons Between Two Bar Magnets — Effect?**
- Story: A beam of electrons flies between two magnets. The magnets create a B field in the gap. The electrons are moving charges, so the magnetic field exerts a force on them — but always sideways, never speeding them up or slowing them down. The electrons deflect perpendicular to both their motion and the field.
- Analogy: Like a bowling ball with backspin rolling across a tilted lane — the tilt pushes it sideways, curving its path without changing how fast it rolls.
- Chain: Electrons enter B field → F = qvB acts on them → Direction from RHR2 (then flip for negative charge) → Force is ⊥ to both v and B → Electrons curve → Speed unchanged, direction changed.
- Source: notes-19 p1 (F = qvB sinθ, RHR2), notes-20 p2 (F ⊥ v always)
- Variations: protons instead of electrons (opposite deflection), B field in different direction, v parallel to B (F=0 trap), find magnitude of deflection force, different magnet orientations
- Connects to: Q1 (same force formula, RHR2), Q4 (if they curve enough → circular path), Q6 (KE unchanged because F ⊥ v)

**Q12 — Switch S Opened → Voltmeter Reading**
- Story: A circuit has a switch that, when closed, connects a parallel branch. Opening the switch removes that branch, changing the total resistance and redistributing how voltage is shared across components. The voltmeter reading changes because the current redistribution changes voltage drops.
- Analogy: Like a two-lane road narrowing to one lane — traffic flow (current) decreases overall, but the remaining lane now handles all traffic, so the "pressure" (voltage) across it changes.
- Chain: S closed → parallel branch active → lower R_total → specific voltage distribution. S opens → parallel branch removed → R_total increases → I_total decreases → Voltage across remaining components redistributes → Voltmeter reads differently.
- Source: notes-17 pp2-4 (HCP 5, switch open/closed analysis)
- Variations: switch closed instead of opened, different resistor configurations, find current before/after switch, find power dissipated, multiple switches, capacitor instead of resistor in switched branch
- Connects to: Q13 (series power distribution), Q14 (parallel power distribution), Kirchhoff's rules for complex switch circuits

**Q13 — R and 4R in Series — Power Distribution**
- Story: Two resistors in series share the same current. Since power equals I²R, the bigger resistor dissipates more power. If the 4R resistor produces 40W of heat, the R resistor produces exactly 1/4 of that — because it has 1/4 the resistance but the same current flowing through it.
- Analogy: Like two water wheels on the same stream (same flow rate). The bigger wheel (4R) catches more energy from the water. The smaller wheel (R) catches proportionally less — exactly 1/4 as much.
- Chain: Series → same current I through both → P = I²R → P₄R = I²(4R) = 40W → I²R = 10W → P_R = 10W.
- Source: notes-16 p3 (HCP 2, series power: P = I²R, P ∝ R)
- Key rule: **SERIES: same I → use P = I²R → larger R gets MORE power**
- Variations: three resistors in series, find total power, find current, find voltage across each (voltage divider), R and 2R, R and 3R (exam variant: 30W in R → 90W in 3R), mixed series-parallel power
- Connects to: Q14 (parallel — opposite rule), Q12 (switch changes series/parallel configuration)

**Q14 — R and 5R in Parallel — Power Distribution**
- Story: Two resistors in parallel share the same voltage. Since power equals V²/R, the smaller resistor dissipates more power. If R produces 50W, the 5R resistor gets only 1/5 of that — because dividing by a bigger number gives less power.
- Analogy: Like two drinking fountains connected to the same water main (same pressure). The wide-mouth fountain (small R) lets more water through and "uses" more energy. The narrow one (5R) restricts flow and uses less.
- Chain: Parallel → same voltage ΔV across both → P = ΔV²/R → P_R = ΔV²/R = 50W → P₅R = ΔV²/(5R) = 10W.
- Source: notes-16 p3 (HCP 3, parallel power: P = ΔV²/R, P ∝ 1/R)
- Key rule: **PARALLEL: same ΔV → use P = ΔV²/R → larger R gets LESS power**
- Memory aid box: Series uses P=I²R (bigger R = more power). Parallel uses P=ΔV²/R (bigger R = less power).
- Variations: R and 2R parallel (exam variant: 10W in R → 5W in 2R), three parallel resistors, find total power, find current in each branch, mixed configurations
- Connects to: Q13 (series — opposite rule), Q12 (switch adds/removes parallel paths)

**Q15 — Alpha Particle in 1.1T Magnetic Field (Full Calculation)**
- Story: An alpha particle (a helium nucleus — 2 protons and 2 neutrons glued together) enters a magnetic field. With double the charge and quadruple the mass of a proton, it traces a tiny circle. This problem walks through everything: the diagram, the radius, why speed doesn't change, and the acceleration that IS there even though speed isn't changing.
- Analogy: Like throwing a heavy ball bearing through a curved magnetic track at a science museum — the heavier it is, the wider it curves, but the magnets never speed it up or slow it down.
- Chain: Identify q = 2e = 3.2×10⁻¹⁹ C, m = 6.64×10⁻²⁷ kg → Draw diagram with RHR2 → R = mv/(qB) → Calculate R → Diameter = 2R → Speed constant (F ⊥ v, W=0) → Acceleration = v²/R toward center (NOT zero!).
- Source: notes-20 p3 (P₉ Ch 19, full worked example), notes-19 p6
- Parts: (a) diagram obeying RHR, (b) diameter of path, (c) effect on speed, (d) magnitude and direction of acceleration, (e) why speed doesn't change
- Variations: proton instead of alpha, different B values, find period T, find frequency f, what if B doubles, what if speed doubles, deuteron (1p + 1n) comparison
- Connects to: Q4 (same circular motion physics), Q6 (KE constant — same principle), Q9 (proportional reasoning with same formula), L3/L4/L6 (lecture derivations)

**Q16 — Conducting Rod on Rails in 0.8T Magnetic Field (Full Calculation)**
- Story: A metal rod slides along two parallel rails in a magnetic field. As it moves, it changes the area of the circuit loop, which changes the magnetic flux, which induces a voltage (Faraday). That voltage drives a current, and that current in the magnetic field creates a force that opposes the rod's motion (Lenz). To keep the rod moving at constant speed, you must push it with a force equal to this magnetic braking force.
- Analogy: Like dragging your hand through water — the faster you move, the harder the water pushes back. The rod "drags" through the magnetic field, and the field pushes back proportionally. To maintain speed, you match the resistance.
- Chain: Rod moves → Area changes → Φ changes → ε = BℓV induced → I = ε/R flows → Current in B creates force F = BIℓ opposing motion → For constant v: F_applied = F_magnetic = B²ℓ²v/R.
- Source: notes-22 pp3-4 (motional EMF, P₉ Ch 21, P6 Ch 21)
- Parts: (a) EMF = Bℓv, (b) current magnitude and direction (Lenz's Law), (c) magnetic force on rod, (d) applied force for constant speed
- Key insight: At constant velocity → a = 0 → F_net = 0 → F_applied = F_magnetic
- Variations: rod moves opposite direction, rod accelerating, different rod lengths, find initial flux, rod at angle to rails, power dissipated in resistor, energy conservation check (P_applied = P_dissipated)
- Connects to: Q5 (same Lenz's Law principle), Q8 (changing flux → induced current), Q7 (if rod stops → steady Φ → no current)

**L1 — Magnetic Fields: Bar Magnets vs Solenoids**
- Story: Every magnetic field pattern you see from a bar magnet can be perfectly recreated by running current through the right shape of wire. There's no magic in magnets — just organized charge motion.
- Analogy: Like a fountain — natural springs and man-made pumps both produce flowing water. You can't tell which is which just by looking at the stream.
- Chain: Current flows in coil → Creates B field → Field pattern identical to bar magnet → RHR grip: thumb = North → Solenoid IS an electromagnet.
- Connects to: Q3 (force between current-carrying wires), Q7 (solenoid creating steady field), Q12 (transformer uses two solenoids)

**L2 — The Two Right-Hand Rules**
- Story: Physics gives you two tools for magnetism, both using your right hand. Rule 1 (grip) tells you what field a current creates. Rule 2 (flat hand) tells you what force a field exerts on a moving charge. Every magnetism problem uses one or both.
- Analogy: Rule 1 is like wrapping your hand around a garden hose to feel which way the water swirls. Rule 2 is like sticking your hand out a car window and feeling the wind push your palm.
- Chain: RHR1: Thumb=current, fingers=B field created. RHR2: Fingers=v, curl toward B, thumb=F (positive charge). Negative charge: flip F.
- Connects to: Q1/Q2 (RHR2 applications), Q3 (RHR1 for wire fields), L1 (RHR1 for solenoids)

**L3 — Charged Particle Circular Motion (Lecture)**
- Story: The lecture derived the radius formula by setting magnetic force equal to centripetal force. The key insight: every variable in R = mv/(qB) has a physical meaning — more momentum means wider circle, more field or charge means tighter circle.
- Analogy: Like a ball on a string — heavier ball (more m) or faster spin (more v) needs a longer string (bigger R). Pulling the string harder (stronger B) tightens the circle.
- Chain: F_magnetic = F_centripetal → qvB = mv²/r → Solve for r → r = mv/(qB) → Each variable has an intuitive role.
- Connects to: Q4 (the problem this derives), Q9 (proportional reasoning from this formula), L4 (acceleration clarification)

**L4 — Tangential vs Centripetal Acceleration**
- Story: Acceleration has two flavors that work independently. Tangential acceleration changes how fast you go. Centripetal acceleration changes which way you go. Magnetic force only provides centripetal — that's why it bends without speeding up.
- Analogy: In a car: the gas pedal is tangential acceleration (speeds you up). The steering wheel is centripetal acceleration (turns you). Magnetic force is like a steering wheel with no gas pedal.
- Chain: Tangential: parallel to v → changes speed. Centripetal: perpendicular to v → changes direction. Magnetic force is always ⊥ to v → purely centripetal → speed constant, direction changes.
- Connects to: Q6 (KE constant because of this), Q4 (circular path is the result), L3 (formula derivation)

**L5 — Finding B = 0 Between Two Wires**
- Story: Two wires each create their own magnetic field. Between them, those fields point in opposite directions (if currents are the same way). There's exactly one point where they perfectly cancel — and it's always closer to the weaker wire.
- Analogy: Like two speakers playing the same note — there's a spot between them where the sound waves cancel out (destructive interference). That spot is closer to the quieter speaker.
- Chain: Each wire makes B = μ₀I/(2πr) → Between wires with same-direction currents: B fields oppose → Set B₁ = B₂ → I₁/r₁ = I₂/r₂ → Solve for position → Zero point closer to weaker current.
- Variations: opposite-direction currents (zero point is outside), three wires, unequal spacing
- Connects to: Q3 (force between wires uses same B formula), L2 (RHR1 determines field directions)

**L6 — Alpha Particle in B Field (Worked Example)**
- Story: An alpha particle (2 protons, 2 neutrons stuck together) enters a magnetic field. It has double the charge and quadruple the mass of a proton, so its radius depends on this specific combination. The lecture walked through every step including the trap: acceleration is NOT zero even though speed is constant.
- Analogy: Like a cannonball vs a tennis ball curving in wind — the cannonball has more mass (wider curve) but might also have different cross-section (charge). You need to know both to predict the path.
- Chain: Identify q = 2e, m = 4mₚ → Use r = mv/(qB) → Find r → Diameter = 2r (read carefully!) → Direction from RHR → Speed constant (W=0) → Acceleration = v²/r (NOT zero).
- Connects to: Q4 (same formula, different particle), Q9 (proportional reasoning), L4 (the acceleration trap)

---

## 2. Concept Map Tab

### 2.1 Structure
New 5th tab labeled "MAP" between BOOK PROBLEMS and TOOLS.

### 2.2 Three Hub Nodes
Color-coded to match existing chapter scheme:

- **Magnetic Forces** (gold, `#d4a826`): Q1, Q2, Q3, Q4, Q6, Q9, Q10, Q11, Q15, L2, L3, L4, L6, Exam-FluxAngles
- **Electromagnetic Induction** (purple, `#8b6cef`): Q5, Q7, Q8, Q16, L1, Exam-LoopIntoField, Exam-OppositeWires
- **Circuits & Applications** (blue, `#4ea8de`): Q12, Q13, Q14, L5

### 2.3 Node Design
- Rounded rectangles: problem number + abbreviated title
- Click → switches to STUDY tab, scrolls to card, highlights for 1.5s
- Hover → shows the "Story" preview (first sentence)

### 2.4 Connection Lines
Curved SVG paths between related nodes. Each has a hover tooltip explaining the relationship. Key connections:

| From | To | Relationship |
|------|----|-------------|
| Q1 | Q2 | Forward vs reverse RHR |
| Q1 | Q4 | Force → circular motion consequence |
| Q1 | Q11 | Same force on electrons between magnets |
| Q4 | Q6 | Circular motion → KE constant |
| Q4 | Q9 | Base case → proportional reasoning |
| Q4 | Q15 | Same formula, alpha particle instead of proton |
| Q5 | Q7 | Changing vs steady current (trap pair) |
| Q5 | Q8 | Different cause, same Lenz's Law |
| Q5 | Q16 | Different cause (moving rod), same Lenz's Law |
| Q7 | Q8 | Steady vs changing flux (trap pair) |
| Q8 | Q16 | Bar magnet vs sliding rod — both change Φ |
| Q10 | L1 | Solenoid as electromagnet |
| Q10 | Q3 | Force between magnetic objects |
| Q12 | Q13 | Switch changes circuit → power redistributes |
| Q13 | Q14 | Series vs parallel power (opposite rules) |
| Q15 | Q9 | Same R = mv/(qB), different particle |
| Q16 | Q5 | Both use Lenz's Law, different flux-change source |
| L3 | Q4 | Lecture derivation → problem application |
| L4 | Q6 | Tangential/centripetal → KE argument |
| L5 | Exam-OppositeWires | Same-direction (between) vs opposite (outside) |
| Exam-FluxAngles | Q5 | Flux concept feeds into Faraday's Law |
| Exam-LoopIntoField | Q8 | Both involve changing flux → Lenz's Law |

### 2.5 Master Conceptual Chain
Displayed across the top of the map:
> Charges move → Moving charges create B → B exerts force on other charges → Changing B creates E (Faraday) → Induced E drives current (Lenz opposes) → Circuits harness all of this

### 2.6 Responsive Behavior
- Desktop: SVG node graph with curved connections
- Mobile (<800px): Vertical structured list, connections as indented text bullets

---

## 3. Comprehensive Question Variations

### 3.1 Variation Types (per card)

| Type | Description | Example |
|------|------------|---------|
| Variable Flips | Rearrange formula, solve for different variable | Q4: Find R → also find v, B, m, T, f |
| Sign/Direction Flips | Negative charge, flipped field/motion | Q1: proton → electron, Q8: N-pole → S-pole |
| Conceptual Reframes | Same physics, qualitative question | Q6: "Does KE change?" → "Does velocity change?" → "Is acceleration zero?" |
| Scenario Swaps | Same principle, different physical setup | Q5: resistor slider → switch closure → loop movement |
| Trap Variations | The specific wrong-answer patterns | Q9: "What about the period?" (T independent of v!) |
| Multi-Step Combos | Chain two concepts together | Q10 → Q4: velocity selector feeds mass spectrometer |

### 3.2 Presentation
- Collapsed by default: `▸ 4 variations` badge
- Expand to show mini-cards with compact Story → Chain → Math → Answer
- Each variation has its own answer box
- Estimated: 3-5 variations per card = ~80-100 total variation mini-cards

### 3.3 Variation Content Per Problem

| Problem | # Variations | Key Variations |
|---------|-------------|----------------|
| Q1 | 4 | electron, v∥B (F=0), angle θ, find v given F |
| Q2 | 3 | electron, 3D combos, find B magnitude |
| Q3 | 4 | different I magnitudes, 3+ wires, angled wires, find I given F |
| Q4 | 5 | negative charge, find T/f, B into vs out, helix (v at angle), B doubled |
| Q5 | 5 | R increases, switch open/close, loop moves, loop rotates, battery removed |
| Q6 | 4 | velocity change?, momentum change?, acceleration=0 trap, add E field |
| Q7 | 4 | increasing I, decreasing I, oscillating I, loop moves toward solenoid |
| Q8 | 5 | S-pole first, loop moves instead, horizontal, magnet at center, two coils |
| Q9 | 5 | speed halved, mass doubled, B doubled, both v&B doubled, find period |
| Q10 | 4 | find E, find B, negative charge (same v!), too fast/too slow |
| Q11 | 5 | loop pulled out, loop expanding, rotating (AC generator), at specific angle, peak EMF |
| Q12 | 5 | find V₂, find turns ratio, find I₂, DC input trap, power loss |
| Q13 | 4 | voltage divider, 3+ resistors, power per resistor, mixed with parallel |
| Q14 | 4 | two equal (R/2), 3+ resistors, current divider, complex networks |
| Q15 | 5 | find τ, V(t), I(t), discharging, time to reach X%, R or C doubled |
| Q16 | 4 | two-loop, three-loop, multiple batteries, opposing batteries |
| L1-L6 | 2-3 each | Variations of lecture-specific scenarios |

**Total estimated variations: ~85-95 mini-cards**

---

## 4. Enhanced Quiz System

### 4.1 Three Quiz Modes

**Mode selector** shown at the top of the EXAM QUIZ tab as three buttons (same style as tab buttons).

#### Conceptual Quiz (new)
- ~20 questions, no numbers
- Tests cause-effect reasoning and trap awareness
- Example: "A proton's speed doubles in the same B field. What happens to its circular path's radius and period?"
- Wrong-answer explanations use verbal cause-effect chains

#### Calculation Quiz (existing, enhanced)
- Existing 10 questions preserved
- Enhanced explanations: each wrong answer now includes the cause-effect error, not just the math error
- Example: "You got E×B because you thought the forces multiply — but they oppose each other, so the speed that balances them is E/B."

#### Mixed Exam Sim (new)
- ~25 questions randomly drawn from conceptual + calculation + variation pools
- Optional timer: 2 min per question (toggle on/off)
- Matches real exam format and pacing

### 4.2 Enhanced Score Card
- Groups missed questions by **concept cluster**, not just listing them
  - "Lenz's Law: 0/3" vs "Circular Motion: 2/2"
- **"Your reasoning gap"** — Auto-generated 1-sentence diagnosis based on error pattern
  - Example: "You understand force direction but struggle with what happens when flux decreases vs increases."
- Retake button focuses on missed concepts (optional filtered retake)

### 4.3 Book Problems Quiz
- Existing 26 book problems preserved unchanged
- Enhanced wrong-answer explanations with verbal reasoning (same treatment as calculation quiz)

---

## 5. Visual Design Changes

### 5.1 New CSS Classes

```
.story-block     — warm bg, 15px font, no border, conversational
.analogy-block   — left purple border, italic, indented
.chain-block     — gold arrows between steps, prose font
.math-confirms   — existing .eq style + verbal translation line
.diagram-caption — small text under SVG, connects to story
.connects-pill   — clickable badge, scrolls to target card
.variation-toggle — collapsed header with count badge
.variation-mini  — compact card inside expanded section
.quiz-mode-btn   — mode selector button (3 options)
.concept-group   — score card grouping header
.map-node        — concept map clickable node
.map-connection  — SVG curved line with hover tooltip
.map-chain       — master chain text banner
```

### 5.2 Color Palette Additions
No new colors — reusing existing palette:
- Story: `rgba(212,168,38,.04)` background (warm gold tint)
- Analogy: `var(--purple)` left border
- Chain arrows: `var(--gold)`
- Variation toggle: `var(--text-dim)` with gold accent on hover
- Map nodes: chapter colors (gold, purple, blue)

### 5.3 Typography
No new fonts — existing Inter + JetBrains Mono:
- Story: Inter 15px regular
- Analogy: Inter 14px italic
- Chain: Inter 14px, arrows in JetBrains Mono
- Math translation: Inter 12px, `var(--text-dim)`

### 5.4 Responsive
All new elements follow existing mobile breakpoint (`@media max-width: 800px`):
- Variation mini-cards: full width
- Concept map: switches to structured list
- Quiz mode selector: stacks vertically

---

## 6. Technical Constraints

- **Single file:** Everything stays in `public/index.html`. No build system, no dependencies beyond Desmos API and Google Fonts.
- **SVGs preserved:** All 43 existing inline SVGs untouched. New diagram captions are text elements, not SVG modifications.
- **No external libraries:** Concept map is pure SVG + CSS, no D3 or vis.js.
- **Performance:** File will grow from ~1858 lines to estimated ~6000-7500 lines (25 cards × 6-part structure + ~120 variation mini-cards + concept map SVG + enhanced quiz + past exam content). Still loads fast as a single HTML file with no network dependencies beyond fonts and Desmos.
- **Deployment:** Same Cloudflare Pages pipeline. `git push` to deploy.

---

## 7. Teacher's Notes Integration — Source References

Every card includes a "Source" line linking to the teacher's lecture notes (notes-16 through notes-22). This grounds the study guide in what the professor actually taught and emphasizes the teacher's exact reasoning style.

### Source Map (which notes cover which problems)
| Problem | Teacher's Notes |
|---------|----------------|
| Q1 | notes-19 p1 (RHR2 intro), notes-20 p2 (RHR2 detail) |
| Q2 | notes-19 p1 (RHR2 reverse), notes-20 p1 |
| Q3 | notes-19 pp2-4 (parallel wires, P3 worked), notes-19 p4 (attract/repel) |
| Q4 | notes-19 p5 (circular motion), notes-20 p2 (centripetal) |
| Q5 | notes-22 pp2-3 (Faraday/Lenz), notes-21 pp3-4 (flux) |
| Q6 | notes-19 p5, notes-20 pp2-3 ("F⊥v → KE constant") |
| Q7 | notes-22 p2 (Faraday: steady I → ΔΦ=0 → no EMF) |
| Q8 | notes-21 p3 (magnet in/out coil), notes-22 p2 (Lenz's law) |
| Q9 | notes-19 p5, notes-20 p3 (R = mv/qB, R ∝ v) |
| Q10 | notes-21 p1 (solenoid poles, RHR1), notes-19 pp2-4 |
| Q11 | notes-19 p1 (F = qvB, RHR2 for electrons), notes-20 p2 |
| Q12 | notes-17 pp2-4 (HCP 5: switch open/closed, voltage change) |
| Q13 | notes-16 p3 (HCP 2: series power, P = I²R) |
| Q14 | notes-16 p3 (HCP 3: parallel power, P = ΔV²/R) |
| Q15 | notes-20 p3 (P₉ Ch 19: alpha particle), notes-19 p6 |
| Q16 | notes-22 pp3-4 (P₉ Ch 21, P6 Ch 21: motional EMF, sliding rod) |

### Teacher's Style Integration
Each card includes the teacher's exact phrasing where available:
- Q6: "F_net ⊥ v → F does no work → KE constant → speed constant"
- Q7: Emphasis on "steady = constant = NO induction"
- Q15: "v = SPEED CHANGING? ≠! ⊥ v"
- L3: "acceleration = 0 because speed is constant — WRONG!"

---

## 8. Past Exam Integration — 2010 Exam Solutions

A new section at the bottom of the STUDY tab (or as a sub-section within BOOK PROBLEMS) containing the full 2010 past exam with worked solutions, restructured in the learning profile format.

### 8.1 Additional Problems from Past Exam

These problems either duplicate existing Q1-Q16 content (serving as additional practice with different values/setups) or introduce new problem types:

| Exam Problem | Maps to Study Guide | New Content? |
|-------------|-------------------|-------------|
| Exam Q1 (force on electron) | Q1 variation (negative charge) | Yes — electron cases with specific B/v combos |
| Exam Q2 (find B direction) | Q2 variation | Yes — wire current (F = ILB) instead of moving charge |
| Exam Q3 (parallel wire forces) | Q3 match | Confirms same content, adds B field diagrams |
| Exam Q4 (negative charge path) | Q4 variation (negative charge) | Yes — explicitly tests negative charge circular path |
| Exam Q5 (variable resistor → current) | Q5 variation | Yes — slider moves RIGHT (opposite direction), answer = CW |
| Exam Q6 (KE in circular path) | Q6 match | Confirms same answer |
| Exam Q7 (steady solenoid current) | Q7 match | Confirms same answer |
| Exam Q8 (loop pushed into B field) | New! | Metal loop entering field — higher/lower potential ends |
| Exam Q9 (bar magnet S-pole down) | Q8 variation | Yes — S-pole facing loop instead of N-pole |
| Exam Q10 (2× speed → radius) | Q9 variation | Yes — 2× instead of 4× |
| Exam Q11 (solenoid force on magnet) | Q10 match | Confirms repel/attract logic |
| Exam Q12 (electrons between magnets) | Q11 match | Confirms deflection analysis |
| Exam Q13 (switch closed → voltmeter) | Q12 variation | Yes — switch CLOSED instead of opened, reading INCREASES |
| Exam Q14 (R and 2R parallel, 10W) | Q14 variation | Yes — different R ratio and power value |
| Exam Q15 (R and 3R series, 30W) | Q13 variation | Yes — different R ratio, answer = 90W |
| Exam Q16 (magnetic flux, circular area) | New! | Φ = BA cosθ with 3D coordinate system, three angle cases |
| Exam Q17 (conducting rod on rails) | Q16 match | Same problem type, different dimensions (0.5m × 0.25m) |
| Exam Q18 (B=0 between opposite wires) | L5 variation | Yes — opposite currents (B=0 outside), worked solution |

### 8.2 New Problem Types to Add

**Exam Q8 — Metal Loop Pushed Into B Field (potential difference)**
- Story: A rectangular loop gets pushed into a region where a magnetic field exists. As the leading edge enters the field, flux starts changing, inducing an EMF. One end of the resistor is at higher potential than the other — determined by which way the induced current flows.
- Analogy: Like pushing a sponge into a stream — the water (field) creates a pressure difference across the sponge (resistor) as it enters.
- Chain: Loop enters field → Leading edge in B, trailing edge not yet → Flux increasing → Lenz opposes → Current flows to create opposing B → Determines which resistor end is at higher potential.
- Variations: loop exiting field, loop fully inside (no change → no current), different entry sides, loop at angle

**Exam Q16 — Magnetic Flux Through Circular Area (3D angles)**
- Story: A circle lies flat in the xz plane. The normal to this circle points straight up (y-direction). Magnetic flux measures how much of the field "threads through" the circle — it depends on the angle between the field and the normal.
- Analogy: Like holding a hula hoop in the rain. If the rain falls straight through the hoop (B ∥ normal), maximum rain passes through. If rain falls sideways along the hoop's plane (B ⊥ normal), zero rain passes through. At an angle, it's cos(θ) of the maximum.
- Chain: Circle in xz plane → normal = ŷ → Φ = BA cosθ where θ = angle between B and ŷ → Three cases: B along z (θ=90°, Φ=0), B at 60° from y (Φ = BA cos60°), B along y (θ=0°, Φ=BA).
- Key insight: **Φ = 0 when B is parallel to the surface (perpendicular to normal). Maximum when B is perpendicular to the surface (parallel to normal).**
- Variations: circle in xy plane (normal = ẑ), circle in yz plane (normal = x̂), tilted circle, square instead of circle, B at arbitrary angle

**Exam Q18 — B=0 Between Opposite-Direction Parallel Wires**
- Story: Two wires carry currents in opposite directions. Unlike same-direction wires (where B cancels between them), opposite-direction wires have fields that ADD between them and cancel OUTSIDE. The zero point is outside, on the side of the weaker current — because the weaker wire's field needs less distance to match the stronger one.
- Analogy: Like two speakers playing out of phase — between them the sounds reinforce (louder), but outside on one side they cancel. The quiet spot is closer to the quieter speaker.
- Chain: Opposite currents → Fields add between wires (no cancellation there) → Fields oppose outside → B₁ = B₂ when I₁/r₁ = I₂/r₂ → Solve for position → Zero point is outside, near the weaker wire.
- Worked example: I₁=25A, I₂=75A, 40cm apart → 25/x = 75/(x+0.4) → x = 0.2m from 25A wire, on the side away from 75A wire.
- Connects to: L5 (same-direction version — zero point is between wires)

### 8.3 Presentation

Past exam content integrated in two ways:
1. **As variations** within existing Q1-Q16 cards (where the exam problem is a variation of an existing problem)
2. **As new cards** at the end of the STUDY tab for genuinely new problem types (Exam Q8, Q16, Q18), labeled with exam source: "FROM 2010 EXAM"

### 8.4 Key Equations Cheat Sheet

Added to the TOOLS tab as a dedicated section (in addition to existing formula grid), organized the teacher's way with boxed equations:

```
MAGNETIC FORCE:
  On charge:  F = qvB sinθ          (notes-19 p1, notes-20 p2)
  On wire:    F = BIℓ sinθ           (notes-18 p5, notes-19 p1)
  Direction:  RHR2 (+ charge), flip for −

CIRCULAR MOTION IN B:
  qvB = mv²/R  →  R = mv/(qB)       (notes-19 p5)
  F ⊥ v → W = 0 → speed constant    (notes-20 p3)

B FIELD SOURCES:
  Long wire:  B = μ₀I/(2πR)          (notes-18 p2, notes-19 p2)
  Solenoid:   B = μ₀nI = μ₀(N/L)I   (notes-21 p1)
  Direction:  RHR1 (thumb = I, fingers = B)

PARALLEL WIRES:
  F/ℓ = μ₀I₁I₂/(2πR₁₂)             (notes-19 p2)
  Same direction → attract; opposite → repel

ELECTROMAGNETIC INDUCTION:
  Magnetic flux:  Φ = BA cosθ         (notes-21 p4, notes-22 p1)
  Faraday's law:  ε = N|ΔΦ/Δt|       (notes-22 p2)
  Lenz's law:     induced I opposes ΔΦ (notes-22 p2)
  Motional EMF:   ε = Bℓv             (notes-22 p3)

CIRCUIT POWER:
  P = IΔV = I²R = ΔV²/R              (notes-16 p1)
  SERIES:   same I → P = I²R → P ∝ R  (notes-16 p3)
  PARALLEL: same ΔV → P = ΔV²/R → P ∝ 1/R (notes-16 p3)
```

---

## 9. Updated Variation Counts

With teacher's notes and past exam content incorporated:

| Problem | Base Variations | + Exam Variations | Total |
|---------|----------------|-------------------|-------|
| Q1 | 4 | +2 (electron cases from exam Q1) | 6 |
| Q2 | 3 | +2 (wire current form from exam Q2) | 5 |
| Q3 | 4 | +1 (B field diagrams from exam Q3) | 5 |
| Q4 | 5 | +1 (negative charge path from exam Q4) | 6 |
| Q5 | 5 | +1 (slider right from exam Q5) | 6 |
| Q6 | 4 | +0 (same content) | 4 |
| Q7 | 4 | +0 (same content) | 4 |
| Q8 | 5 | +1 (S-pole variant from exam Q9) | 6 |
| Q9 | 5 | +1 (2× speed from exam Q10) | 6 |
| Q10 | 4 | +1 (like poles repel from exam Q11) | 5 |
| Q11 | 4 | +1 (specific geometry from exam Q12) | 5 |
| Q12 | 4 | +1 (switch closed from exam Q13) | 5 |
| Q13 | 4 | +1 (R and 3R from exam Q15) | 5 |
| Q14 | 4 | +1 (R and 2R from exam Q14) | 5 |
| Q15 | 5 | +0 (same problem type) | 5 |
| Q16 | 5 | +1 (different dimensions from exam Q17) | 6 |
| New: Loop into B field | — | Exam Q8 | 3 |
| New: Magnetic flux angles | — | Exam Q16 | 4 |
| New: Opposite-direction wires B=0 | — | Exam Q18 | 3 |
| L1-L6 | 2-3 each | — | ~15 |
| **Total** | | | **~120 variations** |

---

## 10. What's NOT Changing

- Header, title, subtitle
- Search functionality (works on all new text content automatically)
- Desmos calculator integration
- Tools tab (formula sheets, unit tables, constants)
- Omi lecture tag system (red border styling)
- Overall dark theme and color scheme
- Existing SVG diagrams (all preserved)
- Book problems tab structure (quiz engine enhanced but structure same)
