# Pokemon Nature Chart (Gen 3+ / Scarlet-Violet)

Authoritative nature reference for the 25-nature system used in current games
and VGC. Use this to reason about which stat a nature boosts or lowers on a
Pokemon. For exact final stats (EVs, IVs, level, damage rolls), use the damage
calculator tool instead; this file covers nature modifiers only.

## How to read natures

Each Pokemon has one nature. Natures modify two of the five battle stats (Attack,
Defense, Special Attack, Special Defense, Speed). HP is never affected.

| Effect | Modifier |
| ------ | -------- |
| Raised stat | +10% |
| Lowered stat | −10% |
| Neutral | No change (raised and lowered stat are the same) |

Rules for reasoning:

- **25 total natures:** 20 that raise one stat and lower a different stat; 5
  neutral natures that raise and lower the same stat (no net effect).
- **HP is excluded.** No nature raises or lowers HP.
- **One boost, one drop.** Every non-neutral nature increases exactly one stat
  and decreases exactly one other stat.
- Nature is a flat multiplier applied on top of base stats, EVs, and IVs. It is
  separate from items, abilities, and field conditions.

Stat abbreviations used below: **Atk** Attack, **Def** Defense, **SpA** Special
Attack, **SpD** Special Defense, **Spe** Speed.

---

## Natures alphabetically

Primary lookup: find a nature by name and see which stats it raises and lowers.
Neutral natures raise and lower the same stat (marked as neutral).

| Nature | Increases | Decreases |
| ------ | --------- | --------- |
| Adamant | Attack | Sp. Atk |
| Bashful | Sp. Atk | Sp. Atk (neutral) |
| Bold | Defense | Attack |
| Brave | Attack | Speed |
| Calm | Sp. Def | Attack |
| Careful | Sp. Def | Sp. Atk |
| Docile | Defense | Defense (neutral) |
| Gentle | Sp. Def | Defense |
| Hardy | Attack | Attack (neutral) |
| Hasty | Speed | Defense |
| Impish | Defense | Sp. Atk |
| Jolly | Speed | Sp. Atk |
| Lax | Defense | Sp. Def |
| Lonely | Attack | Defense |
| Mild | Sp. Atk | Defense |
| Modest | Sp. Atk | Attack |
| Naive | Speed | Sp. Def |
| Naughty | Attack | Sp. Def |
| Quiet | Sp. Atk | Speed |
| Quirky | Sp. Def | Sp. Def (neutral) |
| Rash | Sp. Atk | Sp. Def |
| Relaxed | Defense | Speed |
| Sassy | Sp. Def | Speed |
| Serious | Speed | Speed (neutral) |
| Timid | Speed | Attack |

---

## Natures by stat grid

Rows = stat raised (+10%). Columns = stat lowered (−10%). Cell = nature name.
Diagonal cells are the five neutral natures (same stat raised and lowered).

| Raised \ Lowered | Atk | Def | SpA | SpD | Spe |
| ---------------- | --- | --- | --- | --- | --- |
| **+Atk** | Hardy | Lonely | Adamant | Naughty | Brave |
| **+Def** | Bold | Docile | Impish | Lax | Relaxed |
| **+SpA** | Modest | Mild | Bashful | Rash | Quiet |
| **+SpD** | Calm | Gentle | Careful | Quirky | Sassy |
| **+Spe** | Timid | Hasty | Jolly | Naive | Serious |

Use this grid when you know which stat to boost and which to drop. Example: a
physical attacker that must outspeed opponents wants +Atk and −SpA → **Adamant**.

---

## Beneficial natures (raises this stat)

For each stat: natures that give +10% to that stat.

- **Attack** — Adamant, Brave, Lonely, Naughty, Hardy (neutral)
- **Defense** — Bold, Impish, Lax, Relaxed, Docile (neutral)
- **Sp. Atk** — Modest, Mild, Quiet, Rash, Bashful (neutral)
- **Sp. Def** — Calm, Careful, Gentle, Sassy, Quirky (neutral)
- **Speed** — Timid, Jolly, Hasty, Naive, Serious (neutral)

---

## Hindering natures (lowers this stat)

For each stat: natures that give −10% to that stat. Avoid these on spreads that
rely on the listed stat.

- **Attack** — Bold, Calm, Modest, Timid
- **Defense** — Lonely, Gentle, Hasty, Mild
- **Sp. Atk** — Adamant, Careful, Impish, Jolly
- **Sp. Def** — Lax, Naughty, Naive, Rash
- **Speed** — Brave, Quiet, Relaxed, Sassy

---

## Quick reference notes

- **Neutral natures (no stat change):** Hardy, Docile, Bashful, Quirky, Serious.
- **HP:** never modified by nature.
- **Physical attackers:** prefer Adamant (+Atk, −SpA) or Jolly (+Spe, −SpA) when
  speed matters; avoid natures that lower Attack.
- **Special attackers:** prefer Modest (+SpA, −Atk) or Timid (+Spe, −Atk); avoid
  natures that lower Sp. Atk.
- **Defensive pivots / support:** Calm (+SpD, −Atk), Careful (+SpD, −SpA), Bold
  (+Def, −Atk), and Impish (+Def, −SpA) are common when the Pokemon does not
  need the lowered stat.
- **Mixed or priority-sensitive sets:** verify the lowered stat is unused before
  committing; e.g. Naive (+Spe, −SpD) trades bulk for speed on mixed attackers.
