---
name: wrap-up
description: >
  Summarize a discussion, extract key insights, critique the user's prompting technique, and suggest further reading.
  Use this skill whenever the user signals they want to conclude or wrap up a conversation. Trigger phrases include:
  "let's wrap up", "let's conclude", "summary of this discussion", "give me a wrap-up", "conclude this chat",
  "let's summarize", "what did we cover", "post-mortem", or any indication the user wants a retrospective on
  the current conversation. Also trigger if the user says "wrap-up skill" or "conclusion skill". This skill
  applies to ANY topic — it is not domain-specific. Use it even for short conversations if the user asks for it.
---

# Wrap-Up Skill

This skill produces a structured end-of-conversation deliverable with three sections: a discussion summary, a prompting critique, and a bibliography. The goal is to help the user retain what they learned and get better at using Claude over time.

## When to use

The user will signal they're ready to conclude. They may say "let's wrap up," "conclude this discussion," "summary," or similar. Sometimes they'll reference this skill by name. When triggered, produce the full output below without asking clarifying questions — the user wants a clean conclusion, not another back-and-forth.

## Attending to the trigger message

The user often includes their own reflections when triggering the wrap-up — things like "let's wrap this up, I really liked how the steelmanning worked" or "good discussion, though I think I went on too many tangents." This embedded feedback is high-signal and should be treated as direct input to the prompting critique. Specifically:

- If the user praises something ("x worked well"), validate or push back on that assessment in the "What worked well" section. Don't just echo it — evaluate whether Claude agrees and add specifics about *why* it worked.
- If the user self-critiques ("I think I went off track"), engage with that in "What could improve" — again, don't just echo. Was their self-assessment accurate? Too harsh? Did it miss the real issue?
- If the user's trigger message contains reflections that don't map neatly to the prompting critique (e.g., "this changed how I think about X"), weave those into the Discussion Summary or Key Insights as appropriate.
- If the trigger message is bare ("let's wrap up"), proceed normally — this section only applies when the user volunteers feedback.

## Output format

Produce the wrap-up as a Markdown file saved to the outputs directory. The filename should reflect the topic discussed (e.g., `wrap-up-browser-tab-design.md`). Also present the content conversationally in-chat so the user can read it immediately.

Use this structure:

```
# Wrap-Up: [Brief Topic Description]

## Discussion Summary

[A concise narrative of how the conversation evolved. Not a transcript — a synthesis.
Start with the opening question or premise, trace the key turns and corrections,
and arrive at the final position. Emphasize how the thinking developed rather than
just listing what was said. Keep it to 2-4 paragraphs.]

## Key Insights

[The most important takeaways — things the user should remember a month from now.
These should be distilled, not just restated. Aim for 3-7 insights depending on
discussion depth. Each insight should be a short paragraph that captures the idea
in a way that stands alone without the full discussion context.]

## Prompting Critique

### What worked well
[Identify specific prompting behaviors that produced better responses. Examples:
pushing back on overclaimed frameworks, grounding abstract arguments with concrete
examples, flagging own biases, asking for clarification at the right moments,
reframing questions productively. Be specific — reference actual moments in the
conversation.]

### What could improve
[Be genuinely critical here. The user wants to get better, not be flattered.
Look for: repeated questions that could have been consolidated, tangents that
didn't serve the discussion, moments where a sharper prompt would have gotten
a better answer faster, missed opportunities to go deeper, times the user
accepted a weak answer without pushing back, or cases where the framing of
a question constrained the response unnecessarily. Also note if Claude gave
a mediocre response that the user didn't challenge — the user should learn
to catch those.]

### Prompting tips for next time
[1-3 concrete, actionable suggestions tailored to patterns observed in this
specific conversation. Not generic advice — things like "when you notice Claude
giving a too-neat framework, say X" or "you tend to ask broad questions first
and narrow later; try leading with the specific case next time."]

## Bibliography

[List any sources explicitly cited during the discussion. Then add 2-5 high-quality
further reading/listening/watching suggestions that are genuinely relevant — books,
papers, talks, articles, podcasts. Prioritize quality over quantity. Include a one-line
description of why each is relevant.

If nothing was cited and no further reading is clearly relevant, write:
"No sources were cited and no further reading is recommended for this discussion."
Do not pad this section.]
```

## Tone and approach

- Be direct and honest, especially in the critique section. The point is critical feedback, not encouragement. If the prompting was excellent, say so briefly and move on. If there were clear mistakes, name them.
- Write the summary as a narrative, not a list. Show the logical arc of the discussion so the reader can follow how the thinking developed.
- Key insights should be genuinely distilled — not just "we talked about X" but "the important thing about X is Y because Z."
- The bibliography should include real, specific sources. Do not fabricate titles or authors. If using web search to find relevant further reading, do so. If nothing is clearly relevant, leave the section empty rather than padding it.
- The whole document should be something the user can find useful weeks later without remembering the full conversation.

## Important notes

- This skill is about the conversation that just happened. Read the full conversation history carefully before producing output.
- The prompting critique should reference specific moments, not generic advice. "You pushed back on the switching cost argument effectively" is good. "Try to be more specific in your questions" without context is not.
- If Claude performed poorly at points in the conversation, note that too — the critique isn't just about the user's prompting, it's about the quality of the exchange as a whole.
