# Pokemon Type Chart (Gen 6+ / Scarlet-Violet)

Authoritative type matchup reference for the 18-type system used in current games
and VGC. Use this to reason about super-effective hits, resistances, and
immunities. For exact damage numbers (rolls, OHKO/2HKO), use the damage
calculator tool instead; this file covers type effectiveness only.

## How to read effectiveness

Multipliers apply to a move's damage based on the defender's type(s):

| Value | Meaning           | Damage |
| ----- | ----------------- | ------ |
| `2`   | Super effective   | 200%   |
| `1`   | Neutral           | 100%   |
| `½`   | Not very effective| 50%    |
| `0`   | No effect (immune)| 0%     |

Rules for reasoning:

- Dual-type defenders multiply the two values together. Example: a Ground move vs
  a Water/Rock target = `2 x 2 = 4x` (quad damage). A Grass move vs a Fire/Flying
  target = `½ x ½ = ¼x`.
- Any `0` against either of the defender's types makes the whole hit `0` (immunity
  wins). Example: Electric vs Ground/Flying = `0`.
- STAB (Same-Type Attack Bonus, 1.5x when the attacker shares the move's type) and
  Terastallization are separate multipliers, not part of this chart.

---

## Offensive summary (what each attacking type hits)

For each type: what its moves are strong/weak against. "—" means none.

- **Normal** — Super effective: — · Resisted by: Rock, Steel · No effect: Ghost
- **Fire** — Super effective: Grass, Ice, Bug, Steel · Resisted by: Fire, Water, Rock, Dragon
- **Water** — Super effective: Fire, Ground, Rock · Resisted by: Water, Grass, Dragon
- **Electric** — Super effective: Water, Flying · Resisted by: Electric, Grass, Dragon · No effect: Ground
- **Grass** — Super effective: Water, Ground, Rock · Resisted by: Fire, Grass, Poison, Flying, Bug, Dragon, Steel
- **Ice** — Super effective: Grass, Ground, Flying, Dragon · Resisted by: Fire, Water, Ice, Steel
- **Fighting** — Super effective: Normal, Ice, Rock, Dark, Steel · Resisted by: Poison, Flying, Psychic, Bug, Fairy · No effect: Ghost
- **Poison** — Super effective: Grass, Fairy · Resisted by: Poison, Ground, Rock, Ghost · No effect: Steel
- **Ground** — Super effective: Fire, Electric, Poison, Rock, Steel · Resisted by: Grass, Bug · No effect: Flying
- **Flying** — Super effective: Grass, Fighting, Bug · Resisted by: Electric, Rock, Steel
- **Psychic** — Super effective: Fighting, Poison · Resisted by: Psychic, Steel · No effect: Dark
- **Bug** — Super effective: Grass, Psychic, Dark · Resisted by: Fire, Fighting, Poison, Flying, Ghost, Steel, Fairy
- **Rock** — Super effective: Fire, Ice, Flying, Bug · Resisted by: Fighting, Ground, Steel
- **Ghost** — Super effective: Psychic, Ghost · Resisted by: Dark · No effect: Normal
- **Dragon** — Super effective: Dragon · Resisted by: Steel · No effect: Fairy
- **Dark** — Super effective: Psychic, Ghost · Resisted by: Fighting, Dark, Fairy
- **Steel** — Super effective: Ice, Rock, Fairy · Resisted by: Fire, Water, Electric, Steel
- **Fairy** — Super effective: Fighting, Dragon, Dark · Resisted by: Fire, Poison, Steel

---

## Defensive summary (what each type takes when hit)

For each type: what threatens it. This is the primary lens for team building.
"—" means none.

- **Normal** — Weak to: Fighting · Resists: — · Immune to: Ghost
- **Fire** — Weak to: Water, Ground, Rock · Resists: Fire, Grass, Ice, Bug, Steel, Fairy · Immune to: —
- **Water** — Weak to: Electric, Grass · Resists: Fire, Water, Ice, Steel · Immune to: —
- **Electric** — Weak to: Ground · Resists: Electric, Flying, Steel · Immune to: —
- **Grass** — Weak to: Fire, Ice, Poison, Flying, Bug · Resists: Water, Electric, Grass, Ground · Immune to: —
- **Ice** — Weak to: Fire, Fighting, Rock, Steel · Resists: Ice · Immune to: —
- **Fighting** — Weak to: Flying, Psychic, Fairy · Resists: Bug, Rock, Dark · Immune to: —
- **Poison** — Weak to: Ground, Psychic · Resists: Grass, Fighting, Poison, Bug, Fairy · Immune to: —
- **Ground** — Weak to: Water, Grass, Ice · Resists: Poison, Rock · Immune to: Electric
- **Flying** — Weak to: Electric, Ice, Rock · Resists: Grass, Fighting, Bug · Immune to: Ground
- **Psychic** — Weak to: Bug, Ghost, Dark · Resists: Fighting, Psychic · Immune to: —
- **Bug** — Weak to: Fire, Flying, Rock · Resists: Grass, Fighting, Ground · Immune to: —
- **Rock** — Weak to: Water, Grass, Fighting, Ground, Steel · Resists: Normal, Fire, Poison, Flying · Immune to: —
- **Ghost** — Weak to: Ghost, Dark · Resists: Poison, Bug · Immune to: Normal, Fighting
- **Dragon** — Weak to: Ice, Dragon, Fairy · Resists: Fire, Water, Electric, Grass · Immune to: —
- **Dark** — Weak to: Fighting, Bug, Fairy · Resists: Ghost, Dark · Immune to: Psychic
- **Steel** — Weak to: Fire, Fighting, Ground · Resists: Normal, Grass, Ice, Flying, Psychic, Bug, Rock, Dragon, Steel, Fairy · Immune to: Poison
- **Fairy** — Weak to: Poison, Steel · Resists: Fighting, Bug, Dark · Immune to: Dragon

---

## Full lookup matrix

Rows = attacking type, columns = defending type. Cell = damage multiplier of the
attack against that single defending type. Blank-looking cells are `1` (neutral).

Column abbreviations: Nor Fir Wat Ele Gra Ice Fig Poi Gro Fly Psy Bug Roc Gho Dra Dar Ste Fai

| ATK \ DEF | Nor | Fir | Wat | Ele | Gra | Ice | Fig | Poi | Gro | Fly | Psy | Bug | Roc | Gho | Dra | Dar | Ste | Fai |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Normal    | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | ½   | 0   | 1   | 1   | ½   | 1   |
| Fire      | 1   | ½   | ½   | 1   | 2   | 2   | 1   | 1   | 1   | 1   | 1   | 2   | 1   | 1   | ½   | 1   | 2   | 1   |
| Water     | 1   | 2   | ½   | 1   | ½   | 1   | 1   | 1   | 2   | 1   | 1   | 1   | 2   | 1   | ½   | 1   | 1   | 1   |
| Electric  | 1   | 1   | 2   | ½   | ½   | 1   | 1   | 1   | 0   | 2   | 1   | 1   | 1   | 1   | ½   | 1   | 1   | 1   |
| Grass     | 1   | ½   | 2   | 1   | ½   | 1   | 1   | ½   | 2   | ½   | 1   | ½   | 2   | 1   | ½   | 1   | ½   | 1   |
| Ice       | 1   | ½   | ½   | 1   | 2   | ½   | 1   | 1   | 2   | 2   | 1   | 1   | 1   | 1   | 2   | 1   | ½   | 1   |
| Fighting  | 2   | 1   | 1   | 1   | 1   | 2   | 1   | ½   | 1   | ½   | ½   | ½   | 2   | 0   | 1   | 2   | 2   | ½   |
| Poison    | 1   | 1   | 1   | 1   | 2   | 1   | 1   | ½   | ½   | 1   | 1   | 1   | ½   | ½   | 1   | 1   | 0   | 2   |
| Ground    | 1   | 2   | 1   | 2   | ½   | 1   | 1   | 2   | 1   | 0   | 1   | ½   | 2   | 1   | 1   | 1   | 2   | 1   |
| Flying    | 1   | 1   | 1   | ½   | 2   | 1   | 2   | 1   | 1   | 1   | 1   | 2   | ½   | 1   | 1   | 1   | ½   | 1   |
| Psychic   | 1   | 1   | 1   | 1   | 1   | 1   | 2   | 2   | 1   | 1   | ½   | 1   | 1   | 1   | 1   | 0   | ½   | 1   |
| Bug       | 1   | ½   | 1   | 1   | 2   | 1   | ½   | ½   | 1   | ½   | 2   | 1   | 1   | ½   | 1   | 2   | ½   | ½   |
| Rock      | 1   | 2   | 1   | 1   | 1   | 2   | ½   | 1   | ½   | 2   | 1   | 2   | 1   | 1   | 1   | 1   | ½   | 1   |
| Ghost     | 0   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 2   | 1   | 1   | 2   | 1   | ½   | 1   | 1   |
| Dragon    | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 2   | 1   | ½   | 0   |
| Dark      | 1   | 1   | 1   | 1   | 1   | 1   | ½   | 1   | 1   | 1   | 2   | 1   | 1   | 2   | 1   | ½   | 1   | ½   |
| Steel     | 1   | ½   | ½   | ½   | 1   | 2   | 1   | 1   | 1   | 1   | 1   | 1   | 2   | 1   | 1   | 1   | ½   | 2   |
| Fairy     | 1   | ½   | 1   | 1   | 1   | 1   | 2   | ½   | 1   | 1   | 1   | 1   | 1   | 1   | 2   | 2   | ½   | 1   |

---

## Quick reference notes

- **Most resistant defending type:** Steel (resists 10 types, immune to Poison).
- **No-damage immunities:** Normal->Ghost, Fighting->Ghost, Ghost->Normal,
  Electric->Ground, Ground->Flying, Psychic->Dark, Poison->Steel, Dragon->Fairy.
- **Only types with no resistances on defense:** Normal (its sole interaction is the
  Ghost immunity).
- **Common VGC offensive coverage:** Fighting + Flying, Ground + Ice, and Fairy +
  Steel/Fire pairings hit most of the chart for super-effective damage. Validate
  specific matchups against the matrix above before committing to a calc.
