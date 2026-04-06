# Forecast

Turn any developing situation into a structured probabilistic forecast — with calibration training built in.

## The problem

Ask Claude "what's going to happen with X?" and you'll get a fluent summary of the situation followed by vague hedging. "It's difficult to predict... there are many factors at play... it could go either way." You learn nothing you didn't already know, and you leave without a single number you could hold yourself accountable to.

The existing forecasting skills in the ecosystem don't solve this either. They're mostly components — a premortem here, a Fermi estimation there, a bias checklist somewhere else. They assume you've already formed a view and just need to stress-test it, or they're built for quantitative domains like sales pipelines and financial models. None of them do the thing you actually want: take a developing situation you're curious about, research it from scratch, and produce a real forecast with explicit probabilities.

## What this skill does

You name a situation — a geopolitical crisis, a tech trend, a market shift, a personal decision, anything uncertain — and the skill runs a full forecasting workflow:

**Research first, always.** The skill mandates 3-5+ web searches before synthesizing anything. It searches the core topic, key actors, contrarian views, and prediction market odds. It doesn't forecast from vibes — it builds a ground-truth picture from current data, flags source quality explicitly, and highlights facts that contradict the mainstream narrative.

**Key variables, ranked.** Identifies the 3-7 factors that will most determine the outcome, ranked by impact × uncertainty. The things that matter most and that we're least sure about come first, because those are where thinking pays off.

**Named scenarios with explicit probabilities.** 3-5 scenarios that cover the plausible outcome space, each with a memorable name, a probability, a pathway (what sequence of events leads here), and early signals that would tell you it's becoming more likely. The probabilities must sum to roughly 100%. No weaseling out with "it depends."

**Calibration checkpoint.** This is the distinctive mechanic. After presenting scenarios, the skill pauses and asks the user to assign their own probability before comparing notes. If the numbers diverge, it explores why — collaboratively, not condescendingly. If a cognitive bias is driving the gap (availability bias, narrative bias, base rate neglect), it names the bias concretely with reference to the specific situation. The goal is to build the user's forecasting muscle over time, not just deliver a one-off prediction.

**Leading indicators and source recommendations.** Ends with specific, observable things that would update the forecast ("if X happens, scenario B jumps from 20% to 45%"), plus 3-5 named sources for ongoing monitoring — specific journalists, analysts, or feeds, not generic outlets. Includes prediction market links when available.

## What makes this different

**It does the research.** Most forecasting tools assume you already know what's happening and just need help structuring your thinking. This skill starts from scratch — it searches, reads, synthesizes, and flags source quality before forming any view. The research phase is mandatory, not optional.

**It produces actual numbers.** Named scenarios with explicit probabilities that sum to 100%, conditional on a stated time horizon. Not "it could go either way" — a real assessment you can revisit and score later.

**It trains your calibration.** The mid-conversation calibration checkpoint — where the skill pauses and asks for your number before sharing its own — is a genuinely interactive mechanic. It turns every forecast conversation into a small calibration exercise, building the habit of assigning probabilities and noticing when your intuition diverges from the evidence.

**It's a complete arc, not a component.** Existing skills cover individual steps: decompose a question, run a premortem, check for biases. This skill covers the full workflow in one conversation: research → ground truth → key variables → scenarios → calibration → indicators → sources. You walk away with a complete forecast, not a worksheet.

**It searches for contrarian views and prediction markets.** The research phase explicitly includes searching for skeptics, critics, and minority positions, plus checking prediction market odds when available. This counteracts the tendency to build a forecast from whatever narrative dominates the first page of search results.

**Epistemic hygiene is woven in, not bolted on.** Instead of a "bias check" section at the end, the skill flags uncertainty, names relevant biases, and distinguishes between knowing, inferring, and guessing throughout the conversation. It also challenges the user's framing when the question itself could be sharper.

## Installation

Copy the `SKILL.md` file into your Claude skills directory:

```
.claude/skills/forecast/SKILL.md
```

Then name any developing situation — "what's going to happen with [X]?" — or say "forecast", "what are the odds", "help me think through scenarios for [X]", or just drop a news article and ask what happens next.

## Examples of good inputs

- A developing geopolitical situation you want to reason about
- A technology trend where you're trying to assess timing or likelihood
- A market or industry shift you want to put probabilities on
- A personal decision with uncertain outcomes (career move, relocation, investment)
- A news article you just read where you want to think about what comes next
- A prediction market question you want an independent assessment of
- Revisiting a previous forecast to see how it held up and update your numbers

## Pairs well with

If you're building a forecasting practice, consider pairing this with a prediction tracking system. The forecast skill produces predictions; a tracker records them, scores them over time, and surfaces calibration patterns. The review loop — making forecasts, recording them, scoring them, and learning from the gaps — is where the real skill development happens.
