# Ingredient Riff

A Claude skill for figuring out dinner when you're standing in the kitchen with ingredients and no plan. Tell it what you have, get three options, pick one, get a complete recipe.

## The problem

Asking an LLM "what should I make with chicken thighs and zucchini" gets you a generic recipe dump — ten suggestions at the same level of enthusiasm, no read on what's actually the best move, and ingredient lists that assume you have a fully stocked pantry. Dedicated AI recipe apps like ChefGPT, DishGen, and SuperCook solve the ingredient-matching part, but they're optimized for browsing a database, not for a quick back-and-forth that lands on one thing you're about to cook.

## What this skill does

Ingredient Riff runs a focused four-step conversation:

1. You say what you have (the anchor ingredients driving tonight's meal).
2. It presents exactly three options — a no-brainer you can make right now, an upgrade that hinges on one or two ingredients you might have, and a wildcard from a different culinary tradition or technique.
3. You pick one (or add more ingredients and it refreshes).
4. It delivers a complete, explicit recipe with real measurements, sensory doneness cues, and steps sequenced for how cooking actually works ("start the rice first so it's ready when the stir-fry is done").

## What makes this different

**It's opinionated, not encyclopedic.** Recipe apps give you a wall of options and let you scroll. This skill picks three and tells you which one it thinks is best. The wildcard slot exists specifically to suggest something you wouldn't have googled.

**The recipe phase is thorough.** Most AI cooking responses are vague — "season to taste," "cook until done." This skill produces recipes with actual quantities, time-aware sequencing, and sensory cues (color, sound, smell) instead of just timers.

**It's customizable to your kitchen.** A single block at the top of the skill defines your pantry staples, gear, and serving size. Edit that block once and every recipe reflects your actual setup — your actual spice rack, your actual equipment, your actual household size.

**It's for right now, not meal planning.** No weekly schedules, no grocery optimization, no nutrition tracking. You're hungry, you have stuff, let's go.

## Installation

Copy the `SKILL.md` file into your Claude skills directory:

```
.claude/skills/ingredient-riff/SKILL.md
```

Then open the skill and edit the "Your kitchen" section at the top to match your pantry, gear, and serving size. The defaults are deliberately minimal — the skill works better when it knows what you actually have.

## Examples of good inputs

- "I have salmon fillets and a bunch of kale"
- "What should I make with sweet potatoes and black beans? I'm passing by the store on the way home"
- "I need to use up these mushrooms and half a block of tofu"
- A photo of produce you just picked up
- "What's for dinner — I've got ground pork and cabbage"
