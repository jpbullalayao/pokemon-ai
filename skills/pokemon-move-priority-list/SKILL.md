---
name: pokemon-move-priority-list
description: >
  Pokemon move priority reference for Generation IX turn order. Use when reasoning
  about priority brackets, which move goes first, Fake Out, Sucker Punch, Extreme
  Speed, Protect, Trick Room, Quick Attack, Aqua Jet, Ice Shard, Grassy Glide,
  or speed vs priority interactions in VGC and competitive battles.
---

# Pokemon Move Priority List

Authoritative move priority reference for Generation IX. Use this to reason about turn order for priority moves like Sucker Punch, Extreme Speed, Fake Out, Protect, Trick Room, etc.

## How to read move priority

Each move has a hidden priority value that determines turn order before Speed is compared.


| Priority | Meaning            | Turn order effect                       |
| -------- | ------------------ | --------------------------------------- |
| `+5`     | Highest priority   | Acts before all lower-priority moves    |
| `0`      | Standard (default) | Speed decides order within this bracket |
| `−7`     | Lowest priority    | Acts after all higher-priority moves    |


Rules for reasoning:

- **Higher priority always goes first.** A +1 move beats a 0 move regardless of
Speed. Values range from +5 to −7; almost all moves are 0.
- **Same bracket, then Speed.** When two moves share a priority value, the
faster Pokemon moves first (unless an item or ability overrides bracket order,
e.g. Quick Claw, Lagging Tail).
- **Trick Room does not change priority.** It only reverses Speed order within
a bracket. +1 still beats 0 even under Trick Room.
- **Switching happens before moves** in most cases.

---

## Priority bracket table (Generation IX)

Moves with non-zero priority, grouped by bracket.


| Priority | Moves                                                                                                                                                                                   |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `+5`     | Helping Hand                                                                                                                                                                            |
| `+4`     | Baneful Bunker, Burning Bulwark, Detect, Endure, King's Shield, Obstruct, Protect, Spiky Shield, Silk Trap                                                                              |
| `+3`     | Fake Out, Quick Guard, Upper Hand, Wide Guard                                                                                                                                           |
| `+2`     | Ally Switch, Extreme Speed, Feint, First Impression, Follow Me, Rage Powder                                                                                                             |
| `+1`     | Accelerock, Aqua Jet, Baby-Doll Eyes, Bullet Punch, Grassy Glide*, Ice Shard, Jet Punch, Mach Punch, Quick Attack, Shadow Sneak, Sucker Punch, Thunderclap, Vacuum Wave, Water Shuriken |
| `0`      | All other moves                                                                                                                                                                         |
| `−1`     | —                                                                                                                                                                                       |
| `−2`     | —                                                                                                                                                                                       |
| `−3`     | Beak Blast, Focus Punch, Shell Trap                                                                                                                                                     |
| `−4`     | Avalanche                                                                                                                                                                               |
| `−5`     | Counter, Mirror Coat                                                                                                                                                                    |
| `−6`     | Circle Throw, Dragon Tail, Roar, Whirlwind, Teleport                                                                                                                                    |
| `−7`     | Trick Room                                                                                                                                                                              |


\* Grassy Glide is +1 only while Grassy Terrain is active and the user is grounded.
