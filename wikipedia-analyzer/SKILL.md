---
name: wikipedia-analyzer
description: Analyze and critique a Wikipedia article for an aspiring editor. Use this skill whenever the user asks to analyze, critique, review, or evaluate a Wikipedia article, or says "analyze the Wikipedia article for [topic]", or pastes a Wikipedia URL and wants feedback. Also trigger when the user mentions Wikipedia editing, Wikipedia article quality, or asks what they could improve on a Wikipedia page. Covers any request involving Wikipedia article assessment, editing opportunities, or learning from Wikipedia's best (or worst) examples.
---

# Wikipedia Article Analyzer

Analyze a Wikipedia article and produce an editorial assessment tailored to an aspiring Wikipedia editor. The goal is to help them find actionable editing opportunities or, if the article is already strong, learn what makes it work.

## Inputs

The user will provide either:
1. A URL to a Wikipedia article (e.g., `https://en.wikipedia.org/wiki/Example`)
2. A phrase like "analyze the Wikipedia article for [topic]"

If the user provides a topic name without a URL, search for it. If the article doesn't exist or is a disambiguation page, tell the user and ask for clarification.

## Fetching the article

Wikipedia content is primarily accessible through `web_search` and `web_fetch`. However, `web_fetch` returns a text-only extraction that **strips images, infoboxes, sidebars, and other non-text elements**. This is a critical limitation — you must compensate for it.

### Step 1: Get the article text
1. If a URL is provided, use `web_fetch` on that URL to get the article text.
2. If a topic is provided, use `web_search` with the query `[topic] site:en.wikipedia.org` to find the correct article, then `web_fetch` the top result.

### Step 2: Get the Talk page
Extract the article title from the URL (the part after `/wiki/`) and fetch the Talk page at `https://en.wikipedia.org/wiki/Talk:[Article_Title]`. The Talk page often contains WikiProject banners showing the article's official quality class (FA, GA, B, C, Start, Stub) and importance rating. If `web_fetch` fails on the Talk page URL, try `web_search` with `Talk:[Article_Title] site:en.wikipedia.org` as a fallback.

### Step 3: Detect images, infoboxes, and media (IMPORTANT)
Since `web_fetch` strips non-text content, you cannot rely on it to tell you whether an article has images, infoboxes, tables, or sidebars. To detect these elements:
- Use `web_search` for `"[Article_Title]" wikipedia infobox OR image OR file` to find references to the article's media.
- Look for image captions or file references that sometimes survive in the `web_fetch` text output (they occasionally appear as orphaned caption text).
- If the Claude in Chrome browser tools are available (check for `read_page` or `navigate` tools), prefer using them: navigate to the article URL and use `read_page` to get the full accessibility tree, which will include images, infoboxes, and all sidebar content. This gives a much more complete picture of the article.
- If no browser tools are available, **explicitly acknowledge this limitation** in the analysis. Say something like: "Note: My text-based extraction may not capture all images, infoboxes, or tables present in the article. Please check the article directly for media content before acting on suggestions about adding images."

When assessing the Media and Formatting dimension, be honest about what you can and cannot see. Never claim an article has no images unless you have confirmed this through browser tools or other reliable means.

## Assessment dimensions

Evaluate the article across these dimensions. Not every dimension will be relevant to every article — use judgment about which ones matter most and weight your analysis accordingly.

### 1. Article class and scope
What quality class does this article appear to be? Use Wikipedia's own scale as your anchor:
- **FA (Featured Article)**: Comprehensive, well-written, well-sourced, stable, follows style guidelines, includes media. The best Wikipedia has to offer.
- **GA (Good Article)**: Broad coverage, well-written, adequately sourced, neutral, stable. Not quite featured-level but solid.
- **B-class**: Mostly complete, reasonably well-written, some sourcing gaps or structural issues.
- **C-class**: Substantial but has clear gaps in content, sourcing, or writing quality.
- **Start-class**: A basic article with meaningful content but major gaps.
- **Stub**: A very short article providing only minimal information.

If the Talk page revealed an official rating, note it and say whether you agree or disagree (and why).

### 2. Lead section
The lead should summarize the entire article and stand alone as a concise overview. Evaluate:
- Does it summarize all major sections of the article?
- Is it proportional to the article length? (1-4 paragraphs depending on article size)
- Does it avoid introducing information not covered in the body?
- Is it accessible to a general reader?

### 3. Structure and organization
- Are the sections logically ordered?
- Are there sections that are disproportionately long or short?
- Does the article follow the expected structure for its topic type? (Biographies, cities, scientific topics, events, etc. each have conventional structures on Wikipedia.)
- Are there redundancies across sections?

### 4. Sourcing and citations
This is often the highest-impact area for editors. Evaluate:
- Are claims supported by inline citations?
- Are there unsourced statements that need citations? (Flag any contentious claims without sources.)
- What's the source quality? Are sources reliable (academic journals, major news outlets, official publications) or weak (personal blogs, press releases, self-published)?
- Are there any dead links visible?
- Is there over-reliance on a single source?
- Are there any citation-needed tags already in the article?

### 5. Neutrality (NPOV)
- Does the article maintain a neutral point of view?
- Is there promotional or puffery language? (Common in articles about companies, living people, and products.)
- Are multiple perspectives represented for controversial topics?
- Is the tone encyclopedic or does it read like an essay, advertisement, or fan page?

### 6. Prose quality
- Is it clearly written and accessible?
- Are there issues with grammar, awkward phrasing, or jargon without explanation?
- Is the writing overly dense or overly simplistic for the topic?
- Does it use weasel words ("some people say", "it is widely believed")?
- Is there excessive use of passive voice?

### 7. Completeness
- Are there obvious subtopics or aspects that are missing?
- Compare mentally to what a good encyclopedia entry on this topic would cover. What's absent?
- Are there entire time periods, perspectives, or facets skipped?

### 8. Media and formatting
- Does the article include relevant images, diagrams, tables, or infoboxes?
- Are images properly captioned and relevant?
- Is the infobox complete and accurate (if one exists)?
- Could the article benefit from a map, timeline, chart, or other visual element?
- Are there formatting issues (orphaned text, broken templates, etc.)?

### 9. Wikilinks and navigation
- Does the article link to relevant other Wikipedia articles?
- Are there too many or too few wikilinks?
- Are the "See also" and "External links" sections useful and not redundant with inline links?
- Does the article have appropriate categories?

## Scoring: Improvement Opportunity

After evaluating the dimensions above, assign the article an **Improvement Opportunity** label. This is NOT a quality score — it's a measure of how much actionable room there is for an aspiring editor to contribute.

- **Gold Mine**: Major editing opportunities everywhere. The article has significant gaps, sourcing problems, structural issues, or neutrality concerns. An editor could make a big impact here. (Typical of Stub and Start-class articles, or neglected C-class articles.)
- **Fertile Ground**: Several meaningful improvements are available. The article has a decent foundation but clear areas that need work. Good for an editor looking to make substantive contributions without starting from scratch. (Typical of C-class and lower B-class articles.)
- **Polish Needed**: The article is generally solid but has specific areas that could be tightened — sourcing gaps, prose that could be clearer, a section that needs expansion, or formatting improvements. Good for editors building their skills on less risky edits. (Typical of upper B-class and some GA articles.)
- **Learning Example**: The article is high quality with little low-hanging fruit. An aspiring editor should study this article to understand what good looks like. Minor tweaks might be possible, but the main value is educational. (Typical of GA and FA articles.)

## Tailoring the output

The output tone and emphasis should shift based on the Improvement Opportunity label:

**For Gold Mine and Fertile Ground articles:**
- Lead with the most impactful editing opportunities. Be specific: "The 'History' section has no citations — adding sources from [type of source] would be a high-impact edit."
- Prioritize edits by impact and difficulty. What could a newer editor tackle first?
- Don't pile on criticism of existing content unless it's actively wrong or harmful. For a stub, the problem is absence, not badness.
- Frame everything as opportunity: "This article doesn't yet cover X, which means an editor who adds that section would be making a significant contribution."

**For Polish Needed articles:**
- Mix specific improvement suggestions with notes on what the article does well.
- Point out patterns the editor can learn from — "Notice how this article uses inline citations after every statistical claim — that's the standard to aim for."
- Suggest edits that are achievable but teach good habits (improving a lead section, adding an image with proper licensing, etc.).

**For Learning Example articles:**
- Lead with what makes the article strong. Walk through the dimensions and explain what's working and why.
- Use the article to illustrate Wikipedia best practices: "The lead here is a model — it summarizes every major section in four paragraphs, opens with a clear definition, and avoids jargon."
- If you spot minor improvements, frame them gently and note that at this level, edits often involve editorial judgment calls rather than clear fixes.
- Suggest related articles that might benefit from the same treatment — "The article on [related topic] is much weaker; an editor who understands what makes this article work could improve that one significantly."

## Response structure

Use this structure in the chat response:

### Article: [Title]
**URL**: [link]
**Estimated class**: [Your assessment] | **Official class** (if found on Talk page): [rating]
**Improvement Opportunity**: [Gold Mine / Fertile Ground / Polish Needed / Learning Example]

Then deliver the analysis following the tailoring guidance above. Don't mechanically walk through all 9 dimensions — lead with what matters most for this article and its quality level. Group related observations. Keep the total response focused and actionable, not exhaustive.

End with a **Suggested first edit** section: one specific, concrete editing action the user could take on this article right now. Make it achievable for a newer editor. If the article is a Learning Example, instead end with **What to study here** — 2-3 specific things to pay attention to that illustrate Wikipedia editing best practices.

## Research process

1. Fetch the article content via `web_fetch` on the article URL
2. Fetch the Talk page at `https://en.wikipedia.org/wiki/Talk:[Article_Title]` for official quality ratings. If `web_fetch` fails, fall back to `web_search` for `Talk:[Article_Title] site:en.wikipedia.org`
3. If browser tools (Claude in Chrome) are available, navigate to the article and use `read_page` to examine the full page structure including images, infoboxes, and sidebars
4. If browser tools are NOT available, note this limitation explicitly when discussing media/formatting — do not assume the article lacks images just because `web_fetch` didn't return them
5. If relevant, do a quick `web_search` for the topic to understand whether the article's coverage matches what reliable sources say (helps assess completeness and neutrality)
6. Use at least 3-4 tool calls total. More for complex or controversial topics.

## Important principles

- **Be encouraging, not gatekeeping.** The audience is someone who wants to start editing Wikipedia. Frame everything as opportunity and learning, not as criticism of whoever wrote the current article.
- **Be specific.** "This article needs more sources" is useless. "The third paragraph of the History section makes three unsourced claims about the 1990s expansion — finding newspaper coverage from that era would strengthen this section" is useful.
- **Respect Wikipedia's norms.** Reference actual Wikipedia policies and guidelines by name when relevant (WP:NPOV, WP:RS, WP:MOS, WP:LEAD, etc.) so the user learns the vocabulary.
- **Calibrate your expectations.** Not every article needs to be Featured. A well-sourced Start-class article on an obscure topic is a genuine contribution. Don't suggest aspirational improvements that don't match the topic's realistic potential.
- **Acknowledge uncertainty.** If you're unsure whether a source is reliable or whether a section is biased, say so. Wikipedia editing involves a lot of judgment calls.
