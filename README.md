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

- *What I asked for:*
- *What came back:*
- *What I changed:*

**Moment 2**

- *What I asked for:*
- *What came back:*
- *What I changed:*

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
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
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

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



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
