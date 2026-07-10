---
name: pokemon-champions
description: >
  Pokemon Champions competitive domain reference. Use when working with
  Pokemon Champions, VGC roster legality, Champions items, mega evolution,
  or ranked/event format questions. Authoritative roster and item lists
  bundled in docs/ within this skill.
---

# Pokemon Champions

Domain reference for Pokemon Champions — the competitive platform for official
VGC events on Nintendo Switch and mobile. Battles are Doubles format at Level 50.

---

## Regulation sets

### Regulation M-A

The first Champions ruleset. Legal Pokemon are listed in
[docs/regulation-m-a.md](docs/regulation-m-a.md).

### Regulation M-B

Extends Regulation M-A. Pokemon newly legal in M-B are listed in
[docs/regulation-m-b.md](docs/regulation-m-b.md)
(38 additions, including Mega Raichu X and Mega Raichu Y).

M-B legal roster = all entries in Regulation M-A **plus** all entries in the
M-B additions file. The M-B file is a **delta**, not a standalone roster.

---



## Reference files


| File                                                               | Contents                                        |
| ------------------------------------------------------------------ | ----------------------------------------------- |
| [docs/pokemon.md](docs/pokemon.md)               | Full Champions roster                           |
| [docs/regulation-m-a.md](docs/regulation-m-a.md) | M-A legal Pokemon                               |
| [docs/regulation-m-b.md](docs/regulation-m-b.md) | M-B legal pokemon                               |
| [docs/items.md](docs/items.md)                   | All items                                       |


The full roster in `pokemon.md` equals the combined Regulation M-A and M-B
legal lists (M-A ∪ M-B).

---



## Table conventions

Pokemon and item reference files share a common column format:


| Column               | Meaning                                                                                          |
| -------------------- | ------------------------------------------------------------------------------------------------ |
| **Name**             | Display name, including form variants (e.g. "Mega Charizard X", "Paldean Tauros (Combat Breed)") |
| **pokeapi-cli name** | Canonical slug for `pkmn pokemon <slug>` or `pkmn item <slug>`                                   |

[`items.md`](docs/items.md) adds columns for hold items and berries; mega stones get **Shop VP** only:


| Column      | Meaning                                                                                       |
| ----------- | --------------------------------------------------------------------------------------------- |
| **Shop VP** | Victory Points to buy from the Frontier Shop; blank if not shop-purchasable                   |
| **Type**    | Held-item category (e.g. Power Boost, Defense, Recovery); hold items and berries only   |




