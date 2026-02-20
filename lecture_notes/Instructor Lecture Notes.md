# Instructor Lecture Notes
## Science, Frankenstein, and Causal Inference: Analyzing Neighborhood Crime with Synthetic Control Methods
**ICQCM Summit 2026 | February 27 | 10:15 am – 12:30 pm**

---

> **How to use these notes:** This is your run-of-show document. Each section maps directly to your slides and the RMarkdown. Italicized text in *[brackets]* are stage directions or transition cues. Bold text marks key points to land clearly with the audience.

---

## PRE-SESSION (Before 10:15 am)

**Room setup checklist:**
- Project your slides and confirm they display correctly
- Have `1000_synth.Rmd` open in RStudio on your laptop
- Write the GitHub URL on the board or share via chat: `https://github.com/YungChun0303/ICQCM`
- Ask participants to open RStudio and confirm packages are installed as they walk in

**If someone doesn't have R working:**
- Point them to share a screen with a neighbor — pair coding is fine
- Don't let one person's setup problem hold back the whole group

---

## BLOCK 1 — Why Causality? Why SCM? (0:00–0:35)

---

### [0:00–0:08] Welcome & Housekeeping
*[Slide 1 — Title slide]*

Open with energy. You're in New Orleans, it's a conference, keep it warm and informal.

Say something like:
> "Good morning — I'm Yung Chun from Washington University in St. Louis. For the next two hours we are going to do something a little unusual for a methodology seminar. We're going to talk about Frankenstein. And then we're going to build one — in R."

Housekeeping:
- Confirm the GitHub link is accessible — ask everyone to download the ZIP now if they haven't
- Confirm RStudio is open
- Mention: "We have a break built in. The first half is conceptual, the second half is hands-on coding. If anything is unclear in the first half, hold your questions and the coding section will often answer them visually."

---

### [0:08–0:18] Science and Causal Inference
*[Slides 2–4]*

**Slide 2 — "Science?"**
This is your tone-setter. The image and the question mark are intentional — you're inviting the audience to think, not just receive.

Ask the room: *"How many of you would call what you do 'science'?"* Let a few hands go up. Don't editorialize yet.

**Slide 3 — Nathan Chun quote**
Deliver the quote with a smile — it lands best if you own the humor of quoting your kid. The point is genuine though:
> "Science really is about cause and effect. A child playing Zelda gets this intuitively. The question is whether our research designs actually deliver on that promise."

**Slide 4 — Judea Pearl / The Book of Why**
This is the philosophical anchor of the whole seminar. Take your time here.

Key point to land:
> "Pearl argues that most of what we call 'AI' — and frankly, a lot of what we call 'quantitative social science' — lives on Rung 1. We find patterns. We correlate. But correlation is not causality, and your dissertation committee knows that, your reviewers know that, and your policy audiences increasingly know that."

Walk through the three rungs briefly:
- **Rung 1 (Association):** "Seeing" — what goes with what. Most regression lives here.
- **Rung 2 (Intervention):** "Doing" — what happens if I change something. Experiments live here.
- **Rung 3 (Counterfactuals):** "Imagining" — what *would have* happened. This is where SCM lives.

Transition: *"So how do we get to Rung 3 without a time machine or a randomized trial?"*

---

### [0:18–0:28] Causal Identification Methods
*[Slide 5 — Identification table]*

Walk across the three columns — RCT, Natural Experiment, Quasi-Experiment. Keep it brisk; this is review for most of the room.

Key emphasis:
> "Notice the last column — Quasi-Experiments. This is the practical toolkit for most of us who study policies, neighborhoods, schools, institutions. We can't randomize. We don't get lucky natural experiments very often. So we work with what we have — and that's where Difference-in-Differences, and ultimately SCM, live."

Point out that SCM is listed last in the quasi-experiment row — it's the newest and most demanding method in the family.

---

### [0:28–0:35] Why SCM? The Doppelgänger vs. Frankenstein
*[Slides 6–9]*

**Slide 6 — Quasi-experiment comparison table**
The table is the key teaching moment. Contrast the two columns:
> "In a conventional quasi-experiment — DiD — you go looking for a control group that already exists. You hope to find a neighborhood, a city, a school that happens to look like your treated unit. That's finding a Doppelgänger."

> "SCM says: what if no single Doppelgänger exists? What if your treated unit is genuinely unique? Then instead of searching for one, you *build* one. You take the best features from many different units and stitch them together. That's the Frankenstein."

**Slide 7 — Doppelgänger vs. Frankenstein image**
This is a visual rest stop — let the images land. You can be light here:
> "The Doppelgänger is hard to find. The Frankenstein is... well, it's what we're here to build today."

**Slide 9 — The problem of the unique unit**
Bring it back to the concrete case:
> "We're studying one census tract in Denver — Sun Valley. There is no other neighborhood in Denver that is a perfect match for Sun Valley's crime trajectory, its demographics, its history. DiD would fail here. SCM is designed precisely for this situation."

The three bullets map cleanly:
- **The comparative case study challenge** — one treated unit
- **The DiD failure** — parallel trends assumption breaks down
- **The SCM pivot** — from searching to optimizing

---

## BLOCK 2 — SCM Theory & Identification (0:35–1:00)

---

### [0:35–0:45] The Identification Strategy
*[Slides 10–13]*

**Slides 10 & 13 — "You may copy and paste this slide"**
These are your formal identification slides. Tell the audience:
> "This is the slide to screenshot for your papers and grant applications. We're going to unpack what it means now, and then you'll watch it happen live in R."

**Slide 12 — The SCM diagram**
This is the most important conceptual slide. Walk through it slowly, left to right.

The two weight systems — V and W:
- **V weights (covariate weights):** How much should each predictor variable matter when constructing the synthetic twin? Demographics? Income? Past crime rates?
- **W weights (unit weights):** How much of each donor neighborhood should go into the Frankenstein? Neighborhood A gets 40%, Neighborhood B gets 35%, etc.

Key insight to convey:
> "The algorithm is solving two problems simultaneously — what to match on, and who to match from. The result is a single synthetic trajectory that mirrors the treated unit as closely as possible before the intervention."

Then point to the right side:
> "After treatment starts — 2016 Q3 in our case — we stop updating. The synthetic control keeps going on its own trajectory. The gap between what we actually observed and what the Frankenstein predicts is our treatment effect."

---

### [0:45–0:55] Placebo Tests and Inference
*[Slide 14]*

Acknowledge the elephant in the room:
> "You're probably thinking: this is interesting, but how do I know the gap isn't just noise? How do I get a p-value? This is where SCM gets creative."

Walk through the two placebo types:

**In-time placebo:**
> "We move the fake treatment date backward — say, to 2013 — and re-run the model. If SCM finds a big 'effect' before the grant even happened, that's a red flag. The model is picking up something other than the intervention."

**In-space placebo (the main one):**
> "We re-run the exact same model for every single donor neighborhood, pretending each one received the grant. None of them did — so we'd expect small post-treatment gaps for all of them. If the real CNI tract's gap is much larger than all the fake gaps, that's our evidence that the effect is real."

The RMSPE ratio:
> "The formal test compares post-treatment error to pre-treatment error. If your treated unit has a much higher ratio than the placebo units, you have a statistically significant result. The p-value is simply the share of units with a ratio as large or larger than yours."

---

### [0:55–1:00] The Denver CNI Case
*[Slides 15–19]*

Keep this brief — you'll show the actual results in the coding section.

**Slide 16 — The wicked problem framing**
> "This isn't just a methods exercise. The underlying question has real stakes. Crime is a wicked problem — it resists simple solutions, it involves multiple agencies with conflicting incentives, and interventions are often evaluated on process rather than outcomes."

Point out the two-layered research question:
- RQ1: Did CNI change *policing behavior* — specifically, did it reduce police stops?
- RQ2: Did that change in behavior translate into actual *reductions in crime*?

> "These are different questions. An intervention could change policing without changing crime, or vice versa. We're testing both."

**Slides 18–19 — Raw trend plots**
> "Before we touch the model, look at these raw trends. You can already see something happening around 2016–2017. The SCM will let us formalize that intuition and separate signal from noise."

---

## ☕ BREAK (10 minutes)

*[Announce the break. While people stretch:]*
- Circulate and help anyone with R setup issues
- Confirm everyone has the `data_clean/` folder accessible
- Have `1000_synth.Rmd` open and ready to run live
- Open Section 1 of the Rmd so it's ready to go when you return

---

## BLOCK 3 — Live Coding: Data Walk in R (1:10–2:05)

---

> **Coding section philosophy:** You are not teaching R syntax. You are telling a story with data, and the code is how the story gets told. Keep the focus on *what* each step reveals, not *how* the code works. Participants have the Rmd — they can read the syntax later.

---

### [1:10–1:20] Setup & Data Overview
*[Slide 20 — section title slide | Rmd §1–2]*

*[Switch to RStudio live.]*

Run the libraries chunk and constants chunk. Nothing exciting happens — that's fine.

Say:
> "Two things to notice about how we set up the data. First, we define our treated unit and treatment timing as named constants at the top — `FIPS_CNI` and `I_TIME`. This is good practice: if anything changes, you update it in one place."

Run the data loading chunk. Point to the output:
> "Our panel is balanced — every census tract has exactly 44 quarters, from 2011 Q1 to 2021 Q4. SCM requires this. If a neighborhood has missing quarters, it gets dropped from the donor pool entirely."

Pause to explain the `group` variable:
- Group 1 = treated (Sun Valley, one tract)
- Group 2 = adjacent tracts — *excluded* to avoid spillover contamination
- Group 3 = donor pool

> "The exclusion of adjacent tracts is a methodological choice worth discussing. If the CNI program had spillover effects into neighboring areas — which is plausible for something like policing — then including those neighbors as 'controls' would bias our estimate downward."

---

### [1:20–1:30] Descriptive Trends
*[Rmd §3]*

Run the crime trend plot. Let it render. Give the room a moment to look at it.

> "Before any modeling, we do this. Always. Look at the CNI tract — the red line — and the donor average — the grey line. What do you see before 2016?"

*[Pause for responses.]*

> "They move together. That's the parallel trends assumption holding visually. After 2016, they diverge. That's our effect — tentatively. The SCM will tell us whether that divergence is real or noise."

Run the stop trend plot. Compare:
> "Now look at police stops. The pattern is even more dramatic. Stops in the CNI tract drop sharply after the de facto launch. The donor average stays relatively flat. This is a striking descriptive finding even before we model anything."

---

### [1:30–1:45] Building the SCM Step by Step
*[Slides 24–26 | Rmd §4, Steps 1–5]*

*[Keep slides visible alongside RStudio, or switch back and forth.]*

Walk through the four functions conceptually as you run each step:

**Step 1 — `synthetic_control()`**
> "This is purely declarative. We're telling R: here's my panel, here's my treated unit, here's when treatment started. Nothing is estimated yet."

**Step 2 — `generate_predictor()`**
> "Now we tell the algorithm *what to match on*. Notice we call this function twice — once for demographics measured at 2015, and once for the outcome average over the full pre-treatment period. The pre-treatment outcome mean is almost always the most important predictor. We want the synthetic control to track where Sun Valley was historically, not just where it was at one point in time."

**Step 3 — `generate_weights()`**
> "This is where the optimization happens. The algorithm is solving for donor weights that minimize the difference between the CNI tract and the weighted donor average across all our predictors. Most donors will receive zero or near-zero weight — the synthetic is usually built from a small number of neighborhoods."

**Step 4 — `generate_control()`**
> "One line. This applies the weights to the full time series — pre and post — to produce the counterfactual trajectory."

**Step 5 — Inspect the fit**
Run `grab_balance_table()` and show the output:
> "This is your pre-flight check. The 'Synthetic' column should be close to the 'Treated' column for every predictor. If they're far apart, your synthetic control is not a good twin and your results aren't trustworthy."

Run `grab_unit_weights()` filtered to weights over 1%:
> "Usually only 3–5 neighborhoods contribute meaningfully to the synthetic. This is normal and actually a feature — it means the algorithm found specific peers, not a diffuse average of everyone."

Run `plot_trends()`:
> "The pre-treatment fit should look tight. Post-2016, we let the lines go where they go — the gap is our estimated effect."

---

### [1:45–1:55] Results Across All Outcomes
*[Rmd §5–8]*

Run `run_synth()` for all 8 outcomes using the helper function. You don't need to walk through each one in detail — pick 2 or 3 that tell the most interesting story.

Suggested narrative:
> "Let's look at all stops first — the broadest measure. Then drug crimes — which tends to be the most sensitive to policing changes. Then violent crime — which is what residents actually care about most."

Show the treatment effect summary table (§8):
> "The gap column is the key number. Negative means the CNI tract fell *below* its synthetic counterfactual — a reduction. Positive means it exceeded it. And the percent change column gives you the practical magnitude."

Pause for a genuine interpretive moment:
> "What do you see? Does the direction of effects make theoretical sense? Are the magnitudes plausible? This is where your substantive knowledge as a researcher matters — the model gives you a number, but you have to decide if it's credible."

---

### [1:55–2:05] Placebo Tests
*[Rmd §9]*

> "Now we stress-test the results. If these effects are real, the CNI tract's gap should look unusual compared to all the donor tracts that did *not* receive the intervention."

Run `plot_placebos()`:
> "Each grey line is a donor neighborhood running its own fake SCM. The colored line is Sun Valley. If the colored line is clearly outside the grey cloud in the post-treatment period, that's our visual evidence of significance."

Run `grab_significance()`:
> "The MSPE ratio ranks all units — treated and placebos — by how much their post-treatment error exceeds their pre-treatment error. If the CNI tract is ranked first or second, the Fisher p-value will be small. If it's buried in the middle of the distribution, the result is not statistically distinguishable from noise."

Close with the interpretive point:
> "Notice what we *don't* have — we don't have standard errors, confidence intervals, or t-statistics in the traditional sense. SCM inference is fundamentally about uniqueness: is the treated unit's gap unusual relative to what we'd expect by chance? That's a different kind of evidence than what you're used to, but it's honest about what the data can tell us."

---

## BLOCK 4 — Wrap-Up & Discussion (2:05–2:15)

---

### [2:05–2:10] Limitations & Critical Reflection
*[No specific slide — your own words]*

Don't skip this section. ICQCM scholars are specifically oriented toward critical quantitative work and will appreciate intellectual honesty about the method's limits.

Key limitations to name:

> "**Data demands are high.** You need a long pre-treatment period, a balanced panel, and a meaningful donor pool. If you have 3 years of pre-treatment data or 5 donors, SCM will overfit and the results won't be credible."

> "**Donor pool selection is consequential.** The results can change depending on who you include as potential donors. This is both a limitation and a responsibility — you should be transparent about that choice."

> "**SCM cannot identify mechanisms.** We can see *that* the gap exists, but we can't decompose *why* from the model alone. The mechanism story has to come from theory and qualitative evidence."

> "**The Frankenstein can be wrong.** Pre-treatment fit is necessary but not sufficient. There may be unobserved shocks post-treatment that have nothing to do with the intervention."

---

### [2:10–2:15] Open Discussion & Q&A

Open with a genuine question to the room rather than just "any questions?":

> "Here's what I'd actually like to hear from you: where does this method fit — or not fit — in your own research? What would you need to believe about your data before you'd trust a synthetic control estimate?"

If time allows, a useful prompt:
> "Think about the critical quantitative lens ICQCM emphasizes. SCM is powerful, but it's still operating within a positivist causal framework. What does that framework assume? What does it render invisible?"

*[Close warmly, point to the GitHub repo one more time, offer to be available for questions.]*

---

## APPENDIX — Common Participant Questions

**"What if my pre-treatment fit is poor?"**
It means the donor pool doesn't contain units similar enough to your treated unit. Options: expand the donor pool, reconsider predictors, or acknowledge the limitation. Do not proceed with a poor-fitting synthetic as if it's credible.

**"How many donor units do I need?"**
More is generally better — at least 10–15, ideally 20+. With very few donors, the optimization overfits and the placebo distribution is too thin for meaningful inference.

**"Can I use SCM with multiple treated units?"**
Not directly with `tidysynth` — it's designed for one treated unit. For multiple units, look into the `augsynth` package or generalized synthetic control methods (Xu 2017).

**"How do I choose which predictors to include?"**
Theory first, then pre-treatment fit. Include variables that are theoretically related to the outcome trajectory, not every covariate available. More predictors are not always better.

**"What's the difference between tidysynth and the original Synth package?"**
`tidysynth` is a tidy reimplementation of `Synth` — same underlying method, but designed for pipe-friendly workflows and much easier to use. The output functions (`grab_*`, `plot_*`) make diagnostics far more accessible.
