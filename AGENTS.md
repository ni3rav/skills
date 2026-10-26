# Skills catalog

Author skills in `skills/<name>/`. That tree is what `npx skills add` publishes.

`.agents/skills/` is the install tree for entries in `skills-lock.json`. The skills CLI skips those entries, so they stay out of the catalog. Install a dependency with `npx skills add`. Author only under `skills/`.

## Add a skill

One skill per request. Stay flat (`skills/<name>/`) until several skills share a leading word that earns a bucket (`skills/<bucket>/<name>/`).

1. Name the job and its trigger branch. Done when the directory name is fixed: lowercase, hyphens, at most 64 characters.
2. Write `skills/<name>/SKILL.md` and only the sibling files that skill reaches. Disclosed reference goes in `references/`, executables in `scripts/`, templates in `assets/`. Done when the frontmatter `name` equals the directory and no other skill directory changed.

Write the document with the `writing-for-agents` skill and apply `notes/skill-checklist.md`: trigger, structure, steering, pruning.
