---
name: pokemon-nature-chart
description: >
  Pokemon nature reference for interpreting stat boosts and penalties in
  current games and VGC. Use when reasoning about natures, stat modifiers,
  neutral natures, set optimization, Adamant/Modest/Timid/Jolly, physical vs
  special attackers, speed tiers, or which nature raises/lowers a stat.
---

# Pokemon Nature Chart

Authoritative nature reference for the 25-nature system used in current games
and VGC. Use this to reason about which stat a nature boosts or lowers on a
Pokemon.

## How to read natures

Each Pokemon has one nature. Natures modify two of the five battle stats (Attack,
Defense, Special Attack, Special Defense, Speed).


| Effect       | Modifier                                         |
| ------------ | ------------------------------------------------ |
| Raised stat  | +10%                                             |
| Lowered stat | −10%                                             |
| Neutral      | No change (raised and lowered stat are the same) |


Rules for reasoning:

- **25 total natures:** 20 non-neutral; 5 neutral (see modifier table above).
- **HP is excluded.** No nature raises or lowers HP.
- **One boost, one drop.** Every non-neutral nature increases exactly one stat
and decreases exactly one other stat.
- Nature is a flat multiplier applied on top of base stats, EVs, and IVs. It is
separate from items, abilities, and field conditions.

Stat abbreviations used below: **Atk** Attack, **Def** Defense, **SpA** Special
Attack, **SpD** Special Defense, **Spe** Speed.

---

## Natures alphabetically

Find a nature by name and see which stats it raises and lowers.


| Nature  | Increases | Decreases         |
| ------- | --------- | ----------------- |
| Adamant | Attack    | Sp. Atk           |
| Bashful | Sp. Atk   | Sp. Atk (neutral) |
| Bold    | Defense   | Attack            |
| Brave   | Attack    | Speed             |
| Calm    | Sp. Def   | Attack            |
| Careful | Sp. Def   | Sp. Atk           |
| Docile  | Defense   | Defense (neutral) |
| Gentle  | Sp. Def   | Defense           |
| Hardy   | Attack    | Attack (neutral)  |
| Hasty   | Speed     | Defense           |
| Impish  | Defense   | Sp. Atk           |
| Jolly   | Speed     | Sp. Atk           |
| Lax     | Defense   | Sp. Def           |
| Lonely  | Attack    | Defense           |
| Mild    | Sp. Atk   | Defense           |
| Modest  | Sp. Atk   | Attack            |
| Naive   | Speed     | Sp. Def           |
| Naughty | Attack    | Sp. Def           |
| Quiet   | Sp. Atk   | Speed             |
| Quirky  | Sp. Def   | Sp. Def (neutral) |
| Rash    | Sp. Atk   | Sp. Def           |
| Relaxed | Defense   | Speed             |
| Sassy   | Sp. Def   | Speed             |
| Serious | Speed     | Speed (neutral)   |
| Timid   | Speed     | Attack            |


---

## Natures by stat grid

Rows = stat raised (+10%). Columns = stat lowered (−10%). Cell = nature name.
Diagonal cells are the five neutral natures.


| Raised \ Lowered | Atk    | Def    | SpA     | SpD     | Spe     |
| ---------------- | ------ | ------ | ------- | ------- | ------- |
| **+Atk**         | Hardy  | Lonely | Adamant | Naughty | Brave   |
| **+Def**         | Bold   | Docile | Impish  | Lax     | Relaxed |
| **+SpA**         | Modest | Mild   | Bashful | Rash    | Quiet   |
| **+SpD**         | Calm   | Gentle | Careful | Quirky  | Sassy   |
| **+Spe**         | Timid  | Hasty  | Jolly   | Naive   | Serious |


Use this grid when you know which stat to boost and which to drop. Example: a
physical attacker that does not use special moves → **Adamant** (+Atk row, SpA
column).
