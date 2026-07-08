---
name: pokemon-champions
description: >
  Pokemon Champions competitive domain reference. Use when working with
  Pokemon Champions, VGC roster legality, Champions items, mega evolution,
  or ranked/event format questions. Authoritative roster and item lists
  live in vgc/champions/.
---

# Pokemon Champions

Domain reference for Pokemon Champions — the competitive platform for official
VGC events on Nintendo Switch and mobile. Battles are Doubles format at Level 50.

---

## Regulation sets

### Regulation M-A

The first Champions ruleset. Legal Pokemon are listed in
[vgc/champions/regulation-m-a.md](vgc/champions/regulation-m-a.md).

### Regulation M-B

Extends Regulation M-A. Pokemon newly legal in M-B are listed in
[vgc/champions/regulation-m-b.md](vgc/champions/regulation-m-b.md)
(38 additions, including Mega Raichu X and Mega Raichu Y).

M-B legal roster = all entries in Regulation M-A **plus** all entries in the
M-B additions file. The M-B file is a **delta**, not a standalone roster.

---



## Reference files


| File                                                               | Contents                                        |
| ------------------------------------------------------------------ | ----------------------------------------------- |
| [vgc/champions/pokemon.md](vgc/champions/pokemon.md)               | Full Champions roster                           |
| [vgc/champions/regulation-m-a.md](vgc/champions/regulation-m-a.md) | M-A legal Pokemon                               |
| [vgc/champions/regulation-m-b.md](vgc/champions/regulation-m-b.md) | M-B legal pokemon                               |
| [vgc/champions/items.md](vgc/champions/items.md)                   | All items                                       |


The full roster in `pokemon.md` equals the combined Regulation M-A and M-B
legal lists (M-A ∪ M-B).

---



## Table conventions

Pokemon and item reference files share a common column format:


| Column               | Meaning                                                                                          |
| -------------------- | ------------------------------------------------------------------------------------------------ |
| **Name**             | Display name, including form variants (e.g. "Mega Charizard X", "Paldean Tauros (Combat Breed)") |
| **pokeapi-cli name** | Canonical slug for `pkmn pokemon <slug>` or `pkmn item <slug>`                                   |

[`items.md`](vgc/champions/items.md) adds columns for hold items and berries; mega stones get **Shop VP** only:


| Column      | Meaning                                                                                       |
| ----------- | --------------------------------------------------------------------------------------------- |
| **Shop VP** | Victory Points to buy from the Frontier Shop; blank if not shop-purchasable                   |
| **Type**    | Held-item category (e.g. Power Boost, Defense, Recovery); hold items and berries only   |




