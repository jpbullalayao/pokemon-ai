# pokemon-ai

Reference docs and agent skills for Pokemon and VGC workflows. Each skill is individually installable via the [skills.sh](https://skills.sh) CLI.

## Reference docs

- [general/nature-chart.md](general/nature-chart.md) — nature stat modifiers
- [general/type-chart.md](general/type-chart.md) — type matchups
- [general/move-priority-list.md](general/move-priority-list.md) — move priority brackets
- [vgc/champions/](vgc/champions/) — Pokemon Champions meta references

## Skills

### `pokemon-nature-chart`

Authoritative reference for the 25-nature system used in current games and VGC. Use when reasoning about which stat a nature boosts or lowers, picking natures for competitive sets, or interpreting Adamant, Modest, Timid, Jolly, and other common natures.

```bash
npx skills add jpbullalayao/pokemon-ai --skill pokemon-nature-chart
```



### `pokemon-type-chart`

Authoritative type matchup reference for the 18-type system. Use when reasoning about super-effective hits, resistances, immunities, dual-type damage, or team-building defensive profiles.

```bash
npx skills add jpbullalayao/pokemon-ai --skill pokemon-type-chart
```



### `pokemon-move-priority-list`

Authoritative move priority reference for Generation IX turn order. Use when reasoning about priority brackets, which move goes first, Fake Out, Sucker Punch, Extreme Speed, Protect, Trick Room, or speed vs priority in VGC.

```bash
npx skills add jpbullalayao/pokemon-ai --skill pokemon-move-priority-list
```



### `pokemon-champions`

Domain reference for Pokemon Champions competitive play — regulation rulesets, roster legality, and item availability. Authoritative lists in [vgc/champions/](vgc/champions/).

```bash
npx skills add jpbullalayao/pokemon-ai --skill pokemon-champions
```



### `interpret-damage-calc-syntax`

Reference for reading Pokemon damage calculator notation — EV spreads, nature markers, stat stages, field modifiers, damage ranges, and KO verdicts. Use when interpreting calc strings like `156+ Atk` or full Showdown-style output lines.

```bash
npx skills add jpbullalayao/pokemon-ai --skill interpret-damage-calc-syntax
```



### `pokemon-spread-builder`

Procedure for finding minimum-EV spreads in damage calc matchups — defender bulk to survive hits and attacker investment to meet a user-specified KO outcome (OHKO, 2HKO, 3HKO, etc.). Use when optimizing builds for specific calc matchups.

```bash
npx skills add jpbullalayao/pokemon-ai --skill pokemon-spread-builder
```



## Install all at once

```bash
npx skills add jpbullalayao/pokemon-ai
```

