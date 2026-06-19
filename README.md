# pokemon-ai

Reference docs and agent skills for Pokemon and VGC workflows. Each skill is individually installable via the [skills.sh](https://skills.sh) CLI.

## Reference docs

- [general/nature-chart.md](general/nature-chart.md) — nature stat modifiers
- [general/type-chart.md](general/type-chart.md) — type matchups (Gen 6+)
- [vgc/champions/](vgc/champions/) — Pokemon Champions meta references

## Skills

### `pokemon-nature-chart`

Authoritative reference for the 25-nature system used in current games and VGC. Use when reasoning about which stat a nature boosts or lowers, picking natures for competitive sets, or interpreting Adamant, Modest, Timid, Jolly, and other common natures.

```bash
npx skills add jpbullalayao/pokemon-ai --skill pokemon-nature-chart
```

## Install all at once

```bash
npx skills add jpbullalayao/pokemon-ai
```
