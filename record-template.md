---
title: <two to four words naming the subject of the motion>
date: <YYYY-MM-DD>
tags: [decision, debate-room]
verdict: <accept | accept-with-changes | reject>
rounds: <turns used, of 10>
searches: <how many were actually run>
judge: <true | false>
---

# <the motion, as a proposal or a question>

**Motion.** <One sentence: what is being changed, what it costs, and on whose say-so.>

## Proceedings

### X — round 1

- **Change:** <what X wants to do>
- **Why now:** <the trigger — struck through and tagged `STRUCK` if it turns out to carry no evidence>
- **Cost:** <time, money, risk, and what it commits you to>
- **Rollback:** <how to undo it>

### Y — round 1

- **Premise:** <is the problem real, is it the problem X named, is it worth solving now>
- **Weak point:** <the single weakest link in X's case>
- **Do it:** <the benefit, with a magnitude>
- **Don't:** <what breaks, what is preserved>
- **Else:** <the strongest alternative, concrete enough to build>
- **Rules:** <user instructions, then CLAUDE.md / AGENTS.md, then the conventions in the files touched>
- **Verdict:** `ACCEPT` / `ACCEPT WITH CHANGES` / `REJECT` / `DEADLOCK → JUDGE`

<!-- Repeat X and Y for each round. Mark a struck claim ~~like this~~ followed by `STRUCK`. -->

## Evidence ledger

| Claim | Grade | Source |
|---|---|---|
| <the claim, as it was made> | `CITED` | <file and line, or URL and the date fetched> |
| <the claim> | `STRUCK` | <checkable in under a minute, and nobody checked> |
| <the claim> | `DEAD` | <what was checked, and how it contradicted the claim> |
| <the claim> | `UNVERIFIED` | <the query that was actually run, and what evidence would settle it> |

## The three options

| Option | Case |
|---|---|
| Do it | <the case for the change> |
| Don't | <the case for the status quo> |
| Do something else | <the case for the alternative> |

<!-- Mark the winner with ✔ -->

## The call

> [!success] <VERDICT> — <who issued it, which round>
> <The decision in one sentence, then the conditions attached to it.>

> [!note] Carried forward from the losing side
> <The one point the losing side got right that survives the verdict.>

<!-- If the Judge was called, replace the block above with the Judge's four parts:
     the verdict, the single reason that decided it, what the losing side got
     right, and what X does next. -->

---
*X proposes · Y challenges · Judge breaks ties — [debate-room](https://github.com/sniperg/debate-room)*
