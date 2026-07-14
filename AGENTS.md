# Agent instructions

## Skills and README

Each skill under `skills/<name>/` is listed in `README.md` with a short blurb
and an install command:

```bash
npx skills add jpbullalayao/pokemon-ai --skill <name>
```

**Rule:** Whenever you create, rename, delete, or materially change a skill
(purpose, description, or install name), update the matching section under
**Skills** in `README.md` in the **same commit/PR**. Do not leave the README
out of sync with `skills/`.

- **New skill** — add a `### \`<name>\`` section (blurb + install command)
  alongside the other skills.
- **Rename** — rename the section heading and install `--skill` argument.
- **Delete** — remove the section.
- **Description / purpose change** — refresh the blurb so it still matches
  the skill’s `SKILL.md` frontmatter `description`.

**Verification before finishing:** every directory under `skills/` that
contains a `SKILL.md` has a corresponding `###` section in `README.md`, and
there are no README skill sections without a matching `skills/<name>/` folder.

---

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
