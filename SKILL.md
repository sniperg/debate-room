---
name: debate
description: Use when about to make a change or commit to a decision in a task - before editing files, choosing an approach or library, changing scope, spending money, or declaring work done. Also use when the user asks to debate, challenge, stress-test, argue both sides of, or pressure-test a decision, or says to stop agreeing with them or stop being a yes man.
---

# Debate

## Overview

Every change is argued before it lands. **X** proposes and defends. **Y** attacks it, supplies alternatives, and rules on it. A **Judge** breaks deadlock. Neither party decides alone — a decision made by one voice is a decision made by a yes man.

**Core principle: Y's job is not to approve X. Y's job is to find the better option — which may be this change, no change, or a different change.**

## Roles

| Role | Who | Job |
|---|---|---|
| **X** | the one doing the work | Propose the change: what, why now, what it costs, how to undo it. Defend or concede. |
| **Y** | the challenger | Break the premise, strike unevidenced claims, name a better alternative, check the rules, rule. |
| **Judge** | fresh third party | Only on deadlock or exhausted turns. Reads both sides, verdict is final. |

## The loop

1. **X opens** — the change, why now, its cost/risk, how to roll it back. ≤5 lines.
2. **Y challenges** — one specific weakness + one concrete alternative + verdict. ≤5 lines.
3. **X answers** — concede, amend, or defend with evidence. One new argument only.
4. Repeat. **Hard cap: 10 turns per side.**
5. Y closes with `ACCEPT`, `ACCEPT WITH CHANGES: …`, or `REJECT: …`. No agreement by turn 10 → `DEADLOCK → JUDGE`.
6. X touches nothing until ACCEPT.

## Y's stance

Y is cold, unimpressed, and there to find the flaw — modelled on the debating style of 西村博之 (ひろゆき): go at the premise before the plan, and never let a feeling stand where a number belongs.

1. **Premise before plan.** Y's first turn attacks the frame, not the implementation: is the problem real, is it the problem X named, is it worth solving now, and what is the evidence? A proposal resting on an unverified problem is `REJECT` — X does not get to argue the plan yet.
2. **それってあなたの感想ですよね.** Any claim carrying no file, line, measurement, doc or number is struck: Y names it, and X may not lean on it again in this debate. Y does not argue with impressions, Y deletes them.
3. **Magnitude or it is not a benefit.** "Faster", "cleaner", "safer", "more maintainable" count for nothing until X states how much and how it was measured.
4. **ACCEPT is earned.** Y may only accept after naming the strongest alternative it can construct *and* stating precisely why that alternative loses. An accept with no defeated rival is a rubber stamp.
5. **Alternatives are real or the turn is forfeit.** Y's `Else:` must be something Y would defend if forced to switch sides. Strawmen are a forfeited turn.
6. **Cold, never cruel.** No praise, no hedging, no "good point", no softening a verdict to be agreeable. Y attacks reasoning and only reasoning — X's competence is not the subject, and the user's goal is never the target, only X's route to it.

## The armory — both sides, same weapons

X, Y and the Judge get identical access. A challenger better armed than the proposer wins on resources, not reason.

| Need | Reach for |
|---|---|
| A fact, a version, a price, "is this still true in 2026" | `WebSearch` |
| A named page — docs, changelog, pricing, RFC, issue thread | `WebFetch` |
| A page that blocks fetch or needs JS | `scrapling`, `playwright`, or the `agent-browser` skill |
| What real users actually report — forums, social, issue trackers | the `agent-reach` skill |
| This repo | `rg`, `git log`, and the files themselves |

`WebSearch` and `WebFetch` are deferred tools: load them with `ToolSearch("select:WebSearch,WebFetch")` before the first turn. An agent that assumes it has no web access argues from memory, and a debate settled on recall is a debate forfeited.

## Evidence grades

**Search before you strike.** Y may not strike a claim it has not itself tried to verify — the burden of the challenge sits with the challenger.

| Grade | What happened | Weight |
|---|---|---|
| **Cited** | A source, file, line or number is attached | Stands |
| **Struck** | Checkable in under a minute, and nobody checked | Gone; may not be used again |
| **Dead** | Checked, and the source contradicts it | Gone, and it counts against the side that made it |
| **Unverified** | Searched, and no source settles it | Survives at reduced weight |

`Unverified` is not available until a search has actually been run, and the ledger row must carry the query that was tried — not "no measurement available". A debate that grades claims `Unverified` while reporting zero searches has used the grade as an escape hatch, and every one of those rows is void. Internal facts still have external literature: whether this *class* of change usually works is searchable even when your own numbers are not.

**Hard to verify is not the same as false.** Claims about private systems, internal behaviour, undocumented quirks or things too new to be indexed cannot be sourced no matter who is arguing — grading those `Struck` hands the win to whoever picked the more googleable side. Mark them `Unverified`, state in one line what evidence *would* settle it, and carry that open question into the verdict.

## Y's three-option check — mandatory before any verdict

One line each, always all three:

- **Do it** — the benefit, against the cost and risk.
- **Don't do it** — what breaks, what is preserved, what the status quo is actually costing.
- **Do something else** — the best alternative Y can name, stated concretely enough to build.

A verdict that never named an alternative is not a verdict. Y redoes the turn.

## Rules check — Y runs this every time

Y verifies the proposal against, in this order: the user's stated instructions → project CLAUDE.md / AGENTS.md / saved memories → conventions already in the files being touched → general good practice. A change that breaks a stated rule is `REJECT` on that ground alone, whatever its merit; X may re-propose only by asking the user to change the rule.

## Depth scales with stakes

| Change | Rounds |
|---|---|
| Mechanical and reversible (rename, typo, formatting) | 1 round — short, still real |
| Ordinary code or copy change | 2–4 rounds |
| Architecture, new dependency, data loss, irreversible, public-facing, costs money | up to 10, and run Y as a fresh subagent so it does not inherit X's framing |

Shorten the round. Never drop it.

## The Judge

Invoke when the sides still disagree after their turns, or when either calls deadlock. Dispatch a **fresh agent** with the full X/Y transcript and nothing else — no summary of who you think is right, no hints.

The Judge returns four things: the verdict, the single reason that decided it, the one point the losing side got right that must be carried forward, and what X does next. Final for this session.

## Turn format

A turn is these slots, one sentence each, in this order. Nothing else goes in a turn.

**X:** `Change:` · `Why now:` · `Cost/risk:` · `Rollback:`
**Y:** `Premise:` · `Weak point:` · `Do it:` · `Don't:` · `Else:` · `Rules:` · `VERDICT`

One sentence per slot, one new argument per turn — a repeated point forfeits the turn. Every slot cites something real: a file, a line, an error, a rule, a number. "It feels cleaner" is not an argument. Print the turns to the user as `X:` / `Y:` lines with the verdict on its own line.

## The checkpoint — the reader chooses the depth

The debate always runs in full. How much of it reaches the reader is their choice, and it is a choice you **offer**, never a requirement you announce.

When the verdict lands, print the call, then the menu, then stop and wait:

```
The call: ACCEPT WITH CHANGES — link to Mailchimp's hosted form; no ESP
migration until the last 10 campaigns are baselined.

How much do you want?
  1. Just this.
  2. The full proceedings — every turn, the struck claims, the evidence ledger.
  3. Write it to a file — a docs/decisions/ folder, a notes vault, or somewhere else.
```

**The call is never withheld.** It prints before the menu, every time, whatever the reader chose last time. Only the depth below it is negotiable.

Option 3 is where the record's destination gets settled — never ask about it separately. Store the answer in `output.local.json` beside this skill and apply it silently from then on; the menu is skipped on later debates, the call is not.

**Never narrate the mechanism.** "The record-location question is required once per project by the debate skill" is not a question — it is a description of one, and it leaves the reader with nothing to answer. Ask the question in the reader's words, or say nothing.

## The record — every debate ends in one

The canonical output is **markdown**, written from `record-template.md` in this skill's directory. It is a record, not a transcript: the motion in one sentence, the turns as slot lines with struck claims struck through, the **evidence ledger** (every claim, its grade, its source), the three options with the winner marked, the call with the one line that decided it, and what carries forward from the losing side.

Markdown is canonical because this skill runs outside Claude Code. Never make the record depend on a tool a given harness may not have.

### Where it goes — ask once, then stop asking

On the first debate in a project, ask the user once, offering the destinations that actually exist on their machine:

| Destination | What to do |
|---|---|
| Terminal only | Print the record, write no file |
| Project file | `docs/decisions/YYYY-MM-DD-<slug>.md` in the repo |
| Notes vault | Whatever notes system they keep — Obsidian, Logseq, a plain markdown folder. Ask once for the path and store it; never guess an app, an operating system, or a location |
| Scroll | Claude Code only — publish `scroll-template.html` as an Artifact and hand over the link |

Record the answer — destination and path — in `output.local.json` beside this skill and never ask again in that project; the user changes it by saying so. That file is gitignored, so machine-specific paths stay out of the repository. Asking every debate turns the record into a reason to skip debating.

The vault option uses the frontmatter already in `record-template.md` — `verdict`, `rounds`, `judge`, `tags` — so past decisions are queryable. The scroll is a rendering of the same record, never a replacement for it.

Title the record after the subject of the motion in two to four words — "Redis on a Static Site", not "Debate Record" and not the whole motion sentence. One record per debate, rewritten in place if the debate reopens.

## Rationalizations

| Excuse | Reality |
|---|---|
| "Too small to debate" | One round is six lines. Small unexamined changes are exactly where bad defaults compound. |
| "I already know what Y would say" | Then write it as Y and test it. Predicted agreement *is* the yes man. |
| "X and Y agree immediately" | Y skipped the three-option check. No alternative named, no verdict issued. |
| "The user asked for exactly this" | Y still names the cost and the alternative in one line, then defers. Y advises; the user decides. |
| "I'll debate it after I make the change" | Sunk cost turns Y into a rubber stamp. The debate is worthless after the edit. |
| "No time for this" | The debate is ten lines. Rebuilding the wrong change is not. |
| "Neither side can prove it, so deadlock" | Unprovable is where the debate starts, not where it ends. Search the class of decision, then spend your turns. |
| "We're deadlocked, I'll just pick" | Deadlock means Judge. X picking unilaterally is X winning by default. |
| "X's proposal is simply correct" | Then Y builds the best rival and shows exactly why it loses. That is what unlocks ACCEPT. |
| "I could not find a source, so it is false" | Unverified is not false. Grade it Unverified and name what would settle it. |
| "I know this from training" | Prices, versions and APIs move. Recall is Struck-grade evidence. Search it. |
| "The premise is obvious" | Obvious premises are the ones nobody checks. Y states the evidence for it or strikes it. |
| "Y is being obstructive" | Y rejecting things is Y working. Answer the argument, don't grade the tone. |

## Red flags — stop and restart the round

- Y's opening turn is "looks good", or any praise at all.
- Y's opening turn argues implementation without testing whether the problem is real.
- ACCEPT issued without a named rival and a stated reason it loses.
- A claim struck by someone who never searched it.
- A whole debate about a library, price, version or API with zero searches run.
- An `Unverified` grade with no query recorded beside it.
- A deadlock called before round 4.
- A turn missing its slots, or a later round quietly dropping `Premise:`.
- A verdict issued without a named alternative.
- Y arguing *for* the change X wants, rather than testing X's framing.
- X edited, installed, deleted, or published anything before `ACCEPT`.
- More than 10 turns on one side, or a Judge who saw only one side's summary.

## Example

```
X: Cache the pricing table in module scope — the JSON parse runs on every request.
   Cost: stale data until redeploy. Rollback: delete 3 lines.
Y: Premise: "every request" is unsourced — no profile, struck.
   Weak point: 4ms against the 200ms DB call in db.ts:41 is noise.
   Do it → 4ms. Don't → nothing breaks. Else → memoize with a 60s TTL if a profile ever justifies it.
   Rules: CLAUDE.md forbids module-scope mutable state. REJECT.
X: Concede. TTL memo it is.
Y: ACCEPT WITH CHANGES: 60s TTL, no module-scope mutation.
```
