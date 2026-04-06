# Steelman

A structured dialectical analysis skill for Claude. Paste a take, get a verdict.

## The problem

Claude's default behavior when you show it a controversial claim is to hedge. You get "there are valid points on both sides," a list of considerations, and no conclusion. This is useless if you're trying to figure out what's actually true.

Asking Claude to "steelman this" gets you halfway there — you'll get a charitable reconstruction of the argument. But then what? You're left to evaluate it yourself, without the counter-case presented at the same level of rigor, and without a verdict that decomposes the claim into parts and tells you which hold up and which don't.

## What this skill does

This skill runs a full dialectical analysis in three stages:

**Steelman** — Reconstruct the strongest possible version of the claim. Not the version as stated (which is usually sloppy), but the version a smart, well-informed defender would make. Supply evidence, fix logical gaps, and separate bad packaging from defensible ideas.

**Counter** — Switch sides and present the best case against the position you just built. Critically, the counter engages with the steelman, not the original weak version. This prevents the common failure mode where the "counterargument" just knocks down easy targets the steelman already fixed.

**Verdict** — Drop the advocacy and assess honestly. Decompose the claim into sub-claims, evaluate each independently, identify the crux (the key question that determines who's right), and arrive at a judgment with rough probabilities. "The diagnosis is right but the prescription is wrong" is more useful than a single thumbs-up or thumbs-down.

## Debate mode

The skill auto-detects when you paste a back-and-forth between two or more people (a Twitter exchange, Reddit thread, Slack argument, etc.) and adapts its structure. In debate mode, it steelmans both sides independently, then delivers a two-layer verdict:

1. **Who won the debate as conducted?** — Evaluated on argument quality, evidence use, and rhetorical effectiveness.
2. **Who is actually right?** — Evaluated on the merits of the underlying question, independent of how well each person argued.

The gap between these two assessments is often the most interesting part. Good debaters can be wrong. Bad debaters can be right. This skill surfaces that distinction explicitly.

## What makes this different

There are other steelman tools and prompts out there. Here's what this one does that they don't:

- **It renders a verdict.** Most steelman tools stop at "here's the strongest version of both sides" and leave you to judge. This skill decomposes the claim, evaluates each part, identifies the crux, and makes a call — with calibrated uncertainty, not false confidence.

- **The counter engages the steelman.** The most common failure in AI-assisted argument analysis is that the "counterargument" attacks the original weak claim rather than the reconstructed strong version. This skill explicitly instructs the counter to respond to the steelmanned position, which produces substantially better analysis.

- **It auto-detects input type.** Paste a single claim, you get single-claim analysis. Paste a debate, you get multi-party analysis with the dual-layer verdict. No configuration needed.

- **It doesn't moralize.** The skill takes ideas seriously regardless of how uncomfortable they are. Moral assessment can appear in the verdict, but it never prevents effective steelmanning. If a position is defensible, the skill will defend it — that's the point.

- **It scales depth to input.** A one-line hot take gets a proportionate response. A detailed policy argument gets thorough treatment. It doesn't produce 2,000 words about a throwaway tweet.

## Installation

Copy the `SKILL.md` file into your Claude skills directory:

```
.claude/skills/steelman/SKILL.md
```

Then paste any claim, take, or argument into Claude and say "steelman this" — or just paste the take with no commentary. The skill triggers on hot takes, controversial claims, pasted social media content, debate excerpts, or any debatable assertion.

## Examples of good inputs

- A tweet or Reddit comment you want analyzed
- A policy position someone made that sounds right but you're not sure
- A back-and-forth argument between two people where you want to know who's actually correct
- A quote from an article, book, or podcast that makes a strong claim
- Your own position that you want stress-tested before committing to it
