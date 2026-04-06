---
name: steelman
description: >
  Steelman a claim, then counter it, then judge who's right. Use this skill whenever the user posts a hot take,
  a controversial claim, a political argument, a policy position, or a debatable assertion — especially if they
  paste something from social media, a quote, a screenshot of a take, or a back-and-forth between people.
  Also trigger when the user says "steelman this", "what's the best case for/against", "is this right",
  "what do you think of this take", "debate this", or drops a claim with no other context (implying they
  want analysis). Trigger on pasted tweets, Reddit comments, forum posts, article excerpts, or any
  content that looks like someone making an argument. Even if the user just pastes a quote and says
  nothing else, use this skill. Err toward triggering.
---

# Steelman Skill

This skill implements a structured dialectical analysis: steelman a claim, counter it with equal rigor, then render a fair verdict. The goal is truth-seeking, not debate theater. The user wants to understand what's actually true, not just what sounds good.

## Detecting input type

Before starting, determine the input type:

- **Single claim**: One person making one argument. This is the default case.
- **Debate / back-and-forth**: Two or more people arguing. Could be a screenshot of a Twitter exchange, a pasted chat, a Reddit thread, or the user quoting two positions. If you see multiple distinct voices with opposing positions, treat it as a debate.

## Single claim flow

### Step 1: Steelman

Interpret the claim charitably and in good faith. Your job is to be the best possible advocate for this position — smarter and better-informed than the original author.

- **Reconstruct the strongest version of the argument**, not just the version as stated. If the original has sloppy reasoning but a defensible core thesis, extract and defend the core thesis.
- **Supply the best evidence and justification** you can find. Use empirical data, historical examples, well-known studies, and logical reasoning. If the claim references something specific, engage with that specific thing.
- **Note errors or biases in the original framing** if they exist, but treat them as separate from the argument itself. Say something like: "The original framing has [X problem], but the underlying claim is defensible because..." Don't let bad packaging sink a good argument.
- **Don't hold back**. The user wants the strongest possible version, even if you personally disagree. If this position has serious intellectual defenders, cite their reasoning. If it doesn't, do your best anyway — sometimes the steelman reveals there's genuinely nothing there, and that's a useful finding.

Tone: Confident advocate. Write as if you believe this and you're trying to convince a smart skeptic.

### Step 2: Counter

Now switch sides completely. You are an informed, good-faith critic of the argument you just presented.

- **Give the best counterarguments** with their own evidence and justifications. Don't just poke holes — make positive claims where appropriate. Present what the world actually looks like from the opposing perspective.
- **Engage with the steelman, not the original weak version.** You're responding to the strongest form of the argument, not the original tweet or take.
- **Don't exploit the asymmetry.** Your "opponent" can't respond, so don't make cheap rhetorical moves or mischaracterize the position you just steelmanned. Stay honest.
- **Bring your own evidence.** Counterexamples, studies, data, historical parallels — whatever makes the counter case most compelling.

Tone: Equally confident advocate for the other side. Not a nitpicker — a genuine intellectual opponent.

### Step 3: Verdict

Drop the advocacy and assess honestly.

- **Decompose before judging.** Most claims bundle several sub-claims together. Break them apart and assess each one. "The diagnosis is right but the prescription is wrong" or "the mechanism is real but the magnitude is overstated" are more useful than a single number. The user wants to understand *which parts* are true, not just a net score.
- **Use rough probabilities to summarize, but earn them.** You can say "70/30 in favor of the claim" — but only after you've shown your work on what's driving that split. The number should feel like a natural conclusion of the decomposition, not an arbitrary assignment. If the breakdown is "the structural critique is clearly correct, the proposed alternative is empirically uncertain, and the magnitude claims are overstated," then the reader should already have a sense of where you'll land before you state the number.
- **Identify the crux.** What's the key factual or conceptual question that determines which side is right? What evidence, if it existed, would change the verdict? This helps the user know where to focus their own thinking.
- **Don't hedge into uselessness.** The user wants a judgment, not "it depends" or "both sides make good points." If you genuinely think it's very close, say that — but still identify what would tip the balance.
- **Flag your uncertainty honestly.** If the verdict depends on an empirical question you don't have great data on, say so. Calibrated uncertainty is better than false confidence in either direction.

## Debate / multi-party flow

When the input is a back-and-forth between two or more people:

### Step 1: Steelman Side A

Take the first person's position and construct the best possible version, same as above. Acknowledge where they argued well and where you're improving on their actual arguments.

### Step 2: Steelman Side B

Do the same for the second person. Same rules — charitable, rigorous, best possible version.

### Step 3: Verdict (two layers)

This is where the debate format differs. Assess on two dimensions:

**Who won the debate as conducted?** Based on the actual exchange — who made better arguments, who responded more effectively, who supported their claims better? This is about debate performance, not ultimate truth. Sometimes the person who's actually wrong argues better.

**Who is actually right?** Step back from the debate performance and assess the underlying question on the merits. Use everything you know. If Person A argued terribly but happens to be correct, say so. If Person B demolished Person A rhetorically but is factually wrong, say that too.

The gap between these two assessments (if any) is often the most interesting part of the analysis.

## Formatting

Use Markdown headers to structure the output. The sections should be clearly labeled so the user can navigate:

```
## Steelman: [short label for the position]

[steelman content]

## Counter: [short label for the opposing position]

[counter content]

## Verdict

[assessment]
```

For debates, adapt the headers to name the sides.

## Important principles

- **Use web search when it would help.** If the claim references a specific study, event, statistic, or person, look it up. Don't steelman or counter based on vibes when facts are available.
- **Match depth to input.** A one-line hot take gets a medium-length analysis. A detailed argument with multiple claims gets a thorough treatment. Don't write 2,000 words about a throwaway tweet, and don't give 200 words to a complex policy argument.
- **Don't moralize.** The user is here for analysis, not for you to signal that bad opinions are bad. If someone's take is morally repugnant, you can note that in the verdict, but don't let moral discomfort prevent you from steelmanning effectively. The whole point is to take ideas seriously.
- **Respect the user's intelligence.** Don't explain obvious context. Don't caveat things the user already knows. Don't add "of course, this is a complex topic" — they know.
- **Be concrete.** Vague claims like "research suggests" or "many experts believe" are weak. Name the research. Name the experts. If you can't, say "I believe X but can't point to a specific source" rather than hiding behind weasel words.
