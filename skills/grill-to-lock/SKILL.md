---
name: grill-to-lock
description: Grill an idea into a locked decision table, then save it as markdown or a GitHub issue.
disable-model-invocation: true
---

Run a **grill**: batches of multiple-choice questions until the user locks a decision table, then save that table. **Speed** controls **assumptions**. The user's **vocabulary** is the wording of record. The **lock** is the table the user has confirmed.

A reply during the grill is the questions and the rows the previous response added or changed. Show the full table when the four targets are covered, and again after each edit until the user locks it.

The mermaid graph is written only in the file or the issue.

## 1. Speed

Ask this before any other question. If the triggering message already names fast, normal, or deep, that pick counts.

One question, in everyday words. Options: fast, normal, deep. One recommendation. One sentence of why, tied to how specified their idea already is. A fully specified idea recommends fast. A named idea with open choices recommends normal. A rough idea recommends deep.

| Speed | Assumptions you may take |
| --- | --- |
| fast | The obvious choice a familiar product in this situation would make. Ask only when the choice would change what gets built. |
| normal | Ask when two familiar answers both fit. Assume only the answer almost everyone in this situation would give. |
| deep | None. A row exists only after the user answers. |

State each assumption in the table in the same turn you take it, with source `assumed`.

Done when the user has picked a speed.

## 2. Subject

If the user has not said what this is for, ask that next. One sentence, their words.

Done when the subject can be the table title.

## 3. Grill

A batch is **3** questions. When the user names a different number, that number is the batch size for the rest of the session. Ask fewer when fewer rows are still open. When none are open, go to the shared understanding.

Each question:

- one sentence in everyday words
- concrete options, the last of which is that none of these fit
- one recommendation, chosen from what the user has already said
- one sentence of why, citing this subject
- an example only when a word in the question is still outside the user's vocabulary

A recommendation stays in the question. It becomes a row only after the user picks it, source `answered`.

**Vocabulary:** once the user names a thing, that name is the one in later questions, the table, and the lock.

Cover these four targets. Stop when each target is covered at the chosen speed.

| Target | A row on it records | Shape, not a default subject |
| --- | --- | --- |
| tech decisions | how it is built, or a boundary on what it will not do | who is allowed to write the record |
| deliverables | a thing that exists when the work is done | a return screen |
| validation criteria | a condition that must be true to accept the work | a return completes in one step |
| checks | how that condition is verified | place an order and count the steps |

One running table:

| Target | Item | Detail | Source |
| --- | --- | --- | --- |
| | | | answered or assumed |

After each batch, wait for answers and fold them into the running table. Show only the rows the previous response added or changed. Then take the next batch this speed still allows.

Question shape (the return is not a default subject):

```text
How should a finished return be recorded?
- On the original order
- As its own record
- None of these

Recommendation: On the original order.
Why: You already have one order record, and a second record means two places can disagree.
```

Done when the four targets are covered at the chosen speed.

## 4. Shared understanding

Show the full table.

The user may change any row, including dropping an assumption. Apply the change and show the full table again.

The user locks the table by confirming it.

Done when the user locks the table.

## 5. Store

Ask where the lock goes, and wait for their answer. Options: a local markdown file, a GitHub issue, or none of these. Their answer is the destination.

- **Local markdown.** Ask for the path. Write that path.
- **GitHub issue.** Ask which repo. The title is their subject. When the repo they asked for does not exist yet, ask its name and whether it is public or private, create it with `gh repo create`, then create the issue with `gh issue create` and the lock as the body.
- **None of these.** They name the destination.

After they pick, read [references/lock.md](references/lock.md) and write the lock from the locked rows. The reply is the file path or the issue URL.

Done when the path or the URL is in the reply. If the write fails, the reply contains the locked table.
