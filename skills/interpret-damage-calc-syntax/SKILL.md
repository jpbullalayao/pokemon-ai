---
name: interpret-damage-calc-syntax
description: >
  Reference for interpreting Pokemon damage calculator syntax — EV
  spreads, IV counts, nature markers (+/−), stat stages (+1 to +6, −1 to
  −6), field modifiers, items, abilities, damage ranges, HP percentages, and
  KO verdicts (OHKO, 2HKO, etc.). Use when reading or explaining damage calc
  strings, Showdown calc output, spread notation like "156+ Atk" or "0 Atk 0
  IVs", or full lines such as "252+ SpA Choice Specs ... vs. ... guaranteed
  OHKO".
---

# Interpret Damage Calc Syntax

Reference for reading Pokemon damage calculator notation — the compact strings
produced by tools like Pokemon Showdown's calculator and similar VGC damage
calcs. Use this to decode spreads, modifiers, and KO conclusions without
misinterpreting EV counts, IVs, natures, or field conditions.

---

## EV, IV, and nature notation

Spreads prefix a stat abbreviation with an EV count and optional nature marker.
IV counts appear when a calc uses a **non-default** IV for that stat.


| Syntax | Meaning |
| ------ | ------- |
| `32 Atk` | 32 EVs in Attack (neutral nature for Atk) |
| `32+ Atk` | 32 EVs in Attack with an Attack-boosting nature |
| `252- SpA` | 252 EVs in Special Attack with a SpA-reducing nature |
| `4 HP / 0 Def` | 4 EVs in HP and 0 EVs in Defense (slash separates multiple stats) |
| `0 Atk 0 IVs` | 0 Attack EVs **and** 0 IVs in Attack (neutral nature) |
| `252 HP 31 IVs` | 252 HP EVs with 31 IVs in HP |

**Nature markers**

- `+` after the EV count → boosting nature for that stat
- `−` after the EV count → hindering nature for that stat
- No marker → neutral nature for that stat (no +/− on that stat)

**Stat abbreviations** (full names also appear in calcs)

| Abbr | Stat |
| ---- | ---- |
| HP | HP |
| Atk | Attack |
| Def | Defense |
| SpA | Special Attack |
| SpD | Special Defense |
| Spe | Speed |

Same patterns apply to every stat category: `def`, `defense`, `special attack`,
`spa`, `spd`, `speed`, etc.

**IV notation**

- `N IVs` immediately after a stat spread → **exactly N IVs** in that stat
  (range **0–31** in most games)
- Omitted IV clause → calculator default (typically **31** in all stats unless
  the tool states otherwise)
- IVs only appear in the string when they differ from the calc's default —
  their presence is always meaningful
- Applies to attacker and defender spreads alike

---

## Stat stage modifiers

Stages appear as a signed integer **before** the spread, only when non-neutral:


| Syntax | Meaning |
| ------ | ------- |
| `+1 156+ Atk` | 156 EVs, Attack-boosting nature, **+1 Attack stage** |
| `+6 252+ SpA` | +6 SpA stage (maximum boost) |
| `−2 0 Atk` | −2 Attack stage (stat reduced) |

- Range: **+1 through +6** (boosts) and **−1 through −6** (drops)
- **Neutral stages (0) are omitted** — if no `+N` / `−N` prefix, assume no stat-stage change for that calc

Stages stack with nature, items, abilities, and field modifiers in the calc.

---

## Full calc line structure

A complete damage calc string follows this general shape:

```
[stage?] [spread] [IVs?] [held item?] [ability?] [attacker] [move] vs. [spread] [IVs?] [held item?] [defender] [field modifiers?]: [damage] ([% HP]) -- [KO verdict]
```

Parse each segment left to right. Omitted segments are not active in that calc.

**Ordering:** After the spread (and optional `N IVs`), **held item** and **ability**
names appear **before** the Pokemon name, in that order. Both can be present in
the same calc — e.g. `Life Orb Beads of Ruin Calyrex-Shadow` means the attacker
holds Life Orb **and** a teammate's Beads of Ruin aura is active. The move name
always immediately precedes `vs.` (or ends the attacker segment).

### Attacker segment

Everything before `vs.` describes the attacking Pokemon and move:

- **Spread** — EV/nature (and optional stage prefix); **`N IVs`** when non-default
- **Held item** — e.g. `Choice Specs`, `Life Orb` (attacker's held item; appears
  before the Pokemon name and affects damage)
- **Ability** — attacker's own ability (e.g. `Hadron Engine`) **or** a **field
  ability** from another Pokemon on the field (e.g. `Sword of Ruin`, `Beads of
  Ruin`); item and field ability can both appear in one calc
- **Pokemon name** — e.g. `Miraidon`, `Calyrex-Shadow`, `Urshifu-Rapid-Strike`
- **Move** — e.g. `Electro Drift`, `Astral Barrage`, `Surging Strikes (3 hits)`

### Defender segment

After `vs.`:

- **Spread** — defender EV/IV investment, same notation as attacker
- **Held item** — e.g. `Assault Vest` (defender's held item; appears before the
  Pokemon name when relevant to the calc)
- **Pokemon name** — target of the attack

### Field and battle modifiers

Phrases after the defender name describe active conditions affecting damage:


| Example | Meaning |
| ------- | ------- |
| `in Electric Terrain` | Electric Terrain is active; boosts Electric moves |
| `in Rain` | Rain is active |
| `through Reflect` | Reflect is active on the defender's side (physical damage reduction) |
| `on a critical hit` | Calc assumes a critical hit |
| `Sword of Ruin` | Chien-Pao (or another Sword of Ruin user) is on the field; lowers foe Defense |
| `Beads of Ruin` | Chi-Yu (or another Beads of Ruin user) is on the field; lowers foe Sp. Def |

Field abilities from **teammates** (Ruin abilities, etc.) appear in the
attacker segment (before the attacker's name) even though they belong to a different
Pokemon — their presence means that Pokemon is assumed on the field and the aura
is active. They stack with the attacker's own held item in the same string.

Other common modifiers: weather (`Sun`, `Sand`), terrains, screens (`Light Screen`),
auras, and similar — treat any such phrase as an **active condition** assumed in
the calc.

### Damage results

After the colon:

- **`222-262`** — raw damage range dealt to the defender (min–max roll)
- **`(132.9 - 156.8%)`** — same rolls expressed as **% of defender's max HP**
- **`--`** — separator before the KO conclusion

### KO verdict

The trailing phrase summarizes survivability:


| Verdict | Meaning |
| ------- | ------- |
| `guaranteed OHKO` | Defender **never** survives; move always one-shots at these parameters |
| `guaranteed 2HKO` | Two hits always KO; defender cannot survive two hits |
| `5.1% chance to 3HKO` | Probability-based — here, 5.1% chance a third hit is needed |
| (similar patterns) | `X% chance to NHKO`, `possible OHKO`, etc. — read literally |

For non-OHKO lines, the verdict describes how many hits are needed and any
roll-dependent odds.

---

## Worked examples

### Example 1 — full calc with terrain, ability, and OHKO

```
252+ SpA Choice Specs Hadron Engine Miraidon Electro Drift vs. 0 HP / 0 SpD Zacian-Crowned in Electric Terrain: 222-262 (132.9 - 156.8%) -- guaranteed OHKO
```

| Segment | Reading |
| ------- | ------- |
| `252+ SpA` | 252 SpA EVs, SpA-boosting nature |
| `Choice Specs` | Held item (+50% SpA) |
| `Hadron Engine` | Miraidon's ability; active in calc |
| `Electro Drift` | Move used |
| `0 HP / 0 SpD` | Defender has 0 EVs in HP and SpD |
| `in Electric Terrain` | Terrain active; boosts Electric |
| `222-262` | Damage roll range |
| `132.9 - 156.8%` | % of Zacian-Crowned's HP |
| `guaranteed OHKO` | Always one-shots |

### Example 2 — stat stage + physical OHKO

```
+1 156+ Atk Zacian-Crowned Play Rough vs. 4 HP / 0 Def Miraidon: 266-314 (151.1 - 178.4%) -- guaranteed OHKO
```

`+1` = one Attack stage up. `156+ Atk` = 156 Attack EVs with boosting nature.

### Example 3 — multi-hit move, weather, crit

```
156+ Atk Urshifu-Rapid-Strike Surging Strikes (3 hits) vs. 252 HP / 0 Def Zacian-Crowned in Rain on a critical hit: 138-165 (69.3 - 82.9%) -- guaranteed 2HKO
```

`(3 hits)` = Surging Strikes hits three times (total damage shown). `in Rain`
and `on a critical hit` are both assumed active.

### Example 4 — field ability from teammate, guaranteed 2HKO

```
116+ Atk Sword of Ruin Rillaboom Grassy Glide vs. 4 HP / 4 Def Urshifu-Rapid-Strike: 120-144 (68.1 - 81.8%) -- guaranteed 2HKO
```

`Sword of Ruin` is **not** Rillaboom's ability or held item — it means a Pokemon
with Sword of Ruin (typically **Chien-Pao**) is on the field at the same time,
and its aura (lowering foe Defense) is included in the calc.

### Example 5 — screen modifier

```
0 Atk Incineroar Flare Blitz vs. 252 HP / 4 Def Rillaboom through Reflect: 112-132 (54.1 - 63.7%) -- guaranteed 2HKO
```

`0 Atk` = no Attack EVs, neutral nature. `through Reflect` = Reflect reduces
physical damage.

### Example 6 — explicit IV count

```
0 Atk 0 IVs Incineroar Flare Blitz vs. 252 HP / 4 Def Rillaboom through Reflect: 100-117 (48.3 - 56.5%) -- 84.4% chance to 2HKO
```

| Segment | Reading |
| ------- | ------- |
| `0 Atk 0 IVs` | 0 Attack EVs, **0 IVs in Attack** (minimum Atk stat) |
| `through Reflect` | Reflect active |
| `84.4% chance to 2HKO` | Not guaranteed — 84.4% of damage rolls allow a 2HKO |

Compare to Example 5 (same matchup, default IVs): higher damage and
`guaranteed 2HKO`. Lower Attack IV materially weakens the calc.

### Example 7 — held item + field ability + defender item

```
252 SpA 30 IVs Life Orb Beads of Ruin Calyrex-Shadow Astral Barrage vs. 252 HP / 4 SpD Assault Vest Rillaboom: 140-165 (67.6 - 79.7%) -- guaranteed 2HKO
```

| Segment | Reading |
| ------- | ------- |
| `252 SpA 30 IVs` | 252 SpA EVs, neutral nature, **30 IVs in SpA** (non-default) |
| `Life Orb` | Calyrex-Shadow's held item (+30% damage, recoil not shown) |
| `Beads of Ruin` | Chi-Yu (or similar) on the field; lowers foe Sp. Def |
| `Calyrex-Shadow` | Attacker |
| `Astral Barrage` | Move used |
| `Assault Vest` | Rillaboom's held item (+50% SpD; affects damage taken) |
| `guaranteed 2HKO` | Two hits always KO |

Item and field ability are **not** mutually exclusive — read each name between
the spread and the Pokemon name in order (item first, then field ability).

---

## Quick parsing checklist

When interpreting any damage calc string:

1. **Split on `vs.`** — attacker left, defender right
2. **Check for stage prefix** (`+N` / `−N`) before attacker spread
3. **Parse EV/nature** on both sides (`N+ Stat`, `N- Stat`, `N Stat`)
4. **Check for `N IVs`** after a stat — non-default IV for that stat (0–31)
5. **Parse names between spread and Pokemon** — in order: held item (attacker's),
   then field ability (teammate's); both may be present; defender may also have
   a held item before its name (e.g. `Assault Vest Rillaboom`)
6. **Read field phrases** after defender — all are assumed active
7. **Read damage range and %** after `:`
8. **Read KO verdict** after `--` for survivability conclusion
