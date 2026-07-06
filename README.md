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

## Install all at once

```bash
npx skills add jpbullalayao/pokemon-ai
```
