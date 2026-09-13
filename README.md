# debate-room

A Claude Code skill that refuses to let a decision through on one voice.

Every change is argued before it lands. **X** proposes and defends it. **Y** goes at the premise before the plan, strikes any claim that arrives without evidence, names a better alternative, checks it against the rules, and rules on it. When the two cannot agree, a **Judge** — a fresh third party who sees only the transcript — issues the final verdict.

The point is not friction. The point is that a decision approved by the same reasoning that produced it has not been checked at all.

## What it does

- **Y attacks the frame first** — is the problem real, is it the problem you named, is it worth solving now. A proposal resting on an unverified problem is rejected before its implementation is even discussed.
- **Unevidenced claims are struck, not argued with.** No file, line, measurement or number attached means the claim is gone for the rest of the debate.
- **ACCEPT is earned:** Y may only accept after building the strongest rival option and stating exactly why it loses.
- **Both sides are armed the same.** `WebSearch`, `WebFetch`, `scrapling`, `playwright`, `agent-browser`, `agent-reach` — a challenger better armed than the proposer wins on resources, not reason.
- **Search before you strike.** Y may not strike a claim it has not itself tried to verify, and hard-to-verify is graded `Unverified`, never `Struck` — otherwise the more googleable side always wins.
- **You choose the depth.** Every debate ends with the call, then a menu: just the verdict, the full proceedings, or write it to a file. The call is never withheld behind the question.
- **Every debate ends in a record** — portable markdown with the turns, the evidence ledger, the three options and the call, filed wherever you keep decisions (a repo folder, an Obsidian vault, or a published HTML scroll on Claude Code). Markdown is canonical so the skill works in Codex and anywhere else, not just Claude Code.
- **Y must weigh three options before any verdict:** do it, don't do it, do something else. A verdict that never named an alternative is not a verdict.
- **Y checks the rules every round** — your instructions, then `CLAUDE.md` / `AGENTS.md` / saved memories, then the conventions already in the files being touched. A change that breaks a stated rule is rejected on that ground alone.
- **Turns are slots, not essays.** One sentence each, one new argument per turn, every claim citing a file, a line, an error, or a number.
- **Ten turns per side, hard cap.** No agreement by then goes to the Judge.
- **Depth scales with stakes** — one round for a rename, up to ten for anything irreversible, public-facing, or costly.

## Install

```bash
# Claude Code
git clone https://github.com/sniperg/debate-room.git ~/.claude/skills/debate

# Codex
git clone https://github.com/sniperg/debate-room.git ~/.codex/skills/debate

# Gemini CLI, Copilot CLI — the cross-runtime skills directory
git clone https://github.com/sniperg/debate-room.git ~/.agents/skills/debate
```

**How you invoke it differs by harness.** Claude Code exposes skills as slash commands, so `/debate` works there. Codex has no user-defined slash commands — skills are offered to the model, so ask for it by name: *"use the debate skill on this"*. In both, the agent should also reach for it unprompted before a change lands.

## Example

```
X: Cache the pricing table in module scope — the JSON parse runs on every request.
   Cost: stale data until redeploy. Rollback: delete 3 lines.
Y: Do it → saves ~4ms/req. Don't → 4ms is invisible next to the 200ms DB call.
   Else → memoize with a 60s TTL, same win, no staleness.
   Rules: CLAUDE.md forbids module-scope mutable state. REJECT.
X: Concede. TTL memo it is.
Y: ACCEPT WITH CHANGES: 60s TTL, no module-scope mutation.
```

## License

MIT
