# Prediction Tracker

A persistent prediction journal that turns vague hunches into scorable claims and builds your calibration over time.

## The problem

You make predictions all the time — in forecast conversations, reading the news, debating with friends, thinking about your career. But you never write them down, so you never find out if you're any good. Worse, memory is self-serving: you remember the calls you got right and quietly forget the ones you got wrong. Without a written record, you can't improve.

The tools that exist for this are either heavyweight (prediction market platforms where you bet real money) or disconnected from where the predictions actually happen (standalone apps you have to context-switch into). None of them live where you're already thinking — inside a conversation with an AI that just helped you reason through a developing situation.

## What this skill does

This skill maintains a persistent markdown prediction log on your filesystem. It operates in three modes:

**Record.** After a conversation where you've been assigning probabilities or making claims about the future, the skill extracts your predictions, forces them into precise, scorable format, and saves them. It pushes you on the parts people skip: What exactly are you claiming? What number would you put on it? By when? How will we know? A prediction without a probability isn't a prediction — it's a vibe.

**Update.** When new information arrives that bears on an open prediction, the skill helps you revise your number and logs the update with a datestamped rationale. The update trail is preserved, so you can see how your estimate evolved as the situation developed. Updates that don't change the number are also worth recording — they show active maintenance of a position rather than neglect.

**Review.** When predictions come due, the skill searches for current information, walks you through resolution (YES / NO / AMBIGUOUS), calculates Brier scores, and co-writes a retrospective. With enough resolved predictions, it runs calibration analysis: are your 70% predictions coming true 70% of the time? Are you systematically overconfident? Do your updates make your forecasts better or worse?

## What makes this different

There is no other published skill that does this. The prediction tracking space has prediction market API connectors (Polymarket, Kalshi), financial time series forecasters (ARIMA, Prophet), and component-level reasoning frameworks (premortems, bias checklists). None of them maintain a personal prediction journal. Here's what's distinctive about this one:

**It lives where the predictions happen.** You just finished a forecast conversation with Claude. The skill extracts predictions from that conversation and logs them — no context switch, no separate app, no copy-pasting into a spreadsheet. The transition from "thinking about the future" to "recording a scorable claim" is one sentence: "track that."

**It forces precision.** The skill won't let you record a prediction without a probability, a resolution date, and resolution criteria. This is the hard part that people skip, and it's the reason most prediction-tracking attempts die. Vague claims can't be scored, and unscored claims can't teach you anything.

**Dual Brier scoring measures update quality.** Every resolved prediction gets two Brier scores: one for the initial estimate and one for the final estimate (after any updates). The gap between them tells you something most calibration tools ignore — whether your updates are making your forecasts better or worse. If your initial Brier score is consistently better than your final score, you're updating in the wrong direction, which is a specific, actionable finding.

**The update trail preserves your reasoning.** Each probability revision includes a datestamped rationale. When you review a resolved prediction, you can trace the full arc of your thinking: what you initially believed, what new information arrived, how you adjusted, and where you ended up. This is the raw material for identifying systematic biases — "I keep overreacting to dramatic news" or "I consistently underweight base rates."

**Retrospectives are the real output.** The Brier score tells you *how wrong* you were. The retrospective tells you *why*. The skill co-writes a 2-4 sentence retrospective for every resolved prediction, covering which assumptions broke, which biases were in play, and what you'd do differently. With enough resolved predictions, it surfaces recurring patterns across retrospectives — that's where the calibration skill development actually happens.

**It's just a markdown file.** The prediction log is a plain markdown file on your filesystem. You can read it, grep it, link to it from your notes, version-control it, or process it however you want. No proprietary format, no database, no vendor lock-in.

## Prerequisites

This skill requires filesystem access. It works in:

- **Claude Code** — filesystem access is built in
- **Claude Cowork** — enable the Filesystem MCP server in Settings → MCP Servers, and grant access to the directory where you want the log stored
- **claude.ai with Filesystem MCP** — same setup as Cowork

If you haven't set up filesystem access yet: in Cowork or claude.ai, go to Settings → MCP Servers → add the Filesystem server, and allow access to the directory where you keep your notes. By default, the skill stores the prediction log as `prediction-log.md` in your Documents folder, or wherever you tell it to.

## Installation

Copy the `SKILL.md` file into your Claude skills directory:

```
.claude/skills/prediction-tracker/SKILL.md
```

Then say "track this prediction," "log my predictions," "what are my open predictions," or "check my calibration" to get started.

## Pairs well with

This skill is the natural companion to a forecasting skill. The forecast produces the predictions; this skill records them, scores them, and surfaces the patterns. The loop — forecast → record → update → review → learn → forecast better — is the complete calibration training cycle.
