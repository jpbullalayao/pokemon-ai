---
name: pokemon-spread-builder
description: >
  Procedure for finding minimum-EV spreads in damage calc matchups — defender
  bulk to survive hits and attacker investment to meet a user-specified KO
  outcome (OHKO, 2HKO, 3HKO, etc.). Use when the user asks what build or EV
  spread is needed to live or survive a move; what minimum Atk/SpA or
  bulk achieves a specific calc result; optimize a spread for a matchup; or
  asks "what spread does X need to take Y from Z" or "how much SpA does X
  need to 2HKO Y".
---

# Pokemon Spread Builder

Procedure for finding **minimum-EV spreads** in damage calc matchups. Covers
defender bulk to survive hits and attacker investment to meet a user-specified
KO outcome. Orchestrates `pkmn-calc` searches.

---

## Prerequisites

Before running this procedure, load:

- `interpret-damage-calc-syntax` — decode spread notation in the prompt and read
  calc output verdicts
- `pokemon-damage-calc-cli` — run `pkmn-calc` and respect format EV caps; use
  `--json` for field abilities (e.g. Fairy Aura) per the CLI skill

Do not re-document notation, KO verdict tables, or CLI flags here.

---

## Resolve target outcome (first step)

Read the user's prompt to determine the **target calc verdict** before
searching. Do not assume OHKO unless the user asked for it.

| User says… | Target verdict (per interpret skill) |
| ---------- | ------------------------------------ |
| live / survive (first hit) | **Not** `guaranteed OHKO` and **not** `X% chance to OHKO` |
| guarantee OHKO / always KO / OHKO | `guaranteed OHKO` |
| guarantee 2HKO / ensure 2HKO | `guaranteed 2HKO` |
| guarantee 3HKO | `guaranteed 3HKO` |
| X% chance to 2HKO (etc.) | matching `X% chance to NHKO` verdict |

If the outcome is ambiguous (e.g. "KO Archaludon" with no N specified), run a
baseline calc first, then ask **one** clarifying question — or state the
assumption used (e.g. "assuming guaranteed OHKO").

Confirm the target verdict using the interpret skill's KO verdict table on
`pkmn-calc --text` output — do not paraphrase verdicts differently.

---

## Shared rules

- **Minimize EVs:** always report the **least total EV investment** on
  **unlocked** stats that meets the resolved target. Search from low investment
  upward; never default to max investment. When Spe, Atk, or SpA EVs are locked
  by the prompt, minimize only the remaining bulk axes.
- **Breakpoint tie-break:** when adjacent EV values produce **identical**
  damage rolls, report the **lower** EV count (e.g. 23 Def over 24 Def).
- **Roles:** infer from prompt — "my X needs to live Y from Z" → X = defender;
  "how much SpA does X need to 2HKO Y" → X = attacker, Y = defender with stated
  spread.

### Nature category (Procedure A)

Before searching bulk EVs — and **before any `pkmn-calc` call** — resolve which
**nature category** the spread should use. Do not default to a pure defensive
nature for every defender.

| Category | Typical natures | When |
| -------- | --------------- | ---- |
| Defensive | Bold/Impish/Calm/Careful (boost the threatened bulk stat) | Walls, supports, or user asks for bulk / defensive nature |
| Attacking | Adamant/Modest (etc.); avoid dropping the threatened bulk stat when possible | User wants offensive power preserved |
| Speed | Jolly/Timid (etc.); same bulk-drop caveat | User cares about outspeeding or speed ties |

**Resolve in this order:**

1. **Infer from the prompt** when constraints imply a category or exact nature:
   - "Modest Charizard that lives …" → attacking category, nature fixed Modest.
   - "live Rock Slide … but speed ties with max speed Modest Charizard" →
     attacking/speed category; nature Modest; Spe locked to that benchmark
     (Champions: `32 Spe` for max Spe Modest). Remaining EVs go to HP/Def for
     the survive check.
   - Incineroar / clearly supportive bulk checks without offensive wording →
     defensive category.
2. **If still ambiguous** (e.g. "What does Charizard need to live Rock Slide
   from 32 Atk Garchomp?" with no Spe/offense cue):
   - **STOP.** Ask **one** clarifying question: which nature category to
     optimize under. Option labels only — **no** commentary after each
     choice (no "— preserve SpA…", "— maximize bulk…", etc.). Example:
     - Attacking (e.g. Modest)
     - Speed (e.g. Timid)
     - Defensive (e.g. Bold/Calm)
3. **Honor an explicitly stated nature** — do not override it.

Only offer an alternate category if the user asked for options or the primary
category cannot meet the check within EV caps.

---

## Efficiency (keep Procedure A/B fast)

- After nature + target + attacker/defender decoding are resolved, run calcs
  immediately. Do not re-derive format caps, spread-move multipliers, or
  Champions stat math already covered by the prerequisite skills — pass flags
  to `pkmn-calc` and read the verdict.
- Batch a small parallel set of candidate spreads (baseline + suspected
  minimum + ±1 boundary). Prefer a few targeted calls over planning a large
  EV grid in prose.
- Decode "32 Atk" / `+` / nature markers via `interpret-damage-calc-syntax`
  once; do not re-litigate attacker nature in long internal debate.

---

## Procedure A — Defender (survive / meet bulk target)

1. **Resolve target outcome** (typically: survive first hit).
2. **Decode attacker** from the prompt per `interpret-damage-calc-syntax`
   (notation only — no calc yet).
3. **Resolve nature category** and any **EV locks** (Spe / Atk / SpA floors
   implied by speed ties, benchmarks, or stated spreads). Fix the concrete
   nature. **If nature is ambiguous, stop here and ask — do not proceed to
   step 4 until answered.**
4. **Run initial calc** per `pokemon-damage-calc-cli` with default defender
   spread and the game type specified by the harness or user prompt.
5. **Search defender bulk spreads:** iterate HP × Def (physical) or HP × SpD
   (special) on **unlocked** stats only. Low → high.
6. **Stop** at the least-total-EV spread meeting the target verdict under the
   resolved nature and locks.
7. **Present:** nature category rationale, any locked EVs, full spread, nature,
   and `pkmn-calc --text` desc line. If the primary category fails within EV
   caps, say so and offer an alternate category only then.

---

## Procedure B — Attacker (meet KO target)

1. **Resolve target outcome** (OHKO, 2HKO, 3HKO, or probability-based — per
   user prompt).
2. **Decode defender** spread from the prompt per `interpret-damage-calc-syntax`.
   Include field modifiers stated in the prompt per `pokemon-damage-calc-cli`.
3. **Fix attacker** species, nature, move, and any stated items/abilities.
4. **Search attacker offensive EVs** on the relevant stat (Atk / SpA), including
   nature variants if not fixed. Low → high.
5. **Stop** at the **least** offensive EV investment where the calc verdict
   matches the resolved target.
6. **Breakpoint tie-break:** if N and N+1 EVs produce identical rolls but both
   meet the target, report N.
7. **Present:** spread, nature, target outcome stated explicitly, and
   `pkmn-calc --text` desc line.

---

## Worked example A — Defender, defensive category (Champions)

Target: **survive first hit**. Sneasler `32 Atk` Close Combat vs Incineroar.
Incineroar is a support — defensive category (Impish).

```bash
pkmn-calc --champions --doubles --text -a Sneasler -d Incineroar -m "Close Combat" \
  --attacker-evs "32 Atk" --defender-nature Impish --defender-evs "31 HP / 23 Def"
# → 168-200 (83.5 - 99.5%) -- guaranteed 2HKO
```

Minimum: Impish `31 HP / 23 Def`. Boundaries: 30 HP or 22 Def → chance to OHKO;
24 Def = breakpoint identical to 23 → report 23.

---

## Worked example A2 — Defender, attacking + speed locks (Champions)

Target: **survive first hit**. User: "Charizard spread that lives 32 Atk Jolly
Garchomp Rock Slide but speed ties with max speed Modest Charizard."

Inferred: attacking/speed category; nature **Modest**; Spe locked **`32 Spe`**
(Champions max Spe). Search min HP × Def on remaining EV budget.

```bash
pkmn-calc --champions --doubles --text -a Garchomp -d Charizard -m "Rock Slide" \
  --attacker-evs "32 Atk" --attacker-nature Jolly \
  --defender-nature Modest --defender-evs "0 HP / 23 Def / 32 Spe"
# → 128-152 (83.6 - 99.3%) -- guaranteed 2HKO
```

Minimum: Modest `0 HP / 23 Def / 32 Spe`. Boundaries: 22 Def → 6.3% chance to
OHKO; 24 Def = breakpoint identical to 23 → report 23. This example uses
`--doubles`; Singles calcs overstate Rock Slide damage and incorrectly suggest
this spread fails.

---

## Worked example A3 — Ambiguous nature → ask first (no calc yet)

User: "What does Charizard need to live Rock Slide from 32 Atk Garchomp?"

Nature category is ambiguous (no Spe/offense/defensive cue). **Correct first
response:** ask which nature category to optimize under, with label-only
options (no per-option explanations):

> To find the minimum Charizard spread that survives Garchomp's 32 Atk Rock
> Slide (Champions, Doubles), which nature category should I optimize under?
> - Attacking (e.g. Modest)
> - Speed (e.g. Timid)
> - Defensive (e.g. Bold/Calm)

**Incorrect:** assume Modest, run the full EV search, then ask — by then the
question is late and the answer may be wrong for the user's nature. Also
incorrect: stuffing category tradeoffs into each option label.

After the user replies (e.g. attacking → Modest), proceed with Procedure A
from step 4.

---

## Worked example B — Attacker, target `guaranteed OHKO` (Champions)

Target: **guaranteed OHKO** (user explicitly asked to guarantee the KO). Timid
Floette-Mega Moonblast vs `2 HP / 0 SpD` Archaludon with Fairy Aura.

```
25 SpA Fairy Aura Floette-Mega Moonblast vs. 2 HP / 0 SpD Archaludon: 168-198 (100.5 - 118.5%) -- guaranteed OHKO

24 SpA Fairy Aura Floette-Mega Moonblast vs. 2 HP / 0 SpD Archaludon: 166-196 (99.4 - 117.3%) -- 93.8% chance to OHKO
```

Minimum attacker investment for **this** target: **25 SpA** (Timid). A different
target (e.g. `guaranteed 2HKO`) would yield a lower SpA — always search against
the user's stated outcome, not OHKO by default.
