# Learning Profile Overhaul — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restructure the PHYS 1120 study guide to match a verbal-logical, analogy-first, concept-map learning profile — rewriting all 25 problem cards, adding ~120 question variations, a concept map tab, enhanced quiz modes, and past exam integration.

**Architecture:** Single-file SPA (`public/index.html`). All changes are HTML/CSS/JS within this one file. No build system, no external libraries beyond existing Desmos API and Google Fonts. File will grow from ~1858 lines to ~6000-7500 lines.

**Tech Stack:** Vanilla HTML/CSS/JS, inline SVG for concept map, existing Desmos API integration.

**Spec:** `docs/superpowers/specs/2026-04-02-learning-profile-overhaul-design.md`

**NOTE:** Use the real user profile (reallyjustjohnny6@gmail.com) for any account references.

---

## Task 1: Add New CSS Classes for Profile-Driven Card Structure

**Files:**
- Modify: `public/index.html` — add CSS rules in the second `<style>` block (after line 137, before `</style>`)

- [ ] **Step 1: Add story block styles**

Insert after the `.omi-hidden` rule (line 137) and before the closing `</style>`:

```css
/* ── LEARNING PROFILE: STORY-FIRST CARD STRUCTURE ── */
.story-block{background:rgba(212,168,38,.04);padding:14px 18px;border-radius:8px;font-size:15px;line-height:1.7;color:var(--text);margin-bottom:12px}
.analogy-block{border-left:3px solid var(--purple);padding:8px 14px;margin:10px 0;font-style:italic;font-size:14px;color:var(--text);background:rgba(139,108,239,.04);border-radius:0 6px 6px 0}
.analogy-block::before{content:"Think of it like... ";font-weight:600;font-style:normal;color:var(--purple)}
.chain-block{margin:10px 0;padding:10px 14px;background:rgba(255,255,255,.02);border-radius:6px}
.chain-step{display:flex;align-items:baseline;gap:6px;margin:4px 0;font-size:14px;line-height:1.5}
.chain-arrow{font-family:'JetBrains Mono',monospace;color:var(--gold);font-weight:700;flex-shrink:0}
.math-confirms{margin:10px 0}
.math-confirms .section-label{font-family:'JetBrains Mono',monospace;font-size:11px;color:var(--text-dim);text-transform:uppercase;letter-spacing:1px;margin-bottom:6px}
.math-verbal{font-size:12px;color:var(--text-dim);margin-top:2px;margin-bottom:8px;padding-left:12px;font-style:italic}
.diagram-caption{font-size:12px;color:var(--text-dim);text-align:center;margin-top:6px;font-style:italic}
.connects-section{display:flex;flex-wrap:wrap;gap:6px;padding:10px 20px;border-top:1px solid var(--border)}
.connects-pill{font-family:'JetBrains Mono',monospace;font-size:11px;padding:4px 10px;border-radius:12px;background:rgba(212,168,38,.08);border:1px solid rgba(212,168,38,.15);color:var(--gold);cursor:pointer;transition:all .2s;text-decoration:none}
.connects-pill:hover{background:rgba(212,168,38,.15);box-shadow:0 0 8px rgba(212,168,38,.2)}
.connects-label{font-size:11px;color:var(--text-dim);align-self:center}

/* ── VARIATIONS ── */
.variation-toggle{display:flex;align-items:center;gap:8px;padding:10px 20px;border-top:1px solid var(--border);cursor:pointer;user-select:none;transition:background .2s;font-family:'JetBrains Mono',monospace;font-size:12px;color:var(--text-dim)}
.variation-toggle:hover{background:rgba(212,168,38,.04);color:var(--gold)}
.variation-chevron{transition:transform .3s;font-size:14px}
.variation-chevron.open{transform:rotate(90deg)}
.variation-count{background:rgba(212,168,38,.12);color:var(--gold);padding:2px 8px;border-radius:10px;font-size:10px;font-weight:700}
.variation-container{max-height:0;overflow:hidden;transition:max-height .4s ease}
.variation-container.open{max-height:20000px}
.variation-mini{padding:14px 20px;border-top:1px solid rgba(255,255,255,.04)}
.variation-mini:nth-child(odd){background:rgba(255,255,255,.01)}
.variation-mini .vm-title{font-family:'JetBrains Mono',monospace;font-size:12px;color:var(--gold);margin-bottom:6px;font-weight:600}
.variation-mini .chain-step{font-size:13px}
.variation-mini .eq{font-size:12px}
.variation-mini .answer-box{font-size:12px}
.variation-mini .story-block{font-size:13px;padding:8px 12px}

/* ── CONCEPT MAP TAB ── */
.map-container{max-width:1100px;margin:0 auto;padding:20px}
.map-chain-banner{text-align:center;padding:16px 20px;background:rgba(212,168,38,.06);border:1px solid rgba(212,168,38,.12);border-radius:10px;margin-bottom:24px;font-size:14px;line-height:1.7;color:var(--text)}
.map-chain-banner .chain-arrow{display:inline}
.map-svg-container{width:100%;overflow-x:auto}
.map-node{cursor:pointer;transition:all .2s}
.map-node:hover rect{stroke-width:3;filter:drop-shadow(0 0 8px rgba(212,168,38,.3))}
.map-node:hover text{fill:#fff}
.map-connection{stroke-width:1.5;fill:none;opacity:.4;transition:opacity .2s}
.map-connection:hover{opacity:1;stroke-width:2.5}
.map-tooltip{position:absolute;background:var(--card);border:1px solid var(--gold);border-radius:6px;padding:8px 12px;font-size:12px;color:var(--text);max-width:250px;pointer-events:none;z-index:100;display:none;box-shadow:0 4px 12px rgba(0,0,0,.4)}
.map-hub-label{font-family:'JetBrains Mono',monospace;font-size:14px;font-weight:700;letter-spacing:1px}
@media(max-width:800px){
  .map-svg-container{display:none}
  .map-mobile-list{display:block!important}
}
.map-mobile-list{display:none;padding:0 16px}
.map-mobile-hub{margin-bottom:20px}
.map-mobile-hub h3{font-family:'JetBrains Mono',monospace;font-size:14px;margin-bottom:8px}
.map-mobile-node{padding:8px 12px;margin:4px 0;background:rgba(255,255,255,.03);border:1px solid var(--border);border-radius:6px;cursor:pointer;font-size:13px;transition:border-color .2s}
.map-mobile-node:hover{border-color:var(--gold)}
.map-mobile-conn{padding:4px 12px 4px 24px;font-size:11px;color:var(--text-dim)}

/* ── QUIZ MODES ── */
.quiz-mode-bar{display:flex;justify-content:center;gap:8px;margin-bottom:20px}
.quiz-mode-btn{font-family:'JetBrains Mono',monospace;font-size:12px;padding:8px 20px;border:1px solid var(--border);border-radius:6px;background:transparent;color:var(--text-dim);cursor:pointer;transition:all .3s}
.quiz-mode-btn:hover{border-color:var(--gold);color:var(--gold)}
.quiz-mode-btn.active{background:var(--gold);color:var(--bg);border-color:var(--gold)}
.concept-group{margin-top:12px;padding:8px 12px;background:rgba(212,168,38,.06);border-radius:6px}
.concept-group h4{font-family:'JetBrains Mono',monospace;font-size:12px;color:var(--gold);margin-bottom:4px}
.reasoning-gap{text-align:center;margin-top:16px;padding:12px;background:rgba(139,108,239,.08);border:1px solid rgba(139,108,239,.15);border-radius:8px;font-size:14px;color:var(--purple)}

/* ── HIGHLIGHT ANIMATION ── */
@keyframes card-highlight{0%{box-shadow:0 0 0 3px rgba(212,168,38,.6)}100%{box-shadow:0 0 0 0 rgba(212,168,38,0)}}
.card-highlighted{animation:card-highlight 1.5s ease-out}

/* ── SECTION LABELS ── */
.section-label{font-family:'JetBrains Mono',monospace;font-size:11px;color:var(--text-dim);text-transform:uppercase;letter-spacing:1px;margin-bottom:4px;display:block}
.source-ref{font-family:'JetBrains Mono',monospace;font-size:10px;color:var(--text-dim);padding:4px 0;opacity:.6}
```

- [ ] **Step 2: Verify CSS loads without errors**

Open `public/index.html` in browser. DevTools console should show no CSS parse errors. Page should look identical (new classes not yet used).

- [ ] **Step 3: Commit**

```bash
cd "C:\Users\johnn\Desktop\phys1120-study"
git add public/index.html
git commit -m "feat: add CSS classes for learning profile card structure, variations, concept map, quiz modes"
```

---

## Task 2: Add JavaScript Utilities for New Features

**Files:**
- Modify: `public/index.html` — add JS functions before the `// ============ INIT ============` section (before line 1716)

- [ ] **Step 1: Add variation toggle function**

Insert before `// ============ INIT ============`:

```javascript
// ============ LEARNING PROFILE: VARIATION TOGGLES ============
function toggleVariations(btn){
  const container=btn.nextElementSibling;
  const chevron=btn.querySelector('.variation-chevron');
  container.classList.toggle('open');
  chevron.classList.toggle('open');
}

// ============ LEARNING PROFILE: CROSS-REFERENCE SCROLL ============
function scrollToCard(cardId){
  // Switch to study tab first
  switchTab('study');
  setTimeout(()=>{
    const card=document.getElementById(cardId);
    if(!card)return;
    card.scrollIntoView({behavior:'smooth',block:'center'});
    card.classList.add('card-highlighted');
    setTimeout(()=>card.classList.remove('card-highlighted'),1500);
  },100);
}

// ============ CONCEPT MAP: NODE CLICK ============
function mapNodeClick(cardId){
  scrollToCard(cardId);
}

// ============ CONCEPT MAP: TOOLTIP ============
let mapTooltip=null;
function showMapTooltip(evt,text){
  if(!mapTooltip){
    mapTooltip=document.createElement('div');
    mapTooltip.className='map-tooltip';
    document.body.appendChild(mapTooltip);
  }
  mapTooltip.textContent=text;
  mapTooltip.style.display='block';
  mapTooltip.style.left=(evt.pageX+12)+'px';
  mapTooltip.style.top=(evt.pageY-30)+'px';
}
function hideMapTooltip(){
  if(mapTooltip)mapTooltip.style.display='none';
}

// ============ ENHANCED QUIZ: MODE SWITCHING ============
let quizMode='conceptual'; // 'conceptual','calculation','mixed'
let conceptualQuiz=[];
let mixedPool=[];

function setQuizMode(mode){
  quizMode=mode;
  document.querySelectorAll('.quiz-mode-btn').forEach(b=>b.classList.remove('active'));
  document.querySelector('[data-mode="'+mode+'"]').classList.add('active');
  // Reset quiz state
  quizIdx=0;quizScore=0;quizAnswered=false;quizMissed=[];
  document.getElementById('quiz-score-card').style.display='none';
  document.getElementById('quiz-area').style.display='block';
  renderQuizQ();
}
```

- [ ] **Step 2: Update the switchTab function to include MAP tab**

Find the existing `switchTab` function and ensure it handles `'map'` as a valid tab ID. The existing function likely uses `document.getElementById('tab-'+id)` pattern, which will work automatically if we name the new tab div `tab-map`.

- [ ] **Step 3: Commit**

```bash
cd "C:\Users\johnn\Desktop\phys1120-study"
git add public/index.html
git commit -m "feat: add JS utilities for variation toggles, cross-reference scroll, concept map, quiz modes"
```

---

## Task 3: Restructure Q1-Q4 (Magnetic Forces Core)

**Files:**
- Modify: `public/index.html` — replace Q1-Q4 card HTML (lines ~159-332)

For each card, the new structure is:
1. Add `id` attribute to the card div for cross-referencing
2. Replace card-text contents with: story-block → analogy-block → chain-block → math-confirms → existing content (mnemonic/warn preserved)
3. Keep card-svg completely unchanged
4. Add connects-section after card-body
5. Add variation-toggle + variation-container after connects-section
6. Add source-ref line

- [ ] **Step 1: Restructure Q1**

Replace the Q1 card (from `<!-- Q1 -->` to just before `<!-- Q2 -->`). The new card keeps the existing SVG diagram intact and wraps it in the new structure:

```html
<!-- Q1 -->
<div class="problem-card" id="card-q1">
<div class="card-header"><span class="q-num">Q1</span><span class="q-title">Force Direction on Proton (RHR 2)</span></div>
<div class="card-body">
<div class="card-text">
<div class="story-block">A moving charge in a magnetic field gets shoved sideways &mdash; always sideways, never speeding up or slowing down. The direction of the shove depends on which way the charge moves and which way the field points. Your right hand is the tool that figures it out.</div>
<div class="analogy-block">a crosswind hitting a jogger &mdash; the wind pushes you sideways but doesn't slow you down or speed you up. The jogger's direction and the wind's direction together determine which way you get pushed.</div>
<div class="chain-block">
<span class="section-label">Cause &rarr; Effect Chain</span>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Charge moves through a region where B field exists</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> B field pushes charge perpendicular to both v and B</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> RHR 2: fingers along v, curl toward B, thumb = F direction</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> For negative charge (electron): flip the force direction</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> If v parallel to B: sin&theta; = 0, so F = 0 (no force at all)</div>
</div>
<div class="math-confirms">
<span class="section-label">The Math Confirms It</span>
<div class="eq">F = qvB sin&theta;</div>
<div class="math-verbal">"Force equals charge times speed times field strength times how perpendicular they are"</div>
</div>
<div class="step"><span class="step-label">SUB-CASES</span></div>
<div class="sub-case"><b>(a)</b> v &rarr; (right), B &uarr; (up) &rArr; <b>F out of page</b> (&odot;)</div>
<div class="sub-case"><b>(b)</b> v &uarr; (up), B &otimes; (into page) &rArr; <b>F right</b> (&rarr;)</div>
<div class="sub-case"><b>(c)</b> v &odot; (out of page), B &rarr; (right) &rArr; <b>F up</b> (&uarr;)</div>
<div class="mnemonic">MNEMONIC: "FBI" &mdash; F = Thumb, B = Curl-to, I(v) = Fingers point</div>
<div class="warn">For NEGATIVE charges (electrons), flip the force direction!</div>
<div class="source-ref">Source: notes-19 p1, notes-20 p2</div>
</div>
<div class="card-svg">
<!-- KEEP EXISTING SVG EXACTLY AS-IS (lines 175-210) -->
<svg width="320" height="220" viewBox="0 0 320 220">
<rect width="320" height="220" fill="#0a0a12" rx="8"/>
<text x="10" y="20" fill="#888" font-size="11">(a) v right, B up</text>
<line x1="30" y1="60" x2="100" y2="60" stroke="#3ecf8e" stroke-width="2" marker-end="url(#arrowG)"/>
<text x="55" y="55" fill="#3ecf8e" font-size="11">v</text>
<line x1="65" y1="80" x2="65" y2="35" stroke="#4ea8de" stroke-width="2" marker-end="url(#arrowB)"/>
<text x="70" y="50" fill="#4ea8de" font-size="11">B</text>
<circle cx="65" cy="95" r="8" fill="none" stroke="#f0c040" stroke-width="2"/>
<circle cx="65" cy="95" r="2" fill="#f0c040"/>
<text x="78" y="99" fill="#f0c040" font-size="11">F (out)</text>
<text x="160" y="20" fill="#888" font-size="11">(b) v up, B into page</text>
<line x1="200" y1="80" x2="200" y2="35" stroke="#3ecf8e" stroke-width="2" marker-end="url(#arrowG)"/>
<text x="205" y="50" fill="#3ecf8e" font-size="11">v</text>
<circle cx="200" cy="65" r="8" fill="none" stroke="#4ea8de" stroke-width="2"/>
<line x1="194" y1="59" x2="206" y2="71" stroke="#4ea8de" stroke-width="1.5"/>
<line x1="206" y1="59" x2="194" y2="71" stroke="#4ea8de" stroke-width="1.5"/>
<text x="212" y="69" fill="#4ea8de" font-size="11">B</text>
<line x1="210" y1="60" x2="250" y2="60" stroke="#f0c040" stroke-width="2" marker-end="url(#arrowF)"/>
<text x="230" y="55" fill="#f0c040" font-size="11">F</text>
<text x="10" y="125" fill="#888" font-size="11">(c) v out of page, B right</text>
<circle cx="55" cy="165" r="8" fill="none" stroke="#3ecf8e" stroke-width="2"/>
<circle cx="55" cy="165" r="2" fill="#3ecf8e"/>
<text x="67" y="169" fill="#3ecf8e" font-size="11">v (out)</text>
<line x1="30" y1="190" x2="110" y2="190" stroke="#4ea8de" stroke-width="2" marker-end="url(#arrowB)"/>
<text x="60" y="205" fill="#4ea8de" font-size="11">B</text>
<line x1="55" y1="155" x2="55" y2="140" stroke="#f0c040" stroke-width="2" marker-end="url(#arrowF)"/>
<text x="60" y="142" fill="#f0c040" font-size="11">F</text>
<defs>
<marker id="arrowG" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><path d="M0,0 L8,3 L0,6Z" fill="#3ecf8e"/></marker>
<marker id="arrowB" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><path d="M0,0 L8,3 L0,6Z" fill="#4ea8de"/></marker>
<marker id="arrowF" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><path d="M0,0 L8,3 L0,6Z" fill="#f0c040"/></marker>
</defs>
</svg>
<div class="diagram-caption">Three cases showing how v and B directions determine F via RHR 2</div>
</div>
</div>
<div class="connects-section">
<span class="connects-label">Connects to:</span>
<a class="connects-pill" onclick="scrollToCard('card-q2')">Q2 &mdash; reverse problem</a>
<a class="connects-pill" onclick="scrollToCard('card-q4')">Q4 &mdash; what force does over time</a>
<a class="connects-pill" onclick="scrollToCard('card-q11')">Q11 &mdash; same force on electrons</a>
<a class="connects-pill" onclick="scrollToCard('card-l2')">L2 &mdash; lecture version</a>
</div>
<div class="variation-toggle" onclick="toggleVariations(this)">
<span class="variation-chevron">&#9654;</span>
<span>What else could they ask?</span>
<span class="variation-count">6 variations</span>
</div>
<div class="variation-container">
<div class="variation-mini">
<div class="vm-title">V1: Electron instead of proton</div>
<div class="story-block">Same setup, but now it's an electron. Everything about the force calculation is the same &mdash; except at the very end, you flip the direction because the charge is negative.</div>
<div class="chain-block">
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Apply RHR 2 as if positive charge</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Get force direction for positive</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Electron is negative: reverse that direction</div>
</div>
<div class="answer-box"><span class="lbl">KEY</span><br>Electron force = opposite of proton force in same setup</div>
</div>
<div class="variation-mini">
<div class="vm-title">V2: v parallel to B (F = 0 trap)</div>
<div class="story-block">If the charge moves along the same direction as the field, there's no "sideways" to push. The cross product of parallel vectors is zero.</div>
<div class="chain-block">
<div class="chain-step"><span class="chain-arrow">&rarr;</span> v and B point same direction (or opposite)</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> sin(0&deg;) = 0 or sin(180&deg;) = 0</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> F = qvB &times; 0 = 0</div>
</div>
<div class="answer-box"><span class="lbl">TRAP</span><br>No force when v is parallel or antiparallel to B</div>
</div>
<div class="variation-mini">
<div class="vm-title">V3: v at angle &theta; to B</div>
<div class="story-block">When v isn't perfectly perpendicular to B, you only get a fraction of the maximum force. The sin&theta; factor tells you how much.</div>
<div class="eq">F = qvB sin&theta;</div>
<div class="math-verbal">"At 30&deg;, you get half the max force. At 60&deg;, you get 87%. At 90&deg;, full force."</div>
<div class="answer-box"><span class="lbl">KEY</span><br>&theta; = 90&deg; gives max force. &theta; = 0&deg; or 180&deg; gives zero.</div>
</div>
<div class="variation-mini">
<div class="vm-title">V4: Find v given F, q, B</div>
<div class="story-block">Rearrange the formula: if you know the force, charge, and field, you can find how fast the particle was moving.</div>
<div class="eq">v = F / (qB sin&theta;)</div>
<div class="math-verbal">"Speed equals force divided by charge-times-field-times-angle-factor"</div>
</div>
<div class="variation-mini">
<div class="vm-title">V5: 3D direction combos (exam-style)</div>
<div class="story-block">The exam loves mixing into-page (&otimes;) and out-of-page (&odot;) with up/down/left/right. Practice all combos: v into page + B up, v out of page + B left, etc.</div>
<div class="answer-box"><span class="lbl">METHOD</span><br>Always: fingers along v, curl toward B, thumb = F. Then flip if negative charge.</div>
</div>
<div class="variation-mini">
<div class="vm-title">V6: Find &theta; given F, q, v, B</div>
<div class="story-block">Sometimes they give you all the values including force, and ask what angle the velocity makes with the field.</div>
<div class="eq">&theta; = arcsin(F / qvB)</div>
<div class="math-verbal">"The angle whose sine equals force divided by the product of charge, speed, and field"</div>
</div>
</div>
</div>
```

- [ ] **Step 2: Restructure Q2**

Replace Q2 card with same pattern. Key unique content:

```html
<!-- Q2 -->
<div class="problem-card" id="card-q2">
<div class="card-header"><span class="q-num">Q2</span><span class="q-title">Find B from Known v and F (RHR 2 Reverse)</span></div>
<div class="card-body">
<div class="card-text">
<div class="story-block">You know something is pushing a charge, and you know which way it's moving &mdash; now figure out which direction the invisible field must be pointing. It's like detective work: the clues (v and F) tell you where the culprit (B) is hiding.</div>
<div class="analogy-block">figuring out which direction the wind is blowing by watching which way a flag leans. You see the effect (flag direction) and the motion, and work backward to the cause (wind direction).</div>
<div class="chain-block">
<span class="section-label">Cause &rarr; Effect Chain</span>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Know F direction (the push on the charge)</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Know v direction (how the charge is moving)</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> RHR 2 in reverse: palm faces F, fingers along v</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> The direction your fingers curl = B direction</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Negative charge? Flip B at the end</div>
</div>
<div class="math-confirms">
<span class="section-label">The Math Confirms It</span>
<div class="eq">F = qvB sin&theta; &rArr; B = F/(qv sin&theta;)</div>
<div class="math-verbal">"Field strength equals force divided by charge-times-speed (when perpendicular)"</div>
</div>
<div class="sub-case"><b>Ex 1:</b> v &rarr; (right), F &uarr; (up) &rArr; <b>B out of page</b> (&odot;)</div>
<div class="sub-case"><b>Ex 2:</b> v &rarr; (right), F &otimes; (into page) &rArr; <b>B down</b> (&darr;)</div>
<div class="warn">Common mistake: forgetting to flip for negative charges!</div>
<div class="source-ref">Source: notes-19 p1, notes-20 p1</div>
</div>
<div class="card-svg">
<!-- KEEP EXISTING Q2 SVG EXACTLY AS-IS -->
<svg width="280" height="180" viewBox="0 0 280 180">
<rect width="280" height="180" fill="#0a0a12" rx="8"/>
<text x="10" y="18" fill="#888" font-size="11">Ex 1: v right, F up &rarr; B = ?</text>
<line x1="30" y1="70" x2="120" y2="70" stroke="#3ecf8e" stroke-width="2" marker-end="url(#arrowG)"/>
<text x="65" y="64" fill="#3ecf8e" font-size="11">v</text>
<line x1="75" y1="65" x2="75" y2="35" stroke="#f0c040" stroke-width="2" marker-end="url(#arrowF)"/>
<text x="80" y="45" fill="#f0c040" font-size="11">F</text>
<circle cx="75" cy="85" r="8" fill="none" stroke="#4ea8de" stroke-width="2"/>
<circle cx="75" cy="85" r="2" fill="#4ea8de"/>
<text x="88" y="89" fill="#4ea8de" font-size="12" font-weight="bold">B (out)</text>
<text x="10" y="118" fill="#888" font-size="11">Ex 2: v right, F into page &rarr; B = ?</text>
<line x1="30" y1="150" x2="120" y2="150" stroke="#3ecf8e" stroke-width="2" marker-end="url(#arrowG)"/>
<text x="65" y="144" fill="#3ecf8e" font-size="11">v</text>
<circle cx="75" cy="150" r="6" fill="none" stroke="#f0c040" stroke-width="1.5"/>
<line x1="71" y1="146" x2="79" y2="154" stroke="#f0c040" stroke-width="1.5"/>
<line x1="79" y1="146" x2="71" y2="154" stroke="#f0c040" stroke-width="1.5"/>
<text x="85" y="147" fill="#f0c040" font-size="10">F(in)</text>
<line x1="170" y1="150" x2="170" y2="165" stroke="#4ea8de" stroke-width="2" marker-end="url(#arrowB)"/>
<text x="178" y="163" fill="#4ea8de" font-size="12" font-weight="bold">B (down)</text>
</svg>
<div class="diagram-caption">Working backward: known v and F reveal the hidden B direction</div>
</div>
</div>
<div class="connects-section">
<span class="connects-label">Connects to:</span>
<a class="connects-pill" onclick="scrollToCard('card-q1')">Q1 &mdash; forward problem</a>
<a class="connects-pill" onclick="scrollToCard('card-q10')">Q10 &mdash; solenoid field direction</a>
</div>
<div class="variation-toggle" onclick="toggleVariations(this)">
<span class="variation-chevron">&#9654;</span>
<span>What else could they ask?</span>
<span class="variation-count">5 variations</span>
</div>
<div class="variation-container">
<div class="variation-mini">
<div class="vm-title">V1: Electron instead of proton</div>
<div class="chain-block">
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Apply RHR 2 reverse as if positive</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Get B direction for positive charge</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Electron: flip B direction</div>
</div>
</div>
<div class="variation-mini">
<div class="vm-title">V2: Wire current form (F = BIL)</div>
<div class="story-block">Instead of a single charge, a current-carrying wire sits in a field. Same RHR 2, but now I replaces v.</div>
<div class="eq">F = BIL sin&theta;</div>
<div class="math-verbal">"Force on wire = field times current times wire length times angle factor"</div>
</div>
<div class="variation-mini">
<div class="vm-title">V3: All six 3D direction combos</div>
<div class="answer-box"><span class="lbl">PRACTICE</span><br>v&rarr; F&uarr; &rArr; B&odot; | v&uarr; F&rarr; &rArr; B&otimes; | v&odot; F&rarr; &rArr; B&darr; | v&otimes; F&uarr; &rArr; B&rarr; | v&darr; F&otimes; &rArr; B&rarr; | v&rarr; F&otimes; &rArr; B&darr;</div>
</div>
<div class="variation-mini">
<div class="vm-title">V4: Find magnitude of B</div>
<div class="eq">B = F/(qv sin&theta;)</div>
<div class="math-verbal">"Divide force by charge times speed (assuming perpendicular)"</div>
</div>
<div class="variation-mini">
<div class="vm-title">V5: Given F magnitude, find if B is unique</div>
<div class="story-block">The magnitude equation alone doesn't tell you the direction. You need RHR 2 reverse for direction AND the formula for magnitude. Both are needed for a complete answer.</div>
</div>
</div>
</div>
```

- [ ] **Step 3: Restructure Q3**

Replace Q3 card. Keep existing SVG. New content:

```html
<!-- Q3 -->
<div class="problem-card" id="card-q3">
<div class="card-header"><span class="q-num">Q3</span><span class="q-title">Forces Between Parallel Wires</span></div>
<div class="card-body">
<div class="card-text">
<div class="story-block">Two wires carrying current create magnetic fields that reach each other. Those fields push on the other wire's current. Same direction currents snuggle together (attract). Opposite direction currents shove apart (repel).</div>
<div class="analogy-block">two people walking the same direction on a narrow sidewalk &mdash; they drift together. Walking opposite directions? They dodge apart.</div>
<div class="chain-block">
<span class="section-label">Cause &rarr; Effect Chain</span>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Wire 1 carries current I&#8321;</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> I&#8321; creates B field around wire 1 (RHR 1)</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Wire 2 sits in wire 1's B field</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Wire 2's current feels force from wire 1's B (F = BIL)</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Same direction = attract | Opposite = repel</div>
</div>
<div class="math-confirms">
<span class="section-label">The Math Confirms It</span>
<div class="eq">F/&#8467; = &mu;&#8320;I&#8321;I&#8322; / (2&pi;R&#8321;&#8322;)</div>
<div class="math-verbal">"Force per length equals the permeability constant times both currents divided by the distance between them (times 2&pi;)"</div>
</div>
<div class="step"><span class="step-label">WORKED EXAMPLE</span><br>I&#8321; = I&#8322; = 25 A, R = 35 cm = 0.35 m, &#8467; = 15 cm = 0.15 m</div>
<div class="calc-result">F/&#8467; = (4&pi;&times;10&#8315;&#8311;)(25)(25) / (2&pi;)(0.35) = 3.57&times;10&#8315;&#8308; N/m<br>F = (3.57&times;10&#8315;&#8308;)(0.15) &asymp; <b>5.4&times;10&#8315;&#8308; N</b> (ATTRACTIVE)</div>
<div class="mnemonic">MNEMONIC: "Same = Snuggle, Opposite = Oh no!" (attract vs repel)</div>
<div class="source-ref">Source: notes-19 pp2-4</div>
</div>
<div class="card-svg">
<!-- KEEP EXISTING Q3 SVG EXACTLY AS-IS -->
<svg width="280" height="180" viewBox="0 0 280 180">
<rect width="280" height="180" fill="#0a0a12" rx="8"/>
<text x="40" y="18" fill="#888" font-size="11">Same Direction = ATTRACT</text>
<line x1="60" y1="40" x2="60" y2="120" stroke="#3ecf8e" stroke-width="3"/>
<text x="45" y="135" fill="#3ecf8e" font-size="11">I&#8321;</text>
<line x1="60" y1="80" x2="60" y2="45" stroke="#3ecf8e" stroke-width="2" marker-end="url(#arrowG)"/>
<line x1="120" y1="40" x2="120" y2="120" stroke="#3ecf8e" stroke-width="3"/>
<text x="105" y="135" fill="#3ecf8e" font-size="11">I&#8322;</text>
<line x1="120" y1="80" x2="120" y2="45" stroke="#3ecf8e" stroke-width="2" marker-end="url(#arrowG)"/>
<line x1="65" y1="80" x2="85" y2="80" stroke="#f0c040" stroke-width="2" marker-end="url(#arrowF)"/>
<line x1="115" y1="80" x2="95" y2="80" stroke="#f0c040" stroke-width="2" marker-end="url(#arrowF)"/>
<text x="72" y="75" fill="#f0c040" font-size="10">ATTRACT</text>
<text x="170" y="18" fill="#888" font-size="11">Opposite = REPEL</text>
<line x1="190" y1="40" x2="190" y2="120" stroke="#3ecf8e" stroke-width="3"/>
<line x1="190" y1="80" x2="190" y2="45" stroke="#3ecf8e" stroke-width="2" marker-end="url(#arrowG)"/>
<line x1="250" y1="40" x2="250" y2="120" stroke="#ef4444" stroke-width="3"/>
<line x1="250" y1="60" x2="250" y2="95" stroke="#ef4444" stroke-width="2" marker-end="url(#arrowR)"/>
<line x1="185" y1="80" x2="165" y2="80" stroke="#f0c040" stroke-width="2" marker-end="url(#arrowF)"/>
<line x1="255" y1="80" x2="275" y2="80" stroke="#f0c040" stroke-width="2" marker-end="url(#arrowF)"/>
<text x="200" y="75" fill="#ef4444" font-size="10">REPEL</text>
<defs><marker id="arrowR" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><path d="M0,0 L8,3 L0,6Z" fill="#ef4444"/></marker></defs>
</svg>
<div class="diagram-caption">Same-direction currents attract; opposite-direction currents repel</div>
</div>
</div>
<div class="connects-section">
<span class="connects-label">Connects to:</span>
<a class="connects-pill" onclick="scrollToCard('card-q1')">Q1 &mdash; force on current-carrying wire</a>
<a class="connects-pill" onclick="scrollToCard('card-q10')">Q10 &mdash; solenoid as electromagnet</a>
<a class="connects-pill" onclick="scrollToCard('card-l5')">L5 &mdash; finding B=0 between wires</a>
</div>
<div class="variation-toggle" onclick="toggleVariations(this)">
<span class="variation-chevron">&#9654;</span>
<span>What else could they ask?</span>
<span class="variation-count">5 variations</span>
</div>
<div class="variation-container">
<div class="variation-mini">
<div class="vm-title">V1: Different current magnitudes</div>
<div class="story-block">When I&#8321; &ne; I&#8322;, the formula still works the same. The force is still mutual (Newton's 3rd law) &mdash; both wires feel equal and opposite forces.</div>
<div class="eq">F/&#8467; = &mu;&#8320;I&#8321;I&#8322; / (2&pi;R&#8321;&#8322;)</div>
</div>
<div class="variation-mini">
<div class="vm-title">V2: Find current given force</div>
<div class="eq">I&#8322; = (F/&#8467;)(2&pi;R&#8321;&#8322;) / (&mu;&#8320;I&#8321;)</div>
<div class="math-verbal">"Rearrange the parallel wire formula to solve for the unknown current"</div>
</div>
<div class="variation-mini">
<div class="vm-title">V3: Three parallel wires</div>
<div class="story-block">With three wires, use superposition: the force on wire 2 is the vector sum of forces from wire 1 and wire 3. Calculate each pair separately, then add.</div>
</div>
<div class="variation-mini">
<div class="vm-title">V4: Find force per unit length</div>
<div class="story-block">Sometimes they ask for F/L instead of total F. Just skip the last multiplication step.</div>
</div>
<div class="variation-mini">
<div class="vm-title">V5: B field at midpoint between wires</div>
<div class="story-block">Use RHR 1 for each wire, find B at the midpoint, then add vectors. Same-direction currents: B fields oppose at midpoint. Opposite: B fields add.</div>
<div class="answer-box"><span class="lbl">KEY</span><br>Same I direction &rarr; B=0 at exact midpoint (if equal I). Opposite I &rarr; B doubles at midpoint.</div>
</div>
</div>
</div>
```

- [ ] **Step 4: Restructure Q4**

Replace Q4 card. Keep existing SVG. New content:

```html
<!-- Q4 -->
<div class="problem-card" id="card-q4">
<div class="card-header"><span class="q-num">Q4</span><span class="q-title">+ Charge in B Out of Page &rarr; Clockwise Circle</span></div>
<div class="card-body">
<div class="card-text">
<div class="story-block">A positive charge enters a magnetic field and gets constantly shoved sideways. Since the shove is always perpendicular to motion, the charge curves &mdash; and keeps curving &mdash; tracing a perfect circle at constant speed.</div>
<div class="analogy-block">a car on an icy road turning the steering wheel &mdash; the tires push sideways (centripetal), changing direction without changing speed. The magnetic field is the invisible steering wheel for charged particles.</div>
<div class="chain-block">
<span class="section-label">Cause &rarr; Effect Chain</span>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Charge enters region where B exists</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Force appears perpendicular to velocity (always sideways)</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Charge curves in the direction of the force</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> But force keeps rotating too (still perpendicular to new v)</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Continuous perpendicular force = circular motion</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> + charge in B&odot; moving right: RHR gives F down &rarr; curves CW</div>
</div>
<div class="math-confirms">
<span class="section-label">The Math Confirms It</span>
<div class="eq">R = mv / (qB)</div>
<div class="math-verbal">"Radius equals momentum divided by how strongly the field grabs the charge"</div>
</div>
<div class="answer-box"><span class="lbl">ANSWER</span><br>Clockwise circular path (for + charge in B out of page, entering rightward)</div>
<div class="warn">Negative charge in same setup &rarr; COUNTERCLOCKWISE</div>
<div class="source-ref">Source: notes-19 p5, notes-20 p2</div>
</div>
<div class="card-svg">
<!-- KEEP EXISTING Q4 SVG EXACTLY AS-IS -->
<svg width="260" height="200" viewBox="0 0 260 200">
<rect width="260" height="200" fill="#0a0a12" rx="8"/>
<g fill="#4ea8de">
<circle cx="20" cy="20" r="2.5"/><circle cx="50" cy="20" r="2.5"/><circle cx="80" cy="20" r="2.5"/><circle cx="110" cy="20" r="2.5"/><circle cx="140" cy="20" r="2.5"/><circle cx="170" cy="20" r="2.5"/><circle cx="200" cy="20" r="2.5"/><circle cx="230" cy="20" r="2.5"/>
<circle cx="20" cy="50" r="2.5"/><circle cx="50" cy="50" r="2.5"/><circle cx="80" cy="50" r="2.5"/><circle cx="110" cy="50" r="2.5"/><circle cx="140" cy="50" r="2.5"/><circle cx="170" cy="50" r="2.5"/><circle cx="200" cy="50" r="2.5"/><circle cx="230" cy="50" r="2.5"/>
<circle cx="20" cy="80" r="2.5"/><circle cx="50" cy="80" r="2.5"/><circle cx="80" cy="80" r="2.5"/><circle cx="110" cy="80" r="2.5"/><circle cx="140" cy="80" r="2.5"/><circle cx="170" cy="80" r="2.5"/><circle cx="200" cy="80" r="2.5"/><circle cx="230" cy="80" r="2.5"/>
<circle cx="20" cy="110" r="2.5"/><circle cx="50" cy="110" r="2.5"/><circle cx="80" cy="110" r="2.5"/><circle cx="110" cy="110" r="2.5"/><circle cx="140" cy="110" r="2.5"/><circle cx="170" cy="110" r="2.5"/><circle cx="200" cy="110" r="2.5"/><circle cx="230" cy="110" r="2.5"/>
<circle cx="20" cy="140" r="2.5"/><circle cx="50" cy="140" r="2.5"/><circle cx="80" cy="140" r="2.5"/><circle cx="110" cy="140" r="2.5"/><circle cx="140" cy="140" r="2.5"/><circle cx="170" cy="140" r="2.5"/><circle cx="200" cy="140" r="2.5"/><circle cx="230" cy="140" r="2.5"/>
<circle cx="20" cy="170" r="2.5"/><circle cx="50" cy="170" r="2.5"/><circle cx="80" cy="170" r="2.5"/><circle cx="110" cy="170" r="2.5"/><circle cx="140" cy="170" r="2.5"/><circle cx="170" cy="170" r="2.5"/><circle cx="200" cy="170" r="2.5"/><circle cx="230" cy="170" r="2.5"/>
</g>
<text x="234" y="16" fill="#4ea8de" font-size="9">&#x2299; B</text>
<circle cx="120" cy="110" r="50" fill="none" stroke="#f0c040" stroke-width="2" stroke-dasharray="6,3"/>
<circle cx="170" cy="110" r="6" fill="#f0c040" stroke="#f0c040" stroke-width="1"/>
<text x="168" y="114" fill="#0a0a12" font-size="10" font-weight="bold">+</text>
<line x1="170" y1="104" x2="170" y2="80" stroke="#3ecf8e" stroke-width="2"/>
<text x="175" y="88" fill="#3ecf8e" font-size="10">v</text>
<path d="M155,65 A50,50 0 0,1 170,105" fill="none" stroke="#f0c040" stroke-width="1.5" marker-end="url(#arrowF)"/>
<text x="85" y="78" fill="#f0c040" font-size="12" font-weight="bold">CW</text>
</svg>
<div class="diagram-caption">Positive charge curves clockwise in B out of page</div>
</div>
</div>
<div class="connects-section">
<span class="connects-label">Connects to:</span>
<a class="connects-pill" onclick="scrollToCard('card-q6')">Q6 &mdash; KE stays constant (same physics)</a>
<a class="connects-pill" onclick="scrollToCard('card-q9')">Q9 &mdash; what if speed changes?</a>
<a class="connects-pill" onclick="scrollToCard('card-q15')">Q15 &mdash; alpha particle full calculation</a>
<a class="connects-pill" onclick="scrollToCard('card-l3')">L3 &mdash; lecture derivation</a>
</div>
<div class="variation-toggle" onclick="toggleVariations(this)">
<span class="variation-chevron">&#9654;</span>
<span>What else could they ask?</span>
<span class="variation-count">6 variations</span>
</div>
<div class="variation-container">
<div class="variation-mini">
<div class="vm-title">V1: Negative charge (CCW)</div>
<div class="story-block">Same setup, but an electron. The force flips, so the circle goes counterclockwise instead. This was on the 2010 exam (Q4).</div>
<div class="answer-box"><span class="lbl">ANSWER</span><br>Negative charge in B&odot;, moving right &rarr; Force UP &rarr; CCW circle</div>
</div>
<div class="variation-mini">
<div class="vm-title">V2: Find period T and frequency f</div>
<div class="story-block">The time for one full circle is the period. Amazingly, it doesn't depend on speed &mdash; faster particles make bigger circles but complete them in the same time!</div>
<div class="eq">T = 2&pi;m / (qB) &nbsp;&nbsp;&nbsp; f = qB / (2&pi;m)</div>
<div class="math-verbal">"Period depends only on mass, charge, and field &mdash; NOT on speed"</div>
<div class="warn">This is the #1 trap: T is INDEPENDENT of v. Doubling speed doubles R but T stays the same.</div>
</div>
<div class="variation-mini">
<div class="vm-title">V3: B into page instead of out</div>
<div class="story-block">Flip B direction &rarr; flip force direction &rarr; circle goes the opposite way. + charge in B&otimes; moving right curves upward &rarr; CCW.</div>
</div>
<div class="variation-mini">
<div class="vm-title">V4: Charge enters at angle to B (helix)</div>
<div class="story-block">If v has a component parallel to B, that component isn't affected by the force (F = qvB sin&theta;, and the parallel part has sin0=0). The perpendicular part still circles. Result: a helix (spiral).</div>
<div class="answer-box"><span class="lbl">KEY</span><br>v&perp; makes a circle, v&parallel; continues straight &rarr; helical (spiral) path</div>
</div>
<div class="variation-mini">
<div class="vm-title">V5: What if B doubles?</div>
<div class="chain-block">
<div class="chain-step"><span class="chain-arrow">&rarr;</span> R = mv/(qB), so if B &rarr; 2B</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> R &rarr; R/2 (radius halves)</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Tighter circle, same speed</div>
</div>
</div>
<div class="variation-mini">
<div class="vm-title">V6: Both v and B double simultaneously</div>
<div class="chain-block">
<div class="chain-step"><span class="chain-arrow">&rarr;</span> R = m(2v)/(q(2B)) = mv/(qB) = R</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Effects cancel &mdash; radius unchanged!</div>
</div>
<div class="answer-box"><span class="lbl">TRAP</span><br>Doubling both v and B leaves R the same</div>
</div>
</div>
</div>
```

- [ ] **Step 5: Verify Q1-Q4 render correctly**

Open `public/index.html` in browser. Verify:
- Story blocks show warm gold-tinted background
- Analogy blocks show purple left border, italic text
- Chain blocks show gold arrows between steps
- Math verbal translations appear below equations
- Connects pills are clickable and scroll to targets
- Variation toggles expand/collapse
- All existing SVGs still render correctly

- [ ] **Step 6: Commit**

```bash
cd "C:\Users\johnn\Desktop\phys1120-study"
git add public/index.html
git commit -m "feat: restructure Q1-Q4 with story-first card layout, analogies, chains, variations"
```

---

## Task 4: Restructure Q5-Q8 (Electromagnetic Induction Core)

**Files:**
- Modify: `public/index.html` — replace Q5-Q8 card HTML

Follow the exact same pattern as Task 3. For each card:
1. Add `id="card-qN"` to the problem-card div
2. Add story-block, analogy-block, chain-block, math-confirms
3. Keep existing SVG untouched
4. Add diagram-caption, connects-section, variation-toggle, variation-container
5. Add source-ref

**Unique content for each card (stories, analogies, chains, variations) is defined in the spec file** `docs/superpowers/specs/2026-04-02-learning-profile-overhaul-design.md` under the "Card content" section. The implementing agent MUST read the spec for:
- Q5: Variable resistor → induced current (thermostat analogy, 6 variations)
- Q6: KE constant in circular path (Ferris wheel analogy, 4 variations)
- Q7: Steady current = no induction (lights-on analogy, 4 variations)
- Q8: Bar magnet through loop (bouncer analogy, 6 variations)

- [ ] **Step 1: Restructure Q5 with full story/analogy/chain/variations**

Key unique elements for Q5:
- Story: "You slide a resistor dial and that changes the current..."
- Analogy: thermostat — room warms, AC kicks on
- Chain: Slider → R changes → I changes → B changes → Φ changes → Lenz opposes
- Variations: slider opposite direction, switch open/close, loop moves, loop rotates, battery removed
- Connects to: Q7 (trap pair), Q8 (different cause same Lenz), Q16 (rod version)

- [ ] **Step 2: Restructure Q6 with full story/analogy/chain/variations**

Key unique elements for Q6:
- Story: "The magnetic force only pushes sideways..."
- Analogy: Ferris wheel — redirects without speed change
- Chain: F ⊥ v → W = Fd cos90° = 0 → ΔKE = 0 → speed constant
- Variations: does velocity change? (yes-direction), does momentum change? (yes), is acceleration=0? (NO trap), add E field
- Connects to: Q4 (circular path), Q9 (speed changes = radius changes), L4

- [ ] **Step 3: Restructure Q7 with full story/analogy/chain/variations**

Key unique elements for Q7:
- Story: "A solenoid has constant, unchanging current..."
- Analogy: standing in room with lights on — you don't notice until someone flickers
- Chain: I steady → B steady → Φ steady → dΦ/dt = 0 → ε = 0 → no current
- Variations: increasing I, decreasing I, oscillating I, loop moves toward solenoid
- Connects to: Q5 (changing = induction), Q8 (changing Φ), Faraday's Law

- [ ] **Step 4: Restructure Q8 with full story/analogy/chain/variations**

Key unique elements for Q8:
- Story: "A magnet falls toward a loop, flux grows, loop fights back..."
- Analogy: bouncer at a door — pushes away when coming in, grabs when leaving
- Chain: Phase 1 (approaching): Φ↑ → Lenz opposes → CCW → repels. Phase 2 (leaving): Φ↓ → Lenz opposes → CW → attracts
- Variations: S-pole first, loop moves instead, horizontal, at center (zero current momentarily), two coils
- Connects to: Q5 (different cause same Lenz), Q7 (if stops → no current), Q16 (rod version)

- [ ] **Step 5: Verify Q5-Q8 render correctly**

Same checks as Task 3 Step 5.

- [ ] **Step 6: Commit**

```bash
cd "C:\Users\johnn\Desktop\phys1120-study"
git add public/index.html
git commit -m "feat: restructure Q5-Q8 with story-first layout, analogies, chains, variations"
```

---

## Task 5: Restructure Q9-Q12 (Applications)

**Files:**
- Modify: `public/index.html` — replace Q9-Q12 card HTML

Same pattern. Read spec for unique content:
- Q9: 4× speed → 4R (car on curve analogy, 6 variations including period trap)
- Q10: Solenoid + bar magnet (refrigerator magnets analogy, 5 variations)
- Q11: Electrons between magnets (bowling ball backspin analogy, 5 variations)
- Q12: Switch opens → voltmeter (two-lane road analogy, 5 variations)

- [ ] **Step 1: Restructure Q9**
- [ ] **Step 2: Restructure Q10**
- [ ] **Step 3: Restructure Q11**
- [ ] **Step 4: Restructure Q12**
- [ ] **Step 5: Verify Q9-Q12 render correctly**
- [ ] **Step 6: Commit**

```bash
git commit -m "feat: restructure Q9-Q12 with story-first layout, analogies, chains, variations"
```

---

## Task 6: Restructure Q13-Q16 (Circuits & Full Calculations)

**Files:**
- Modify: `public/index.html` — replace Q13-Q16 card HTML

Same pattern. Read spec for unique content:
- Q13: Series power R/4R (water wheels analogy, 5 variations including 3R exam variant)
- Q14: Parallel power R/5R (drinking fountains analogy, 5 variations including 2R exam variant)
- Q15: Alpha particle interactive (ball bearing in magnetic track, 5 variations) — KEEP the interactive sliders and calcQ15() function
- Q16: Rod on rails interactive (dragging hand through water, 6 variations) — KEEP the interactive sliders and calcQ16() function

**IMPORTANT for Q15 and Q16:** These cards have interactive slider elements. Preserve the slider HTML and JS functions (`calcQ15()`, `calcQ16()`). Add the story/analogy/chain structure AROUND the existing interactive elements.

- [ ] **Step 1: Restructure Q13**
- [ ] **Step 2: Restructure Q14**
- [ ] **Step 3: Restructure Q15 (preserve interactive sliders)**
- [ ] **Step 4: Restructure Q16 (preserve interactive sliders)**
- [ ] **Step 5: Verify Q13-Q16 render correctly, test sliders still work**
- [ ] **Step 6: Commit**

```bash
git commit -m "feat: restructure Q13-Q16 with story-first layout, preserving interactive elements"
```

---

## Task 7: Restructure L1-L6 (Lecture Notes)

**Files:**
- Modify: `public/index.html` — update L1-L6 cards in the OMI lecture section (lines ~1724-1853)

Same card restructure pattern, but these cards keep the `omi-source` class and red border styling. Add `id="card-lN"` for cross-referencing.

Read spec for unique content:
- L1: Bar magnets vs solenoids (fountain analogy)
- L2: Two right-hand rules (garden hose + car window analogy)
- L3: Circular motion derivation (ball on string analogy)
- L4: Tangential vs centripetal (gas pedal vs steering wheel analogy)
- L5: Finding B=0 between wires (speakers canceling analogy)
- L6: Alpha particle worked example (cannonball vs tennis ball analogy)

Each gets ~2-3 variations.

- [ ] **Step 1: Restructure L1-L3**
- [ ] **Step 2: Restructure L4-L6**
- [ ] **Step 3: Verify lecture cards render with omi-source styling intact**
- [ ] **Step 4: Commit**

```bash
git commit -m "feat: restructure L1-L6 lecture notes with story-first layout, preserve omi styling"
```

---

## Task 8: Add 3 New Exam Problem Cards

**Files:**
- Modify: `public/index.html` — add 3 new cards after L6 section, before closing `</div>` of tab-study

These are genuinely new problem types from the 2010 exam. Each gets the full 6-part card structure.

- [ ] **Step 1: Add "Metal Loop Entering B Field" card**

```html
<!-- EXAM: Metal Loop Entering B Field -->
<div class="problem-card" id="card-exam-loop">
<div class="card-header"><span class="q-num" style="background:var(--purple)">E1</span><span class="q-title">Metal Loop Pushed Into Uniform B Field</span><span class="related-badge" style="background:rgba(139,108,239,.12);color:var(--purple);border:1px solid rgba(139,108,239,.3)">FROM 2010 EXAM</span></div>
<div class="card-body">
<div class="card-text">
<div class="story-block">A rectangular loop gets pushed into a region where a magnetic field exists. As the leading edge enters, flux starts changing, and Lenz's Law determines which end of the resistor is at higher potential.</div>
<div class="analogy-block">pushing a sponge into a stream &mdash; the water creates a pressure difference across the sponge as it enters. The stream (field) pushes back on the sponge (loop).</div>
<div class="chain-block">
<span class="section-label">Cause &rarr; Effect Chain</span>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Loop pushed into B field region at constant velocity</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Leading edge enters B, trailing edge still outside</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Area inside B increases &rarr; &Phi; increases</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Lenz opposes increase &rarr; induced current creates opposing B</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Current direction determines which resistor end is higher potential</div>
</div>
<div class="math-confirms">
<span class="section-label">The Math Confirms It</span>
<div class="eq">&epsilon; = BLv</div>
<div class="math-verbal">"EMF equals field strength times the length of the edge times how fast it enters"</div>
</div>
<div class="answer-box"><span class="lbl">ANSWER</span><br>End "b" of resistor R is at higher potential (current flows from a to b through external circuit)</div>
<div class="source-ref">Source: 2010 Exam Q8</div>
</div>
</div>
<div class="connects-section">
<span class="connects-label">Connects to:</span>
<a class="connects-pill" onclick="scrollToCard('card-q8')">Q8 &mdash; both involve changing flux + Lenz</a>
<a class="connects-pill" onclick="scrollToCard('card-q16')">Q16 &mdash; same motional EMF formula</a>
</div>
<div class="variation-toggle" onclick="toggleVariations(this)">
<span class="variation-chevron">&#9654;</span>
<span>What else could they ask?</span>
<span class="variation-count">3 variations</span>
</div>
<div class="variation-container">
<div class="variation-mini">
<div class="vm-title">V1: Loop exiting the field</div>
<div class="chain-block">
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Area in B decreases &rarr; &Phi; decreases</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Lenz opposes decrease &rarr; current reverses</div>
<div class="chain-step"><span class="chain-arrow">&rarr;</span> Higher potential end flips</div>
</div>
</div>
<div class="variation-mini">
<div class="vm-title">V2: Loop fully inside the field</div>
<div class="answer-box"><span class="lbl">TRAP</span><br>Fully inside &rarr; &Phi; is constant &rarr; no EMF &rarr; no current &rarr; both ends at same potential</div>
</div>
<div class="variation-mini">
<div class="vm-title">V3: Loop enters from different side</div>
<div class="story-block">Same physics, but entering from the right instead of left reverses which edge "sees" the field first, changing the current direction.</div>
</div>
</div>
</div>
```

- [ ] **Step 2: Add "Magnetic Flux Through Circular Area" card**

New card for Φ = BA cosθ with 3D angle cases. Full story/analogy/chain/variations (4 variations covering different planes and angles).

- [ ] **Step 3: Add "B=0 Between Opposite-Direction Wires" card**

New card with worked example (25A and 75A, 40cm apart, opposite directions). Full story/analogy/chain plus the algebraic solution. 3 variations.

- [ ] **Step 4: Verify new cards render correctly**
- [ ] **Step 5: Commit**

```bash
git commit -m "feat: add 3 new exam problem cards (loop in field, flux angles, opposite wire B=0)"
```

---

## Task 9: Add Concept Map Tab

**Files:**
- Modify: `public/index.html` — add MAP tab button and tab content

- [ ] **Step 1: Add MAP tab button**

In the `.tabs` div (line ~154), add the MAP button between BOOK PROBLEMS and TOOLS:

```html
<button class="tab-btn" onclick="switchTab('book')">BOOK PROBLEMS</button>
<button class="tab-btn" onclick="switchTab('map')">MAP</button>
<button class="tab-btn" onclick="switchTab('tools')">TOOLS</button>
```

- [ ] **Step 2: Add MAP tab content div**

Add after the book tab content and before the tools tab:

```html
<!-- ==================== TAB: CONCEPT MAP ==================== -->
<div id="tab-map" class="tab-content">
<div class="map-container">

<!-- Master Chain Banner -->
<div class="map-chain-banner">
<span style="font-family:'JetBrains Mono',monospace;font-size:12px;color:var(--gold);letter-spacing:1px">THE BIG PICTURE</span><br>
Charges move <span class="chain-arrow">&rarr;</span>
Moving charges create B <span class="chain-arrow">&rarr;</span>
B exerts force on other charges <span class="chain-arrow">&rarr;</span>
Changing B creates E (Faraday) <span class="chain-arrow">&rarr;</span>
Induced E drives current (Lenz opposes) <span class="chain-arrow">&rarr;</span>
Circuits harness all of this
</div>

<!-- SVG Concept Map (desktop) -->
<div class="map-svg-container">
<svg viewBox="0 0 1000 600" width="100%" style="min-height:500px">
<!-- Hub: Magnetic Forces (gold) -->
<text x="170" y="30" class="map-hub-label" fill="#d4a826">MAGNETIC FORCES</text>
<!-- Nodes for magnetic forces cluster -->
<g class="map-node" onclick="mapNodeClick('card-q1')">
<rect x="20" y="50" width="130" height="36" rx="8" fill="rgba(212,168,38,.08)" stroke="#d4a826" stroke-width="1.5"/>
<text x="85" y="72" fill="#d4a826" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q1 Force Dir</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q2')">
<rect x="20" y="100" width="130" height="36" rx="8" fill="rgba(212,168,38,.08)" stroke="#d4a826" stroke-width="1.5"/>
<text x="85" y="122" fill="#d4a826" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q2 Find B</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q3')">
<rect x="20" y="150" width="130" height="36" rx="8" fill="rgba(212,168,38,.08)" stroke="#d4a826" stroke-width="1.5"/>
<text x="85" y="172" fill="#d4a826" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q3 Wire Forces</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q4')">
<rect x="180" y="75" width="140" height="36" rx="8" fill="rgba(212,168,38,.08)" stroke="#d4a826" stroke-width="1.5"/>
<text x="250" y="97" fill="#d4a826" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q4 Circular Path</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q6')">
<rect x="180" y="125" width="140" height="36" rx="8" fill="rgba(212,168,38,.08)" stroke="#d4a826" stroke-width="1.5"/>
<text x="250" y="147" fill="#d4a826" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q6 KE Constant</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q9')">
<rect x="350" y="50" width="140" height="36" rx="8" fill="rgba(212,168,38,.08)" stroke="#d4a826" stroke-width="1.5"/>
<text x="420" y="72" fill="#d4a826" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q9 Speed&rarr;Radius</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q15')">
<rect x="350" y="100" width="140" height="36" rx="8" fill="rgba(212,168,38,.08)" stroke="#d4a826" stroke-width="1.5"/>
<text x="420" y="122" fill="#d4a826" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q15 Alpha Particle</text>
</g>

<!-- Hub: EM Induction (purple) -->
<text x="170" y="260" class="map-hub-label" fill="#8b6cef">ELECTROMAGNETIC INDUCTION</text>
<g class="map-node" onclick="mapNodeClick('card-q5')">
<rect x="20" y="280" width="140" height="36" rx="8" fill="rgba(139,108,239,.08)" stroke="#8b6cef" stroke-width="1.5"/>
<text x="90" y="302" fill="#8b6cef" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q5 Var Resistor</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q7')">
<rect x="20" y="330" width="140" height="36" rx="8" fill="rgba(139,108,239,.08)" stroke="#8b6cef" stroke-width="1.5"/>
<text x="90" y="352" fill="#8b6cef" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q7 Steady I=No EMF</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q8')">
<rect x="200" y="280" width="140" height="36" rx="8" fill="rgba(139,108,239,.08)" stroke="#8b6cef" stroke-width="1.5"/>
<text x="270" y="302" fill="#8b6cef" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q8 Magnet+Loop</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q16')">
<rect x="200" y="330" width="140" height="36" rx="8" fill="rgba(139,108,239,.08)" stroke="#8b6cef" stroke-width="1.5"/>
<text x="270" y="352" fill="#8b6cef" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q16 Rod on Rails</text>
</g>

<!-- Hub: Circuits (blue) -->
<text x="600" y="260" class="map-hub-label" fill="#4ea8de">CIRCUITS &amp; APPLICATIONS</text>
<g class="map-node" onclick="mapNodeClick('card-q10')">
<rect x="550" y="50" width="150" height="36" rx="8" fill="rgba(78,168,222,.08)" stroke="#4ea8de" stroke-width="1.5"/>
<text x="625" y="72" fill="#4ea8de" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q10 Solenoid Force</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q11')">
<rect x="550" y="100" width="150" height="36" rx="8" fill="rgba(78,168,222,.08)" stroke="#4ea8de" stroke-width="1.5"/>
<text x="625" y="122" fill="#4ea8de" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q11 e- Beam Defl.</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q12')">
<rect x="550" y="280" width="150" height="36" rx="8" fill="rgba(78,168,222,.08)" stroke="#4ea8de" stroke-width="1.5"/>
<text x="625" y="302" fill="#4ea8de" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q12 Switch+Voltmtr</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q13')">
<rect x="550" y="330" width="150" height="36" rx="8" fill="rgba(78,168,222,.08)" stroke="#4ea8de" stroke-width="1.5"/>
<text x="625" y="352" fill="#4ea8de" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q13 Series Power</text>
</g>
<g class="map-node" onclick="mapNodeClick('card-q14')">
<rect x="550" y="380" width="150" height="36" rx="8" fill="rgba(78,168,222,.08)" stroke="#4ea8de" stroke-width="1.5"/>
<text x="625" y="402" fill="#4ea8de" font-size="11" text-anchor="middle" font-family="'JetBrains Mono',monospace">Q14 Parallel Power</text>
</g>

<!-- Connection lines -->
<!-- Q1 ↔ Q2 -->
<path d="M85,86 L85,100" stroke="#d4a826" class="map-connection" onmouseenter="showMapTooltip(event,'Forward vs reverse RHR')" onmouseleave="hideMapTooltip()"/>
<!-- Q1 → Q4 -->
<path d="M150,68 Q165,68 180,93" stroke="#d4a826" class="map-connection" onmouseenter="showMapTooltip(event,'Force leads to circular motion')" onmouseleave="hideMapTooltip()"/>
<!-- Q4 → Q6 -->
<path d="M250,111 L250,125" stroke="#d4a826" class="map-connection" onmouseenter="showMapTooltip(event,'Circular motion → KE constant')" onmouseleave="hideMapTooltip()"/>
<!-- Q4 → Q9 -->
<path d="M320,93 L350,68" stroke="#d4a826" class="map-connection" onmouseenter="showMapTooltip(event,'Base case → proportional reasoning')" onmouseleave="hideMapTooltip()"/>
<!-- Q4 → Q15 -->
<path d="M320,93 Q335,110 350,118" stroke="#d4a826" class="map-connection" onmouseenter="showMapTooltip(event,'Same formula, alpha particle')" onmouseleave="hideMapTooltip()"/>
<!-- Q5 ↔ Q7 (trap pair) -->
<path d="M90,316 L90,330" stroke="#ef4444" stroke-dasharray="4,3" class="map-connection" onmouseenter="showMapTooltip(event,'TRAP PAIR: changing vs steady current')" onmouseleave="hideMapTooltip()"/>
<!-- Q5 ↔ Q8 -->
<path d="M160,298 L200,298" stroke="#8b6cef" class="map-connection" onmouseenter="showMapTooltip(event,'Different cause, same Lenz Law')" onmouseleave="hideMapTooltip()"/>
<!-- Q7 ↔ Q8 (trap pair) -->
<path d="M160,348 Q180,370 200,348" stroke="#ef4444" stroke-dasharray="4,3" class="map-connection" onmouseenter="showMapTooltip(event,'TRAP PAIR: steady vs changing flux')" onmouseleave="hideMapTooltip()"/>
<!-- Q8 ↔ Q16 -->
<path d="M270,316 L270,330" stroke="#8b6cef" class="map-connection" onmouseenter="showMapTooltip(event,'Both change Φ, different mechanism')" onmouseleave="hideMapTooltip()"/>
<!-- Q13 ↔ Q14 -->
<path d="M625,366 L625,380" stroke="#ef4444" stroke-dasharray="4,3" class="map-connection" onmouseenter="showMapTooltip(event,'Series vs parallel (opposite rules!)')" onmouseleave="hideMapTooltip()"/>
<!-- Q12 ↔ Q13 -->
<path d="M625,316 L625,330" stroke="#4ea8de" class="map-connection" onmouseenter="showMapTooltip(event,'Switch changes circuit → power redistributes')" onmouseleave="hideMapTooltip()"/>

<!-- Legend -->
<rect x="20" y="460" width="960" height="50" rx="8" fill="rgba(255,255,255,.02)" stroke="var(--border)"/>
<text x="40" y="485" fill="#888" font-size="11" font-family="'JetBrains Mono',monospace">LEGEND:</text>
<line x1="120" y1="480" x2="160" y2="480" stroke="#d4a826" stroke-width="2"/><text x="170" y="485" fill="#888" font-size="11">Connection</text>
<line x1="300" y1="480" x2="340" y2="480" stroke="#ef4444" stroke-width="2" stroke-dasharray="4,3"/><text x="350" y="485" fill="#888" font-size="11">Trap pair (easy to confuse)</text>
<text x="600" y="485" fill="#888" font-size="11">Click any node to jump to that problem</text>
</svg>
</div>

<!-- Mobile fallback list -->
<div class="map-mobile-list">
<div class="map-mobile-hub">
<h3 style="color:#d4a826">Magnetic Forces</h3>
<div class="map-mobile-node" onclick="mapNodeClick('card-q1')" style="border-left:3px solid #d4a826">Q1 — Force Direction (RHR 2)</div>
<div class="map-mobile-conn">↔ Q2: forward vs reverse RHR</div>
<div class="map-mobile-conn">→ Q4: force leads to circular motion</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q2')" style="border-left:3px solid #d4a826">Q2 — Find B (RHR 2 Reverse)</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q3')" style="border-left:3px solid #d4a826">Q3 — Parallel Wire Forces</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q4')" style="border-left:3px solid #d4a826">Q4 — Circular Path in B</div>
<div class="map-mobile-conn">→ Q6: KE stays constant</div>
<div class="map-mobile-conn">→ Q9: speed changes radius</div>
<div class="map-mobile-conn">→ Q15: alpha particle version</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q6')" style="border-left:3px solid #d4a826">Q6 — KE Constant</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q9')" style="border-left:3px solid #d4a826">Q9 — Speed → Radius</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q15')" style="border-left:3px solid #d4a826">Q15 — Alpha Particle</div>
</div>
<div class="map-mobile-hub">
<h3 style="color:#8b6cef">Electromagnetic Induction</h3>
<div class="map-mobile-node" onclick="mapNodeClick('card-q5')" style="border-left:3px solid #8b6cef">Q5 — Variable Resistor → Induced I</div>
<div class="map-mobile-conn">⚠️ Q7: TRAP PAIR — changing vs steady</div>
<div class="map-mobile-conn">↔ Q8: different cause, same Lenz</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q7')" style="border-left:3px solid #8b6cef">Q7 — Steady I = No Induction</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q8')" style="border-left:3px solid #8b6cef">Q8 — Bar Magnet Through Loop</div>
<div class="map-mobile-conn">→ Q16: same Lenz, rod version</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q16')" style="border-left:3px solid #8b6cef">Q16 — Rod on Rails</div>
</div>
<div class="map-mobile-hub">
<h3 style="color:#4ea8de">Circuits & Applications</h3>
<div class="map-mobile-node" onclick="mapNodeClick('card-q10')" style="border-left:3px solid #4ea8de">Q10 — Solenoid Force on Magnet</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q11')" style="border-left:3px solid #4ea8de">Q11 — Electron Beam Deflection</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q12')" style="border-left:3px solid #4ea8de">Q12 — Switch + Voltmeter</div>
<div class="map-mobile-conn">→ Q13: circuit changes → power changes</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q13')" style="border-left:3px solid #4ea8de">Q13 — Series Power</div>
<div class="map-mobile-conn">⚠️ Q14: OPPOSITE RULE — series vs parallel</div>
<div class="map-mobile-node" onclick="mapNodeClick('card-q14')" style="border-left:3px solid #4ea8de">Q14 — Parallel Power</div>
</div>
</div>

</div>
</div>
```

- [ ] **Step 3: Verify MAP tab works**

- Tab button appears and switches content correctly
- Desktop: SVG map renders with clickable nodes
- Clicking a node switches to STUDY tab and scrolls to correct card
- Hover on connection lines shows tooltip
- Mobile: SVG hidden, list view shown

- [ ] **Step 4: Commit**

```bash
git commit -m "feat: add interactive concept map tab with clickable nodes and connection tooltips"
```

---

## Task 10: Enhanced Quiz System

**Files:**
- Modify: `public/index.html` — update quiz tab HTML and JS

- [ ] **Step 1: Add quiz mode selector bar**

In the quiz tab content, add mode buttons before the existing quiz area:

```html
<div class="quiz-mode-bar">
<button class="quiz-mode-btn active" data-mode="conceptual" onclick="setQuizMode('conceptual')">CONCEPTUAL</button>
<button class="quiz-mode-btn" data-mode="calculation" onclick="setQuizMode('calculation')">CALCULATION</button>
<button class="quiz-mode-btn" data-mode="mixed" onclick="setQuizMode('mixed')">MIXED EXAM SIM</button>
</div>
```

- [ ] **Step 2: Add conceptual quiz questions array**

Add ~20 conceptual questions (no numbers, test cause-effect reasoning):

```javascript
const conceptualQuestions = [
  {q:"A proton moves rightward through B pointing out of the page. What path does it follow?",
   opts:["Clockwise circle","Counterclockwise circle","Straight line (no force)","Spirals outward"],
   correct:0,
   why:"RHR 2: fingers right, curl toward out-of-page, thumb points down → curves down → clockwise.",
   concept:"Magnetic Forces"},
  {q:"A proton's speed is doubled in the same B field. What happens to the period T of its circular orbit?",
   opts:["T stays the same","T doubles","T halves","T quadruples"],
   correct:0,
   why:"T = 2πm/(qB) — period depends only on mass, charge, and B, NOT on speed. This is the biggest trap.",
   concept:"Magnetic Forces"},
  {q:"A solenoid carries a steady 3A current. A loop sits nearby. What current flows in the loop?",
   opts:["Zero","Clockwise","Counterclockwise","Depends on loop orientation"],
   correct:0,
   why:"Steady current → steady B → steady Φ → dΦ/dt = 0 → no EMF → no induced current.",
   concept:"EM Induction"},
  // ... 17 more conceptual questions covering all topics
];
```

- [ ] **Step 3: Update renderQuizQ to support quiz modes**

Modify the quiz rendering function to draw from the correct question pool based on `quizMode`.

- [ ] **Step 4: Update score card with concept grouping and reasoning gap**

```javascript
function showQuizScore(){
  // Group missed questions by concept
  const conceptGroups = {};
  quizMissed.forEach(m => {
    const concept = m.concept || 'General';
    if(!conceptGroups[concept]) conceptGroups[concept] = {total:0,missed:0,items:[]};
    conceptGroups[concept].missed++;
    conceptGroups[concept].items.push(m);
  });
  // Find weakest concept for reasoning gap
  let weakest = '';
  let worstRatio = 0;
  // ... generate concept-grouped display + reasoning gap sentence
}
```

- [ ] **Step 5: Verify all three quiz modes work**
- [ ] **Step 6: Commit**

```bash
git commit -m "feat: add conceptual, calculation, mixed quiz modes with concept-grouped scoring"
```

---

## Task 11: Update Key Equations Cheat Sheet in Tools Tab

**Files:**
- Modify: `public/index.html` — add or update the formula section in the TOOLS tab

- [ ] **Step 1: Add teacher's boxed equations organized by topic**

Add a new collapsible section in the tools tab with the equations from the spec, organized the teacher's way with source references. Use existing `.formula-card` styling.

- [ ] **Step 2: Include the series vs parallel memory aid box**

```html
<div class="formula-card" style="border:2px solid var(--gold)">
<h4>SERIES vs PARALLEL POWER (boxed by teacher)</h4>
<div class="formula-row"><span class="f-eq">SERIES: same I &rarr; P = I&sup2;R &rarr; P &prop; R</span><span class="f-desc">bigger R = MORE power</span></div>
<div class="formula-row"><span class="f-eq">PARALLEL: same &Delta;V &rarr; P = &Delta;V&sup2;/R &rarr; P &prop; 1/R</span><span class="f-desc">bigger R = LESS power</span></div>
</div>
```

- [ ] **Step 3: Commit**

```bash
git commit -m "feat: update key equations cheat sheet with teacher's boxed formulas and source refs"
```

---

## Task 12: Deploy and Verify

**Files:**
- No code changes — deployment and verification only

- [ ] **Step 1: Run local verification**

Open `public/index.html` directly in browser. Check:
- All 25 problem cards render with new structure
- All ~120 variations expand/collapse
- Concept map nodes are clickable
- All 3 quiz modes work
- Interactive sliders on Q15/Q16 still function
- Search still finds content in new story/analogy blocks
- Mobile responsive layout works (resize to 375px width)

- [ ] **Step 2: Check for broken SVGs**

All 43 inline SVGs should render. Scan for any `<svg>` elements that look broken.

- [ ] **Step 3: Git push and deploy**

```bash
cd "C:\Users\johnn\Desktop\phys1120-study"
git push origin master
```

Wait for Cloudflare Pages to build and deploy (~1-2 minutes).

- [ ] **Step 4: Verify production**

Navigate to https://phys1120-study.pages.dev/ and confirm:
- Page loads without errors
- New card structure visible
- Concept map tab works
- Quiz modes functional

- [ ] **Step 5: Commit any final fixes**

If any issues found during verification, fix and re-deploy.
