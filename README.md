# skills

My personal set of agent skills, mostly derived from that of Matt Pocock and Poteto.

## Layout

| Path | What it is |
| --- | --- |
| `skills/<name>/` | A skill in the catalog. `SKILL.md` is required. `references/`, `scripts/`, and `assets/` only when that skill reaches them. |
| `.agents/skills/` | Skills installed to help write these. Not the catalog. |
| `skills-lock.json` | Names those installs so `npx skills add` leaves them out. |
| `notes/skill-checklist.md` | Checklist for phrasing a skill: trigger, structure, steering, pruning. Not a published skill. |

`writing-for-agents` is installed for that reason. It is not part of the catalog.

MIT license.
