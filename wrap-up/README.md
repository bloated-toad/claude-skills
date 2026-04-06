# Wrap-Up

End every Claude conversation with a structured deliverable instead of just closing the tab.

## The problem

When you finish a long conversation with Claude, the insights are trapped in the chat. You might remember the conclusion, but you lose the reasoning path, the moments where your thinking shifted, and the prompting patterns that worked (or didn't). A week later, the conversation is effectively gone.

Asking Claude to "summarize this conversation" gets you a flat recap — a list of topics you discussed. That's not very useful. You already know what you talked about.

## What this skill does differently

This skill produces a four-section Markdown document designed to be valuable *after* you've forgotten the conversation:

**Discussion Summary** — A narrative arc, not a topic list. How did the thinking develop? Where did it pivot? What was the final position and how did you get there?

**Key Insights** — Distilled takeaways that stand on their own. Not "we discussed X" but "the important thing about X is Y because Z." These should make sense to you a month later without re-reading the chat.

**Prompting Critique** — This is the core differentiator. The skill reviews your actual prompting behavior and gives you specific, critical feedback: what worked, what didn't, and concrete tips for next time. It also flags moments where Claude gave a weak answer that you didn't push back on. The goal is to make you measurably better at prompting over time.

**Bibliography** — Real sources cited in the discussion, plus genuinely relevant further reading. Nothing fabricated, nothing padded. If nothing fits, the section says so.

## Why not just ask for a summary?

A generic "summarize this" request optimizes for completeness — Claude tries to cover everything you said. This skill optimizes for *future usefulness*. The summary traces the evolution of thinking rather than listing topics. The insights are distilled rather than restated. And the prompting critique is something you'd never get from a summary request at all — it requires the skill to explicitly instruct Claude to be critical about both your prompting and its own responses.

## Installation

Copy the `SKILL.md` file into your Claude skills directory:

```
.claude/skills/wrap-up/SKILL.md
```

Then trigger it at the end of any conversation with "let's wrap up", "post-mortem", "what did we cover", or similar.
