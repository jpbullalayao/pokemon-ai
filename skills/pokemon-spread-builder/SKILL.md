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

- **Minimize EVs:** always report the **least total EV investment** that meets
  the resolved target. Search from low investment upward; never default to max
  investment.
- **Breakpoint tie-break:** when adjacent EV values produce **identical**
  damage rolls, report the **lower** EV count (e.g. 23 Def over 24 Def).
- **Roles:** infer from prompt — "my X needs to live Y from Z" → X = defender;
  "how much SpA does X need to 2HKO Y" → X = attacker, Y = defender with stated
  spread.

---

## Procedure A — Defender (survive / meet bulk target)

1. **Resolve target outcome** (typically: survive first hit).
2. **Decode attacker** from the prompt per `interpret-damage-calc-syntax`.
3. **Run initial calc** per `pokemon-damage-calc-cli` with default defender
   spread.
4. **Search defender spreads:** iterate HP × Def (physical) or HP × SpD
   (special), including relevant defensive natures. Low → high.
5. **Stop** at the least-total-EV spread meeting the target verdict.
6. **Present:** spread, nature, and `pkmn-calc --text` desc line.

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

## Worked example A — Defender (Champions)

Target: **survive first hit**. Sneasler `32 Atk` Close Combat vs Incineroar.

```bash
pkmn-calc --champions --text -a Sneasler -d Incineroar -m "Close Combat" \
  --attacker-evs "32 Atk" --defender-nature Impish --defender-evs "31 HP / 23 Def"
# → 168-200 (83.5 - 99.5%) -- guaranteed 2HKO
```

Minimum: Impish `31 HP / 23 Def`. Boundaries: 30 HP or 22 Def → chance to OHKO;
24 Def = breakpoint identical to 23 → report 23.

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
