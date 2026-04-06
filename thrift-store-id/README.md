# Thrift Store ID

Identify and value thrift store finds from photos — fast enough to use standing in the aisle with your phone.

## The problem

You're at Goodwill staring at something that might be a $200 mid-century vase or a $3 reproduction. You snap a photo, send it to Claude, and get back... a cautious paragraph about how "it's difficult to determine exact value without more information" and a suggestion to consult an appraiser. Not helpful when you need a decision in the next 30 seconds.

There's a growing crop of dedicated apps for this — [ThriftAI](https://apps.apple.com/us/app/thriftai-profit-identifier/id6746565278), [WhatsitAI](https://play.google.com/store/apps/details?id=com.dreambit.whatsit.app), [Antique Identifier](https://www.antiqueidentifier.app/), and others. They're fine for what they are, but they're closed ecosystems focused primarily on resale profit. They'll tell you the flip margin but not why a piece is interesting, and they require yet another app on your phone. If you're already using Claude as your daily AI, you shouldn't need a separate app just to ID a pepper mill.

## What this skill does

Upload 1–6 photos of an item and get back a structured identification with no follow-up questions asked:

- **ID** — as specific as possible. Not "blue ceramic vase" but "Bitossi Rimini Blu ceramic vase, Italy, 1960s."
- **Confidence** — High, Medium, or Low, with the key evidence in parentheses.
- **Warning** — common fakes, reproductions, or condition issues to watch for. Only present when there's something real to flag.
- **Pricing** — retail (if still manufactured), resale range from actual market data, and the thrift price if a tag is visible in the photo.
- **Verdict** — Buy, Pass, or Maybe, with a short justification that considers the full picture: not just resale margins, but rarity, design quality, usability, and collectibility.

The skill uses web search to pull current market pricing when it has a medium-or-higher confidence ID, so the numbers reflect what things are actually selling for, not what Claude vaguely remembers from training data.

## What makes this different

**It works inside Claude.** No new app to install, no account to create, no subscription. If you're already talking to Claude on your phone, you just send photos. That matters when you're in a store with one hand holding a teapot.

**The verdict isn't just about flipping.** Most thrift ID tools are built for resellers and optimize for profit margin. This skill serves a broader audience — the home cook who wants to know if that's a real Le Creuset, the design nerd who collects mid-century ceramics, the person who just thinks the thing looks cool and wants to know if $8 is reasonable. The verdict weighs rarity, craftsmanship, usefulness, and personal appeal alongside resale value.

**No follow-up questions.** The skill is explicitly designed to make its best call from whatever photos you provide. It won't ask you to "flip it over and check the bottom" — it'll tell you what it sees, how confident it is, and what to look for if you want to confirm. This is a deliberate design choice for the in-store use case where speed matters.

**Calibrated confidence, not false certainty.** Instead of always claiming 95% accuracy, the skill uses a three-tier confidence system with explicit reasoning. A "Medium" confidence ID with "(color and shape match, no visible markings)" is more useful than a confident wrong answer, because it tells you exactly what would resolve the uncertainty.

**It's a text file you can read and modify.** Unlike a compiled app, the entire skill is a single markdown file. You can read the prompting strategy, adjust the output format, change the verdict criteria, or add domain-specific knowledge for categories you care about.

## Installation

Copy the `SKILL.md` file into your Claude skills directory:

```
.claude/skills/thrift-store-id/SKILL.md
```

Then upload photos of any item — no trigger phrase needed, though "what is this", "what's this worth", and "should I buy this" all work.

## Examples of good inputs

- A photo of something interesting at a thrift store, estate sale, flea market, or antique mall
- Multiple angles of the same item (front, back, bottom, markings, labels)
- A photo with a visible price tag — the skill will factor the price into its verdict
- Just a photo with no text at all — the skill assumes identification intent

## Pairs well with

If you're a regular thrift shopper, consider tracking your finds over time. The skill produces structured output that's easy to log — item, confidence, price paid, estimated value. Over time you'll build a record of your hit rate and develop better instincts for what's worth picking up.
