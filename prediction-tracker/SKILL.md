---
name: prediction-tracker
description: >
  Extract, record, update, and review probabilistic predictions from conversations. Use this skill
  whenever the user says "track this prediction", "log my predictions", "what did I predict",
  "prediction tracker", "check my calibration", "review my predictions", "how did I do",
  "prediction review", "let's score my predictions", "update my prediction", or "revise my
  number." Also trigger when the user says "let's wrap up" or "wrap-up" at the end of a
  forecast conversation — offer to extract predictions before wrapping up. Trigger when the
  user says "record that" or "save that" after stating a probability. Trigger when the user
  shares news relevant to an open prediction and wants to update their probability. If the
  user has been assigning probabilities or making claims about the future in conversation
  and says something like "hold me to that" or "write that down" or "I should track this",
  use this skill. Also trigger on "what are my open predictions" or "what's coming due."
  Err toward triggering.
---

# Prediction Tracker

You're helping someone build forecasting as a skill by maintaining a persistent log of their
predictions. The goal is twofold: (1) force vague hunches into explicit, scorable claims, and
(2) create a review loop where the user confronts their track record honestly.

## Three modes

This skill operates in three modes: **Record**, **Update**, and **Review**. Figure out which
one the user wants based on context, or ask if it's ambiguous.

### Record mode

The user has been making predictions or assigning probabilities in conversation. Your job is
to extract those into well-formed prediction records and save them.

#### Step 1: Extract predictions from the conversation

Scan the current conversation for anything that looks like a prediction. This includes:
- Explicit probability assignments ("I'd put X at 60%")
- Scenario forecasts ("I think Scenario B is most likely")
- Implicit predictions ("I don't think GPT-6 ships this year")
- Conditional claims ("If DeepSeek V4 delivers, open-source catches up within 6 months")
- Agreement or disagreement with Claude's forecasts ("Your 35% on Scenario B feels right" — that's the user adopting 35%)

Present what you found back to the user in plain language. Ask if you missed any or got
any wrong.

#### Step 2: Force precision

For each prediction, you need four things. If any are missing, ask the user directly.
Don't let them off the hook — the whole point is to be explicit.

1. **Claim**: A clear, unambiguous statement that will eventually resolve YES or NO
   (or a numeric value for range predictions). Rewrite vague claims into precise ones.
   - Bad: "AI will keep getting better"
   - Good: "At least one frontier model released before Jan 1 2027 will score >85% on
     SWE-bench Verified"
   - If the user resists precision, explain why it matters: "If we can't agree later on
     whether this came true, the prediction isn't useful for calibration."

2. **Probability**: A number between 1% and 99%. 
   - If the user says "probably" or "likely," push: "What number would you put on that? 
     60%? 75%? Just a gut number."
   - If they give a range ("somewhere between 40-60%"), ask them to pick a point estimate.
   - Note: 50% is fine and honest. Don't pressure them away from it.

3. **Resolution date**: When can this be checked? Must be a specific date.
   - If the prediction is about an event ("GPT-6 releases"), set a deadline by which
     it should have happened. "By when? End of 2026? End of 2027?"
   - Default to the timescale of the forecast conversation if one was established.

4. **Resolution criteria**: How will we know if this came true? What source or evidence
   counts?
   - For model releases: official company announcement or API availability
   - For benchmark scores: published results from the model developer or independent eval
   - For geopolitical events: reporting from major wire services
   - For personal predictions: the user's own judgment at resolution time
   - Keep it simple. One sentence is usually enough.

#### Step 3: Confirm and save

Present the formatted predictions back to the user for confirmation. Then append them to
the prediction log.

**File location**: Use the path the user specifies for their prediction log. If they
haven't specified one, default to a file named `prediction-log.md` in their Documents
folder. If you don't know what exact path to use in the current environment, ask the
user where they want the file stored before writing.

Use the Filesystem MCP tools (`mcp__filesystem__read_file`, `mcp__filesystem__write_file`) to read and write this file — it lives on the user's computer, not Claude's container filesystem. If the file does not exist yet, create it using the format below. Do not save to downloads unless the user explicitly asks. The file format is:

```markdown
# Prediction Log

## Open Predictions

### [YYYY-MM-DD] — [Short title]
- **Claim**: [precise claim]
- **Probability**: [X%]
- **Resolution date**: [YYYY-MM-DD]
- **Resolution criteria**: [how we'll know]
- **Context**: [1-2 sentences on what conversation or reasoning produced this]
- **Tags**: [topic tags for filtering, e.g., #ai #geopolitics #personal]
- **Updates**:
  - [none yet]

---

## Resolved Predictions

### [YYYY-MM-DD] — [Short title] ✅ / ❌
- **Claim**: [precise claim]
- **Initial probability**: [X%]
- **Final probability**: [Y% — same as initial if never updated]
- **Resolution date**: [YYYY-MM-DD]
- **Outcome**: [YES / NO / AMBIGUOUS]
- **Brier score (initial)**: [calculated against initial probability]
- **Brier score (final)**: [calculated against final probability]
- **Updates**:
  - [copied from the open prediction, or "none" if never updated]
- **Retrospective**: [what the user got right/wrong in their model, what assumptions
  broke, what biases were revealed, what they'd do differently — this is the most
  valuable field in the log]
- **Notes**: [any additional context about what happened]

---

## Calibration Summary

[Updated when predictions are resolved. See Review mode.]
```

When appending predictions, read the existing file first so you don't overwrite anything.
New predictions go at the top of the "Open Predictions" section (newest first).

**Backward compatibility**: Older predictions in the log may not have an **Updates** field.
When updating or resolving these, add the field at that time. When resolving a prediction
without updates, set Initial probability = Final probability and note "no updates" in the
Updates field. Don't rewrite the entire file to add the field retroactively — add it
per-prediction as they get touched.

#### Step 4: Nudge toward a review date

After saving, suggest when to check back. Be concrete:
- "Your GPT-6 prediction resolves end of 2026. Want me to remind you to check in around
  October when we might have early signals?"
- "You've got three predictions resolving in Q3. Maybe do a batch review in July?"

The skill can't set actual reminders, but it can note review dates in the log file.
Add a "Suggested review" line to each prediction if the user agrees.

If the user has calendar tools available, offer to create a calendar event for
the review date.

### Update mode

The user has new information about an existing prediction and wants to revise their
probability. This is triggered by:
- The user sharing news relevant to an open prediction ("did you see that Trump paused the strikes?")
- The user explicitly saying "update my prediction" or "I want to revise my number"
- A conversation naturally surfacing information that bears on an open prediction
- The user saying "I'd update X down/up to Y%"

#### Step 1: Identify the prediction

Load the prediction log and figure out which prediction the user is updating. If
ambiguous, show them their open predictions and ask which one.

#### Step 2: Get the new probability and rationale

You need two things:
1. **New probability**: A revised number. Push for precision just like in Record mode.
2. **Rationale**: A brief explanation of *why* the number changed. This should reference
   the specific new information or reasoning that drove the update. Keep it to 1-2
   sentences — enough to reconstruct the logic later, not a full essay.

Good rationale examples:
- "80% → 8%: Trump announced 5-day postponement citing 'productive talks' with Iran"
- "30% → 45%: New polling shows Newsom leading in early primary states by 8pts"
- "70% → 70%: Reviewed new info but it doesn't change my estimate" (this is valid and
  worth recording — it shows the user actively maintaining a position rather than ignoring it)

#### Step 3: Write the update to the log

Read the prediction log, find the prediction, and append a datestamped update line to
its **Updates** section. The format is:

```
- **Updates**:
  - 2026-03-23: 80% → 8%. Trump announced 5-day postponement of strikes, citing
    "productive talks." Off-ramp model dominated commitment-trap model.
```

If this is the first update, replace `- [none yet]` with the update line.

If there are already updates, append the new one below the existing ones (chronological
order).

Also update the top-level **Probability** field to reflect the current number, so the
log always shows the latest estimate at a glance. The update trail preserves the history.

#### Step 4: Acknowledge and contextualize

After saving, briefly note:
- The size of the update (big swing vs. minor adjustment)
- Whether the update moves toward or against the market/consensus if known
- Any suggestions for further updates to watch for ("if the five-day talks collapse,
  we'd want to update back up")

Don't belabor this — the user is building a habit, and friction kills habits.

### Review mode

The user wants to check their track record. This might be triggered by:
- A specific prediction coming due ("did GPT-6 ship?")
- A general calibration check ("how am I doing?")
- The user returning to a topic they previously forecasted

#### Step 1: Load the log

Read the prediction log using `mcp__filesystem__read_file` at the previously established
log path. If no path has been established in the current conversation, use the default
location described above or ask the user where they want the file stored if the exact
path is unclear in the current environment.
If it doesn't exist, tell the user there's nothing to review yet and offer to start recording.

#### Step 2: Identify what's ready to resolve

Check which open predictions have passed their resolution date. Also flag any that are
approaching their date (within 30 days) — these are worth discussing even if not yet due.

For predictions approaching their date, search the web for current information to help
assess whether the prediction is tracking toward YES or NO.

#### Step 3: Resolve predictions

For each prediction that's due:
1. Search for current information on the claim
2. Present what you found
3. Ask the user: "Does this resolve YES, NO, or is it ambiguous?"
4. If ambiguous, discuss and try to reach a judgment. If genuinely unresolvable,
   mark it AMBIGUOUS and note why.
5. Calculate **two** Brier scores:
   - **Brier score (initial)**: (initial_probability/100 - outcome)² — measures
     the quality of the original forecast
   - **Brier score (final)**: (final_probability/100 - outcome)² — measures the
     quality after updates. If the prediction was never updated, these are the same.
   - The gap between initial and final Brier scores measures **update quality** —
     did the user improve their forecast when new information arrived?
   - Lower is better. 0 = perfect, 1 = worst possible. 0.25 = coin flip baseline.
6. **Write the retrospective.** This is the most valuable part of the review. Ask the
   user (or draft one together) covering:
   - What assumptions in the original model were wrong?
   - What information would have changed the initial estimate?
   - What systematic biases does this reveal? (overconfidence? anchoring to
     commitment-trap models? underweighting off-ramps?)
   - What would you do differently next time with a structurally similar question?
   The retrospective should be 2-4 sentences — enough to be useful when reviewed
   later, not a full essay. Write it in plain language, not jargon.
7. Move the prediction from Open to Resolved in the log file, using the Resolved
   format (which includes Initial probability, Final probability, Updates trail,
   both Brier scores, and the Retrospective).

#### Step 4: Calibration analysis

After resolving predictions, if there are at least 5 resolved predictions, calculate
and present:
- **Average Brier score (initial)**: How good were the first estimates?
- **Average Brier score (final)**: How good were the estimates after updates?
- **Update quality**: The average improvement from initial to final Brier score.
  Positive = updates made things better. Negative = updates made things worse.
  This is arguably the most important metric — it measures whether the user is
  learning from new information or just adding noise.
- **Calibration by bucket**: Group predictions by probability range (e.g., 60-69%, 70-79%)
  and show what percentage actually came true. Good calibration means your 70% predictions
  come true about 70% of the time. Use final probabilities for this.
- **Bias check**: Are you systematically overconfident or underconfident? If your average
  assigned probability for YES outcomes is much lower than 100%, you might be underconfident.
  If your NO outcomes had high probabilities, you're overconfident.
- **Common retrospective themes**: If there are enough resolved predictions, look for
  recurring patterns in the retrospectives. "You've noted overweighting commitment traps
  in three separate predictions" is actionable feedback.

Present this conversationally, not as a data dump. Highlight the most interesting
or instructive patterns.

With fewer than 5 resolved predictions, just show the Brier scores individually and
note that it's too early for calibration analysis.

#### Step 5: Update the summary

Update the Calibration Summary section at the bottom of the log with the latest numbers.
Include the date of the review.

## Important principles

- **Never let the user record a prediction without a probability.** That's the whole
  point. "I think X will happen" is not a prediction. "I think X will happen, 70%" is.
  Push gently but firmly.

- **Respect the user's numbers.** Don't tell them their probability is wrong. You can
  share your own estimate for comparison, and you can point out potential biases, but
  their number goes in the log.

- **Be honest during review.** If they got something badly wrong, say so plainly.
  If they got something right, acknowledge it. Don't soften bad calibration —
  that defeats the purpose.

- **Encourage updates.** The update trail is where the real learning happens. When
  a conversation surfaces information relevant to an open prediction, proactively
  ask: "Should we update your number on [prediction]?" Updates that don't change
  the number are still worth recording — they show active maintenance of a position.

- **This pairs with the forecast skill.** If the user just had a forecast conversation,
  there are probably predictions embedded in it. Offer to extract them. If the wrap-up
  skill is triggered after a forecast conversation, suggest running prediction-tracker
  first.

- **Encourage volume.** Calibration only works with enough data points. Encourage
  the user to record predictions even on small things — not just big geopolitical
  forecasts. "Will this restaurant still be open next time I visit?" is a valid
  prediction that builds the calibration habit.

## Tone

Same as the forecast skill: conversational, direct, respectful. You're a training partner,
not a judge. The user is building a skill, and the log is their training journal.
