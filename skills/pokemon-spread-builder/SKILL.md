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
- **Breakpoint tie-break:** when adjacent EV values on **one axis** produce
  **identical** damage rolls, report the **lower** EV count (e.g. 23 Def over
  24 Def). When several HP/bulk splits share the same minimum total and
  identical satisfying rolls, **do not pick one arbitrarily** — show a short
  sample per **Multiple valid bulk spreads** below.
- **Multiple valid bulk spreads (Procedure A):** when the min-total iso-EV
  curve has more than one `(HP, Def)` or `(HP, SpD)` pair that meets the target:
  - List up to **3** representative min-total splits as bullets; if more exist,
    append `etc.` (or an equivalent brief "and others") — do not dump an
    exhaustive grid.
  - Prefer diversifying the sample across the HP↔bulk tradeoff (higher-HP /
    lower-bulk, mid, lower-HP / higher-bulk) rather than three adjacent points.
  - Always also compute and present an **HP-preferring** spread, even when its
    total EVs exceed the absolute minimum:
    1. Among all valid unlocked `(HP, bulk)` pairs that meet the target, find
       the **minimum bulk EV** (`Def` or `SpD`) that still works for at least
       one HP ≤ format cap.
    2. At that fixed bulk EV, find the **lowest HP** that still meets the
       target.
  - HP-preferring can cost more total EVs than the absolute minimum; note that
    it leaves more raw HP for other matchups.
  - If the HP-preferring pair is already in the min-total sample, do not
    duplicate it in a separate section.
- **Roles:** infer from prompt — "my X needs to live Y from Z" → X = defender;
  "how much SpA does X need to 2HKO Y" → X = attacker, Y = defender with stated
  spread.

### Locked parameters → CLI (hard gate)

Before **any** `pkmn-calc` call in Procedure A or B:

1. Parse each side's spread with `interpret-damage-calc-syntax` (`N Stat` /
   `N+ Stat` / `N- Stat` / named nature / stages / IVs).
2. Write a locked parameter block for each one available: species, move, EVs, nature,
   items, abilities, field.
3. Map `+`/`-` markers on EV counts to flags (EVs in `--*-evs`; nature is a
   **separate** flag — `+`/`-` is never encoded inside the EV string
   alone). Pass `--attacker-nature` / `--defender-nature` **when the nature
   is already known** from notation, an explicit prompt, or a resolved defender
   nature category — omit the flag when nature is neutral or still ambiguous.

| Prompt fragment | Required `pkmn-calc` (attacker side) |
| --- | --- |
| `32 Atk` (no `+`/`-`) | `--attacker-evs "32 Atk"` — omit nature unless specified explicitly |
| `32+ Atk` | `--attacker-evs "32 Atk"` **and** `--attacker-nature` set to an **Attack-boosting** nature |
| `32- Atk` | `--attacker-evs "32 Atk"` **and** `--attacker-nature` set to an **Attack-hindering** nature |

Mirror the same marker→separate-nature-flag rule for defender notation when
Procedure B locks a defended spread.

### Nature category (Procedure A — defender only)

Before searching bulk EVs — and **before any `pkmn-calc` call** — resolve which
**nature category** the **defender** spread should use. This is separate from
the attacker's nature when known from the prompt — when `N+ Stat` / `N- Stat`
notation or a named attacker nature fixes the threat, pass `--attacker-nature`;
otherwise omit it. Do not default to a pure defensive nature for every defender.

Infer **only** from **literal words in the prompt**. A species name alone never
implies a category. Do **not** use typical sets, VGC roles, or "what this mon
usually runs."

| Category | Typical natures | When (prompt must say so) |
| -------- | --------------- | ---- |
| Defensive | Bold/Impish/Calm/Careful (boost the threatened bulk stat) | User asks for bulky / defensive nature |
| Attacking | Adamant/Modest (etc.); avoid dropping the threatened bulk stat when possible | User wants offense preserved, names Modest/Adamant, or says keep SpA/Atk |
| Speed | Jolly/Timid (etc.); same bulk-drop caveat | User cares about outspeeding, speed ties, or names Timid/Jolly |

**Resolve in this order (hard gate):**

1. **Scan the prompt for an explicit cue** — named nature, Spe/speed-tie lock,
   or the words bulky / defensive / offense / keep SpA|Atk /
   Timid / Modest / etc.
2. **If any cue is present**, lock that category (and nature/Spe when stated):
   - "Modest Charizard that lives …" → attacking category, nature fixed Modest.
   - "live Rock Slide … but speed ties with max speed Modest Charizard" →
     attacking/speed category; nature Modest; Spe locked to that benchmark
     (Champions: `32 Spe` for max Spe Modest). Remaining EVs go to HP/Def for
     the survive check.
   - "bulky Incineroar" / "Impish Incineroar" → defensive category (or the
     named nature).
3. **If no cue** (e.g. "What does Charizard need to live Rock Slide from 32 Atk
   Garchomp?" or "what build on Sylveon to live 32+ Atk Dire Claw from
   Sneasler"):
   - **STOP.** Your **next output** is the clarifying question — at most one
     short setup sentence, then the options. **Do not** debate categories,
     compare worked examples, or invent a role for the species.
   - Ask **one** clarifying question: which nature category to optimize under.
     Option labels only — **no** commentary after each choice (no "— preserve
     SpA…", "— maximize bulk…", etc.):
     - Attacking (e.g. Modest)
     - Speed (e.g. Timid)
     - Defensive (e.g. Bold/Calm)
4. **Honor an explicitly stated nature** — do not override it.

Only offer an alternate category if the user asked for options or the primary
category cannot meet the check within EV caps.

---

## Efficiency (keep Procedure A/B fast)

- **Ambiguous defender nature category:** do not load extra skills, plan EV
  grids, or write long reasoning — **ask first**, then continue.
- After nature + target + attacker/defender decoding are resolved, run calcs
  immediately. Do not re-derive format caps, spread-move multipliers, or
  Champions stat math already covered by the prerequisite skills — pass flags
  to `pkmn-calc` and read the verdict.
- Batch a small parallel set of candidate spreads (baseline + suspected
  minimum + ±1 boundary). Prefer a few targeted calls over planning a large
  EV grid in prose.
- For Procedure A multi-spread output: characterize the min-total iso-EV
  curve enough to know whether more than three ties exist (then append `etc.`),
  then compute the HP-preferring Def floor — do not exhaustively list every
  printable split.
- Decode "32 Atk" / `+` / nature markers via `interpret-damage-calc-syntax`
  once; after locking known attacker/defender natures, do not re-debate them —
  pass `--attacker-nature` / `--defender-nature` only where the locked block
  already specifies a nature.

---

## Procedure A — Defender (survive / meet bulk target)

1. **Resolve target outcome** (typically: survive first hit).
2. **Decode attacker** from the prompt per `interpret-damage-calc-syntax` and
   lock attacker CLI params per **Locked parameters → CLI** (notation only — no
   calc yet).
3. **Resolve defender nature category** (prompt-literal cues only — see Nature
   category) and any **EV locks** (Spe / Atk / SpA floors from the prompt).
   **No explicit cue → ask now** (next message = question + label-only
   options; no role debate). Do not proceed to step 4 until answered.
4. **Run initial calc** per `pokemon-damage-calc-cli` with default defender
   spread and the game type specified by the harness or user prompt.
5. **Search defender bulk spreads:** iterate HP × Def (physical) or HP × SpD
   (special) on **unlocked** stats only. Low → high. Search enough to
   characterize the min-total iso-EV curve — not every printable split.
6. **Identify** the least total EV investment meeting the target verdict under
   the resolved nature and locks. Collect representative min-total ties (cap
   presentation at 3 + `etc.` per Shared rules).
7. **Compute** the HP-preferring pair per Shared rules.
8. **Present** in this order:
   - Nature category rationale, any locked EVs, and target outcome.
   - **Minimum total** — up to 3 bullets. When more min-total splits exist,
     show three diverse samples and append `etc.`; omit `etc.` when ≤3 exist.
     Example:
     - 24 HP / 21 Def
     - 18 HP / 27 Def
     - 16 HP / 29 Def
     - etc.
   - **HP-preferring** (when distinct from the min-total set) — one bullet,
     e.g. `- 30 HP / 16 Def`, with a short note that it keeps bulk minimal for
     high-HP survival and drops spare HP EVs that do not change the verdict.
   - Supporting `pkmn-calc --text` desc lines (at least one min-total and the
     HP-preferring). Verify each listed bullet when practical.
   - If the primary category fails within EV caps, say so and offer an alternate
     category only then.

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

Target: **survive first hit**. User: "Impish Incineroar to live 32 Atk Close
Combat from Sneasler." Prompt names **Impish** → defensive category locked; no
ask.

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

Same gate for prompts like "What does Charizard need to live Rock Slide from
32 Atk Garchomp?" or "what build on Sylveon to live 32+ Atk Dire Claw from
Sneasler" — **no** nature/Spe/bulky/offense words → **ask immediately**.

**Correct first response** (no long preamble; do not compare other examples):

> To find the minimum Sylveon spread that survives Sneasler's 32+ Atk Dire Claw
> (Champions, Doubles), which nature category should I optimize under?
> - Attacking (e.g. Modest)
> - Speed (e.g. Timid)
> - Defensive (e.g. Bold/Calm)

**Incorrect:** long internal debate; assume Modest/Bold from "typical Sylveon";
treat A4 as permission to invent Modest; run calcs then ask; stuff tradeoffs
into each option label.

After the user replies (e.g. attacking → Modest), proceed with Procedure A
from step 4.

---

## Worked example A4 — Multiple min spreads + HP-preferring (Champions)

Target: **survive first hit**. User first asked for an Archaludon live check
with no nature cue (ask per A3), then answered **Attacking**. Continue with
**Modest**. User threat: `32+ Atk` Sneasler Close Combat.

Locked attacker: Sneasler, Close Combat, `32 Atk` EVs, **Adamant** (`32+ Atk`).
Because the prompt uses `32+ Atk`, pass `--attacker-nature Adamant`.

Minimum total **45** EVs on HP/Def — several equal splits produce identical
results. Present up to three diverse samples (omit `etc.` here because three
known equals are shown):

- 16 HP / 29 Def
- 18 HP / 27 Def
- 24 HP / 21 Def

When the iso-EV curve has more than three splits, cap the list at three and
append `etc.`

**HP-preferring** (more total EVs than 45, but more raw HP for other hits):

- 30 HP / 16 Def

```bash
pkmn-calc --champions --doubles --text -a Sneasler -d Archaludon -m "Close Combat" \
  --attacker-evs "32 Atk" --attacker-nature Adamant \
  --defender-nature Modest --defender-evs "16 HP / 29 Def"
# → 152-180 (83.9 - 99.4%) -- guaranteed 2HKO

pkmn-calc --champions --doubles --text -a Sneasler -d Archaludon -m "Close Combat" \
  --attacker-evs "32 Atk" --attacker-nature Adamant \
  --defender-nature Modest --defender-evs "30 HP / 16 Def"
# → 164-194 (84.1 - 99.4%) -- guaranteed 2HKO
```

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
