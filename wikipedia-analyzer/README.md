# Wikipedia Analyzer — Claude Skill

A custom Claude skill that analyzes Wikipedia articles and produces editorial assessments tailored to aspiring Wikipedia editors.

## What it does

Give it any Wikipedia article (by URL or topic name) and it returns a structured assessment covering:

- **Quality classification** — estimates the article's class (FA → Stub) using Wikipedia's own scale, cross-referenced against the article's Talk page for official ratings
- **Improvement Opportunity score** — not a quality grade, but a measure of how much room there is for an editor to contribute (Gold Mine / Fertile Ground / Polish Needed / Learning Example)
- **Dimensional analysis** across 9 areas: lead section, structure, sourcing, neutrality, prose quality, completeness, media/formatting, wikilinks, and article scope
- **A concrete suggested first edit** — one specific, achievable action a newer editor could take right now

## Design philosophy

The skill is built around a few key ideas:

1. **Opportunity-first framing.** A weak article isn't a failure — it's a chance to contribute. The tone shifts based on article quality: Gold Mine articles get an excited "here's what you can build" treatment; Learning Example articles walk through what makes them work so editors can internalize good patterns.

2. **Specificity over generality.** "Needs more sources" is banned. The skill pushes for things like "The third paragraph of the History section makes three unsourced claims about the 1990s expansion — newspaper archives from that era would be the best source type."

3. **Wikipedia-native vocabulary.** References actual policies (WP:NPOV, WP:RS, WP:MOS, WP:LEAD, etc.) so users learn the norms as they go.

4. **Honest about limitations.** The skill explicitly acknowledges that `web_fetch` strips images, infoboxes, and tables — and adjusts its media/formatting assessment accordingly rather than making false claims about missing content.

## How to use it

Drop `SKILL.md` into your Claude skills directory. Then trigger it with:

- "Analyze the Wikipedia article for [topic]"
- Paste a Wikipedia URL and ask for feedback
- "What could I improve on the Wikipedia page about [topic]?"
- "Review this Wikipedia article: https://en.wikipedia.org/wiki/..."

## Research process

Each analysis involves 3–6+ tool calls:

1. Fetch article text via `web_fetch`
2. Fetch the Talk page for official quality ratings
3. (If available) Use Claude in Chrome browser tools for full page inspection including images and infoboxes
4. (If needed) Web search the topic to cross-check completeness and neutrality
5. Synthesize into a focused, actionable assessment

## Example output structure

```
### Article: [Title]
**URL**: [link]
**Estimated class**: B-class | **Official class**: C-class
**Improvement Opportunity**: Fertile Ground

[Analysis grouped by what matters most, not a mechanical walkthrough of all 9 dimensions]

### Suggested first edit
[One specific, concrete, achievable action]
```

## Requirements

- Claude with web search and web fetch capabilities
- Optional: Claude in Chrome browser tools for richer media detection

## Further reading

If you're interested in Wikipedia editing more broadly, these are worth knowing about:

- **Wikipedia's own [Your First Article](https://en.wikipedia.org/wiki/Wikipedia:Your_first_article)** tutorial — the official starting point for new editors
- **Wikipedia's [Featured Article criteria](https://en.wikipedia.org/wiki/Wikipedia:Featured_article_criteria)** — understanding FA standards helps calibrate quality expectations
- **[WikiProject Council](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_Council)** — finding WikiProjects in your area of interest is one of the best ways to find articles that need work
