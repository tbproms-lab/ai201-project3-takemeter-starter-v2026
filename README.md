# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

TakeMeter is built for r/soccer, specifically the fan comments on the 2025-26 Premier League final table thread. It sorts each comment into one of three labels: `analysis`, `banter` or `reaction`. The goal is to tell apart fans who back a point with a checkable fact, fans who are just needling a rival club, and fans who are venting how they feel about the result.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `analysis`

**Definition:**
Explains or argues something about the season using a specific, checkable fact (a number, a named player, a result)

**Example 1:**

Only 1/5 of Burnley's defence last season played consistently this season, it makes more sense once you realise it's a different set of players.

Trafford - Man City

Egan-Riley - Ligue 1

Roberts - injured for the entire season

Humphreys - injured most of the season and then had to cover across LB and CB

**Example 2:**

I don't know what to make of it... but I find it interesting.

The gap from 1st to 5th was 18 points.

The gap from 5th to 17th was 19 points.

Arsenal and Spurs were about the same distance from the Champions league bubble. Only 6-7 games

### `banter`

**Definition:**
A joke or dig aimed at a club or fanbase, meant to needle or get a laugh rather than to argue a point

**Example 1:**
Congratulations Spurs on back to back 17th place finishes. Consistency is key to building a good foundation.

**Example 2:**
Sunderland above Newcastle, natural order is restored.

### `reaction`

**Definition:**
An immediate expression of feeling about the result, with no claim for anyone to check or dispute

**Example 1:**
Best season of my life... I’m delirious right now.

**Example 2:**
Most nerve wracking second half ever.

### The hardest boundary

Love them or hate them, Arsenal finally got it done.

People laughed at anyone who even mentioned United in the Europa League places, Scamorim tax was so real lmao.

Top 4 and a European trophy for Villa... Emery is a top manager.

If Slot doesn't start next season on fire, he is so gone lol 💀

Iraola has a bright future, what he's done is insane! Buzzing for Bournemouth to qualify for Europe.

Sunderland might be the surprise package of season... 7th place & EL football next season with the cherry on top of Newcastle finishing in the bottom half lol.

Chelsea... LOL. LMAO even!

It's mad that 16th place would be success for Spurs next season 😭

Feel bad for West Ham, I hope they bounce back! 🙏

**Which two labels:**

`banter` vs `reaction`. Most lines are digs at clubs, but the comment is also a season-end vent, so my first instinct was `reaction`. A single `analysis` line (Villa/Emery) is also in there but is outnumbered.

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
1. A fact that only sets up a dig (sarcasm, a mocking emoji, a punchline)
   is `banter`, even though it's checkable. A fact that supports a claim
   about why or how something happened is `analysis`.
2. For a comment that mixes purposes, label it by what most of its lines
   are doing. A line is one paragraph (text separated by a blank line).
   Count only lines that fit a label. If lines split evenly, use the
   label of the first line.

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
The 200+ Reddit comments came from an r/soccer thread about the 2025-26 Premier League final table.

**How I labelled them:** 
I labelled the first 20 posts cold, by hand, with no AI help (marked `cold` in the `note` column of `labels.csv`). The other 180 were pre-labelled by an AI model, and I then read every one and corrected the labels I disagreed with (marked `checked`).

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `analysis` | 66 | 33% |
| `banter` | 71 | 35.5% |
| `reaction` | 63 | 31.5% |
| **Total** | 200 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "My rant has been slightly nullified now that we didn't finish last for the first time ever in the Prem and the first time in over 100 years. But FUCK. ME. we were the worst team in the league since October. If it wasn't for a historically bad AND unlucky start from Wolves they would have been clear of us by February. ..."
>
> *Could have been:* `reaction` or `analysis`
>
> *I chose `reaction`, because of rule 2.* The file stores the post as one paragraph, so it is one line and gets one label. That line is mostly venting, and the checkable facts (worst since October, Wolves' start) back up the rant instead of explaining how the season went.

**2.**
> *The post:* "tbf Brentford didn't win a game in March or April so they've started the bottling much earlier"
>
> *Could have been:* `analysis` or `banter`
>
> *I chose `analysis`, because of rule 1.* The fact (no win in March or April) supports a claim about how and when Brentford bottled. There is no sarcasm, mocking emoji or punchline, so the fact isn't just setting up a dig.

**3.**
> *The post:* "The Isak transfer saga ended up fucking both Newcastle and Pool which is genuinely impressive. "
>
> *Could have been:* `analysis` or `banter`
>
>\ *I chose `banter`, because of rule 1.* It names a cause (the Isak saga), but "genuinely impressive" is sarcasm, and the cause is only there to set up a dig at both clubs. It gives no checkable detail about how the saga hurt them.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`
device: mps  (Apple Silicon GPU (MPS))
torch:  2.14.1

**Settings:** 
3 epochs
learning rate 2e-5
batch size 16
seed 42 (max length 128)

**Anything I changed from the defaults, and why:**
I didn't change anything from the defaults.

**Split sizes:** 
139 train / 31 val / 30 test 
(200 posts, 70/15/15, stratified, seed 42). 
Test split per label: analysis 10, banter 11, reaction 9.
 No label had fewer than about 8 in test

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I asked it to help me label 180 out of the 200 posts based on the rules and prior decisions
- *What came back:* It labelled the 180 rows using my guidelines
- *What I changed:* I changed a couple of the labels to fit my labels better.

**Moment 2**

- *What I asked for:* I asked AI to help me count how many posts were under each label
- *What came back:* It brought back the counts.
- *What I changed:* I didn't change anything on this side since I didn't do the counts personally.

**Pre-labelling disclosure:** 180 of the 200 posts were pre-labelled by an AI model. I read and checked every one of them against my taxonomy and decision rules, and corrected the ones I disagreed with. The other 20 were labelled cold, by me alone.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.467 | 0.500 | +0.033 |
| Macro F1 | 0.363 | 0.389 | +0.026 |
| F1 — `analysis` | 0.556 | 0.667 | +0.111 |
| F1 — `banter` | 0.533 | 0.500 | -0.033 |
| F1 — `reaction` | 0.000 | 0.000 | +0.000 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

I think my trained model will beat the baseline model on all three labels because the trained model has all my data and expectations

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.500 | 0.500 | 0.367 | MISSED |
| 2. Every label F1 | ≥ 0.50 each | min 0.000 (`reaction`) | min 0.000 (`reaction`) | min 0.200 (`banter`) | MISSED |
| 3. Examples per label | ≥ 30 | 63 (smallest, `reaction`) | 63 | 63 | MET |
| 4. Agreement with second labeller | ≥ 80% of 30 | 30/30 = 100% | 100% | 100% | MET |
| 5. Balance (largest ÷ smallest) | ≤ 3x | 1.13x (71 / 63) | 1.13x | 1.13x | MET |

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.500 | 0.500 | 0.367 | 0.133 |
| Macro F1 | 0.389 | 0.393 | 0.367 | 0.026 |
| F1 — `analysis` | 0.667 | 0.640 | 0.500 | 0.167 |
| F1 — `banter` | 0.500 | 0.538 | 0.200 | 0.338 |
| F1 — `reaction` | 0.000 | 0.000 | 0.400 | 0.400 |

Accuracy moved 0.133 across seeds — more than 10 points. This means my dataset is too small or too lopsided for a stable measure. But, that is weird because my dataset is spread pretty evenly acorss three labels.

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | analysis | banter | reaction |
|---|---|---|---|
| **analysis** | 9 | 1 | 0 |
| **banter** | 5 | 6 | 0 |
| **reaction** | 3 | 6 | 0 |

Biggest confusion: 6 real `reaction` posts called `banter`.
The other direction: 0 real `banter` posts called `reaction`.

That's lopsided — the model leans toward `banter` on this pair.
A direction like that usually points at your labelling rather than the
model. Your agreement report is the thing that can tell you which.

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

My biggest off diagonal number is the reaction posts being counted as banter. In fact, the model doesn't classify any of the posts as reaction, which is definetly the thing I need to fix. It seems like boundary between reaction and banter is unclear as 6 reaction posts were called banter but not the other way round.

---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.65 | MISSED | 0.500, 0.500, 0.367. Not one seed reached 0.65, and the best is 0.15 short. |
| 2 | Every label F1 | ≥ 0.50 | MISSED | `reaction` F1 was 0.000 on seeds 42 and 7, and 0.400 on seed 2024. `banter` fell to 0.200 on seed 2024. Seed 42 and 7 also had `banter` at 0.500 and 0.538, so only `analysis` cleared 0.50 on every seed (0.667, 0.640, 0.500). |
| 3 | Examples per label | ≥ 30 | MET | 66 / 71 / 63. The smallest label (`reaction`) has twice the target. The data split is the same on every seed, so this holds on all three. |
| 4 | Agreement | ≥ 80% | MET | 30/30 on the staff set, with no disagreements (`agreement_results.json`). |
| 5 | Balance | ≤ 3x | MET | 71 / 63 = 1.13x. |

**Diagnoses**

**Criterion 1 (accuracy 0.50 / 0.50 / 0.37) and criterion 2 (`reaction` F1 0.00)** share one cause, so I diagnose them together.

- *It is not a count problem.* Criteria 3 and 5 are met. `reaction` has 63 examples, about 44 in train, which is nearly the same as the other labels. A label that has as much data as the others still scored 0.00, so thinness is not the explanation.
- *My agreement report doesn't show a labelling inconsistency.* I agreed with the staff on 30/30, so applying the written rules gives the same label as a second person. That argues against the rules themselves being applied two ways. Two caveats: the staff set tests whether I can follow the staff rules, not whether my own 200 are consistent, and 180 of my labels were AI pre-labelled and then checked by me. I can't fully rule out a `banter`/`reaction` inconsistency in my own file, but I have no evidence for one.
- *The best match is a genuinely hard label pair.* The confusion matrix has 6 of 9 real `reaction` posts called `banter` on seeds 42 and 7, and 0 `banter` posts called `reaction`. My own taxonomy notes name `banter` vs `reaction` as the hardest boundary: season-end digs at clubs are also venting, and my first instinct was `reaction`. With about 44 training examples on a boundary that blurry, DistilBERT defaults to the label it's surer of.
- *The model is barely above chance.* Mean confidence when right was 0.380 and when wrong was 0.373 on seed 42, and chance with three labels is 0.333. The model can't tell its good guesses from its bad ones, so it has learned little beyond `analysis`, which has the clearest surface cue (a checkable fact).
- *Seed 2024 shows the instability.* It flipped to calling 7 real `banter` posts `reaction`, so `reaction` went from 0.00 to 0.40 while `banter` fell from 0.50 to 0.20. The boundary is being drawn differently each run, which means training only weakly constrains it. The test split is also only 30 rows (9 `reaction`), so one or two posts move a label's F1 a lot.
- *Conclusion:* a hard `banter`/`reaction` pair with little data to separate it. It is not a lopsided dataset, which is why the spread surprised me.

**Criteria 3, 4 and 5 were met, so there is nothing to diagnose.**

---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 30 / 30 = 100%

I matched everything the staff rated the text 100% not because it was very easy. Geniunely, looking at the ones the staff said were borderline in staff_labels.csv, those ones gave me trouble as well, but I felt like the decisions and definitions the staff gave were very clear to help me correctly labelled. 

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->


---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
