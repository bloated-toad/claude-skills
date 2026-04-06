---
name: thrift-store-id
description: Identify and value thrift store finds from photos. Use this skill whenever the user uploads 1-6 photos of an item (especially in a thrift store, estate sale, or secondhand context) and wants to know what it is, what it's worth, or whether to buy it. Trigger on phrases like "what is this", "what's this worth", "should I buy this", "thrift find", "thrift store", "estate sale", "garage sale", "flea market", "Goodwill", "Salvation Army", "antique mall", "vintage mall", "vintage market", or any photo of an object with a price tag visible. Also trigger when the user uploads object photos with minimal or no text — assume identification and valuation intent.
---

# Thrift Store Item Identifier & Valuator

## Purpose

Fast, decisive identification and valuation of secondhand items from photos. Often used in-store on a phone with spotty connectivity — the initial response must be compact and glanceable. But once the ID lands, the user may want to dig deeper, and longer follow-ups are fine.

## Input

- 1–6 photos of the item (different angles, markings, labels, stamps, signatures)
- One photo may include a visible price tag. If so, incorporate the price into the verdict.
- The user may provide no text at all, or minimal text. That's fine — photos are the input.

## Behavior Rules

1. **No follow-up questions.** Ever. Make your best call from what you can see.
2. **Guess confidently, qualify honestly.** If you're 70% sure, say so and say why. Don't hedge into uselessness.
3. **Front-load the essentials.** The ID, confidence, and verdict should be glanceable on a phone screen — assume the user might be in-store with bad signal. But after those core fields, feel free to elaborate with context, history, or tips if they'd be useful.
4. **Search when useful.** If confidence on the ID is medium or higher, use web search to find recent market pricing. Prioritize eBay sold listings, resale marketplaces, and retail pricing.
5. **Warnings matter.** If there are common fakes, reproductions, or condition issues that affect value, flag them in one line.
6. **Broad appeal verdict.** The buy/pass decision isn't just about resale margins. Factor in rarity, design quality, usefulness, collectibility, personal appeal, and price together. A thing can be worth buying because it's a great deal to flip, or because it's a beautiful object you'd actually use, or because it's uncommon and interesting. Serve the collector, the home cook, the design nerd, and the reseller.

## Output Format

Use exactly this structure. Every field on its own line. Skip fields that don't apply.

```
ID: [What it is. Maker, era, material, pattern/model name if known.]
Confidence: [High / Medium / Low] ([1-3 word reason])
Warning: [Fakes, reproductions, damage to check, or other red flags. Omit if none.]
Pricing: [Retail $X (if currently sold new). Resale $X–$Y range. Thrift price $Z (if price tag visible).]
Verdict: [Buy / Pass / Maybe — short justification.]
```

### Field Details

**ID:** Be as specific as possible. "Blue ceramic vase" is bad. "Bitossi Rimini Blu ceramic vase, Italy, 1960s" is good. Include the country of origin if identifiable from markings.

**Confidence:** High = distinctive markings, logo, or unmistakable design. Medium = likely correct based on style/materials but no definitive marks. Low = educated guess. Put the key evidence in parentheses.

**Warning:** Only include if there's a real, practical concern. Common fakes, known reproductions, things to inspect before buying (hairline cracks in ceramics, replated silver, etc). Omit entirely if no meaningful warning exists.

**Pricing:**
- If the item is currently manufactured and sold at retail, include the retail/MSRP price first.
- Then resale/secondary market range (what used examples actually sell for).
- If a thrift store price tag is visible in the photos, include it last.
- Format: `Retail $80. Resale $40–$60. Thrift price $7.99.`
- If not currently sold at retail, just show resale range and thrift price.

**Verdict:** One of three words — Buy, Pass, or Maybe — followed by a short justification. Consider the full picture: price relative to value, rarity, design quality, usability, collectibility, and personal appeal. Resale potential is one valid factor but not the only one. If no price tag is visible, base the verdict on the item's overall desirability and note that price wasn't visible.

## Examples

### Example 1: Known retail item with price tag visible

```
ID: Peugeot Paris u'Select adjustable pepper mill, beechwood, made in France. Lifetime-guaranteed grinding mechanism, widely considered the gold standard for pepper mills.
Confidence: High (Peugeot lion logo stamped on base, distinctive u'Select knob profile)
Warning: Cheap plastic lookalikes circulate — confirm the grinding mechanism is steel, not ceramic or plastic. Check the knob turns smoothly through all grind settings.
Pricing: Retail $80. Resale ~$50. Thrift price $7.99.
Verdict: Buy. 90% off retail for a buy-it-for-life kitchen staple — the Peugeot mechanism is genuinely best-in-class. Also easy to flip if you don't need one.
```

### Example 2: Vintage item, medium confidence

```
ID: Anchor Hocking Fire-King jadeite D-handle mug, likely 1950s. Part of the Restaurant Ware line, one of the most collected jadeite pieces.
Confidence: Medium (color and D-handle shape match, but no visible markings in photos — flip it over and look for "Fire-King" stamp or "Oven Ware" embossing)
Warning: Reproductions made since 2000 by Mosser and others. Originals are heavier, have subtle mold seams, and a slightly more opaque green. Side-by-side the difference is obvious.
Pricing: Resale $15–$40 depending on condition and markings. Thrift price $2.99.
Verdict: Buy. Jadeite Fire-King is iconic midcentury kitchenware — great shelf piece or daily driver. At $3, even a reproduction is worth it for the look, and an original is a genuine collectible.
```

### Example 3: Unknown item, low confidence, no price visible

```
ID: Hand-carved wooden mask, possibly West African, unknown age. Style is broadly consistent with Ivory Coast Dan masks — smooth forehead, slit eyes, protruding lips.
Confidence: Low (no maker marks, no provenance indicators, could be tourist/airport art or genuine ceremonial piece — impossible to distinguish from photos alone)
Pricing: Resale $20–$200+ depending on authenticity and age. Genuine older pieces with documented provenance can exceed $500. No price tag visible.
Verdict: Maybe. High variance item — it's a striking decorative piece regardless of provenance. Worth it under $15 if you like the look, but don't pay collectible prices without hands-on authentication.
```