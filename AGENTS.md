# Agent instructions

## Champions reference docs

The `pokemon-champions` skill bundles reference tables so it works when installed outside this repo. Two locations must stay in sync:

| Role | Path |
| --- | --- |
| Canonical source | `vgc/champions/` — repo-wide reference docs linked from `README.md` |
| Skill bundle | `skills/pokemon-champions/docs/` — copies installed with the `pokemon-champions` skill |

**Rule:** Whenever you create, edit, rename, or delete any file under `vgc/champions/`, apply the same change to the corresponding file under `skills/pokemon-champions/docs/` in the **same commit/PR**. Do not leave the two locations out of sync.

**Bulk refresh** (after adding files or when mirroring all four tables):

```bash
cp vgc/champions/{pokemon,regulation-m-a,regulation-m-b,items}.md skills/pokemon-champions/docs/
```

**Verification before finishing:** run `diff -r vgc/champions skills/pokemon-champions/docs` and confirm no differences.
