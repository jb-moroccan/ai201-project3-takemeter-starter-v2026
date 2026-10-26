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

This is a classifier that looks at posts from the New Jersey subreddit and categorizes posts into 4 different labels. These 4 labels are Civic Gripe, State Pride, Blunt Directive, and Local Recommendation. I went through a round of cold and AI pre-labeling that I manually reviewed. The device I used to train on is a CPU. 

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `Civic Gripe`

**Definition:** Targets a shared NJ public pain point, like transit, taxes, roads, MVC, etc.

**Example 1:**
>Clearly needs that hat and sunglasses to hide his identity cause he’s doing such honest work that he’s proud of /s…. Also the Jc cop who let him was named eduardo you’d think he’d stand with Spanish people with a very clear Spanish name

**Example 2:**
>That argument about the small number of people abusing it is intentional though. It prevents programs to help people from being enacted so we can give tax breaks to wealthy people

### `State Pride`

**Definition:** Debates or commentary on NJ culture, state reputation, regional splits (North/South/Central)

**Example 1:**
>There’s a Wendy’s in fort Lee that has a poster with a picture of some black and white movie and it says “the birthplace of cinema” or something.. thought that was weird until I googled it. 

**Example 2:**
>I never do salt and pepper…the bacon or the TAYLOR HAM is salty enough. Minimal ketchup for me, thanks

### `Local Recommendation`

**Definition:** Asks about or provide specific geographic spots, business, or hidden gems in the state of NJ.

**Example 1:**
>I am 25f with a medical condition that makes me pass out if I stand too long. Looking for bars in the central Jersey area for younger people with lots of seating. Not just standing room.

Thanks

**Example 2:**
>Derpycon's a great time! I've gone the past 2 years and am going again this year. As others have said, it's a pretty small convention, but there's a lot to like about the smaller cons.

They've got a few small panel rooms in addition to the main events room. The con runs pretty late with some panels going til a little after midnight if I'm remembering correctly. The arcade is great and has some really fun rhythm and racing games. Dealer's room and artist alley are both pretty small. They've also got stuff like karaoke, AMVs, board games, basically anything you'd find at any nerdy cons.

The location is also great. The setup of the hotel is great for hanging around and the fact that it's right in New Brunswick means you've got plenty of options for bars and food.

Since it's pretty small, if you're not sure about spending the whole weekend down there, you can just get a single day badge for whichever day ends up having the most stuff on the schedule you'd be interested in.

### `Blunt Directive`

**Definition:** A direct, no nonsense factual correction or practical solution.

**Example 1:**
>I block the texts. When they call I pick up and stay on the line enough to get a real person. Then I play the loudest part of Raining Blood by Slayer into my phone from my computer speakers at max volume.

**Example 2:**
>The people behind you that are honking will not pay your bills when you rush and cause an accident.

### The hardest boundary

**Which two labels:** State Pride and Local Recommendation

**The decision rule I used every time:**
**Test: is the post pointing the reader at a place or business they could go to?** If yes, it's `Local Recommendation`. If the post is about NJ as a whole (its culture, food, reputation, or regional identity), it's `State Pride`, even when it names a real town or business along the way.

The tie-breaker: delete the named place from the post. If the point survives, it's `State Pride`. If the point disappears with it, it's `Local Recommendation`.

*Example:* "There's a Wendy's in Fort Lee with a poster calling the area the 'birthplace of cinema.'" Remove Fort Lee and the Wendy's, and the post still says NJ has a claim to film history. That's `State Pride`. Compare "Looking for bars in central Jersey with lots of seating." Remove "central Jersey" and the request is meaningless. That's `Local Recommendation`.

A post that names a place *and* asks for or gives a recommendation is always `Local Recommendation`, even if it sounds like culture talk.

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**

New Jersey Subreddit: https://www.reddit.com/r/newjersey/

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->

I labeled the first 20 cold without any help. Then, I used AI to help pre-label the remaining rows. I then went through to double check the labels to see if they were accurate and updated any that didn't make sense.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| Civic Gripe | 55 | 35.7% |
| State Pride | 39 | 25.3% |
| Blunt Directive | 34 | 22.1% |
| Local Recommendation | 26 | 16.9% |
| **Total** | 154 | 100% |

I only used 154 posts total because I ran out of time collecting posts on reddit, so I decided to stick with a lower amount.

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "Car insurance is dogshit here. Also, maybe the person is disabled and can't drive so they can't buy a car? You make it sound so easy to just go out and get a car 😂"
>
> *Could have been:* Civic Gripe or Blunt Directive
>
> *I chose, because:* I chose Blunt Directive because although the post mentions insurance, the subject was discussing about the person not having a car and not being able to drive. It's a direct response to the commenter proposing another idea of why the person may not drive or have a car, rather than talking about insurance issues in NJ.

**2.**
> *The post:* There’s a Wendy’s in fort Lee that has a poster with a picture of some black and white movie and it says “the birthplace of cinema” or something.. thought that was weird until I googled it.,State Pride,cold
>
> *Could have been:* State Pride or Local Recommendation
>
> *I chose, because:* I chose State Pride because if you remove the Fort Lee and Wendy's, the post still talks about NJ claim to film history vs. suggesting to visit either the restaurant or city. This is about the history not recommending a place to visit.

**3.**
> *The post:* Isn’t or wasn’t there also an office or something Nintendo related in Glen Rock as well? Feel like I remember that as a kid. Nothing to actually go in and see but just a random office or something.
>
> *Could have been:* State Pride or Local Recommendation
>
> *I chose, because:* I chose State Pride because even though both a city (Glen Rock) and office (Nintendo) are places that are mentioned in the post, this is more of a question around NJ history than asking about a place to visit. State Pride captures the commentary of this post on NJ's gaming history and culture.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** "distilbert-base-uncased"

**Settings:** <!-- epochs, learning rate, batch size, seed -->
EPOCHS = 3
LEARNING_RATE = 2e-5
BATCH_SIZE = 16
SEED = 42

**Anything I changed from the defaults, and why:**
Nothing

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

label
Civic Gripe             55
State Pride             39
Blunt Directive         34
Local Recommendation    26

train 107  ·  val 23  ·  test 24

                      train  val  test
label                                 
Civic Gripe              38    8     9
State Pride              27    6     6
Local Recommendation     18    4     4
Blunt Directive          24    5     5

[note] 'State Pride' has only 6 examples in the test split.
[note] 'Local Recommendation' has only 4 examples in the test split.
[note] 'Blunt Directive' has only 5 examples in the test split.


---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I asked Claude to help pre-label the remaining rows of reddit posts after the 20 I first labeled cold.
- *What came back:* A list of rows of reddit posts pre-labeled with one of my 4 labels and a note that it was pre-labeled.
- *What I changed:* I modified a couple rows, notated with the text modified, for labels that I believe were labeled incorrectly.

**Moment 2**

- *What I asked for:* I asked Claude if any 1 label in my group of reddit posts was above 70% since that was a limit we needed to adhere to.
- *What came back:* It came back with a breakdown of each label and how many posts had that label along with their percentages. None were over 70%.
- *What I changed:* Nothing was modified in code as a result since all the labels were under the 70% limit.

**Pre-labelling disclosure:**
Besides the 20 posts that were labeled cold, the remaining 134 posts in the list were labeled my Claude providing it with my descriptions for what each label meant. Anything that didn't get labeled properly will have 'modified' indicated after the pre label text.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.417 | 0.375 | -0.042 |
| Macro F1 | 0.329 | 0.136 | -0.192 |
| F1 — `Blunt Directive` | 0.000 | 0.000 | +0.000 |
| F1 — `Civic Gripe` | 0.200 | 0.545 | +0.345 |
| F1 — `Local Recommendation` | 0.615 | 0.000 | -0.615 |
| F1 — `State Pride` | 0.500 | 0.000 | -0.500 |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

I expect the trained model to beat the baseline most on Civic Gripe's F1, because it has the most labeled posts, so training has the most examples to learn that label from.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->

Civic Gripe did go up for its F1 from 0.200 to 0.545 so my prediction was accurate even though overall, the model performed worse than the baseline. Accuracy dropped from 0.417 to 0.375 and macro F1 dropped from 0.329 to 0.136. All labels other than Civic Gripe for their F1 were 0.000, so it's likely the model is not predicting any label other than Civic Gripe or is getting them wrong.

---

## Run Log — Before

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---|---|---|---|
| Overall accuracy | 0.375 | 0.375 | 0.375 | 0.000 |
| Macro F1 | 0.136 | 0.136 | 0.136 | 0.000 |
| F1 — `Civic Gripe` | 0.545 | 0.545 | 0.545 | 0.000 |
| F1 — `State Pride` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `Local Recommendation` | 0.000 | 0.000 | 0.000 | 0.000 |
| F1 — `Blunt Directive` | 0.000 | 0.000 | 0.000 | 0.000 |

Accuracy moved 0.000 across seeds. Stable enough to trust.

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. 0.60 accuracy for each seed | ≥ 0.60 | 0.375 | 0.375 | 0.375 | MISSED |
| 2. 0.50 F1 score for each seed | ≥ 0.50 | 0.136 | 0.136 | 0.136 | MISSED |
| 3. Most common label has no more than twice least common | yes | no | no | no | MISSED |
| 4. Labeling 30 posts a second time, 70% match first pass | ≥ 70% | yes | yes | yes | MET |
| 5. Accuracy on most confident third must be 0.15 higher than least confident | yes | no | no | no | MISSED |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | Civic Gripe | State Pride | Local Recommendation | Blunt Directive |
|---|---|---|---|---|
| **Civic Gripe** | 9 | 0 | 0 | 0 |
| **State Pride** | 6 | 0 | 0 | 0 |
| **Local Recommendation** | 4 | 0 | 0 | 0 |
| **Blunt Directive** | 5 | 0 | 0 | 0 |

The device I used to train on is a CPU. 

Biggest confusion: 6 real `State Pride` posts called `Civic Gripe`.
The other direction: 0 real `Civic Gripe` posts called `State Pride`.

That's lopsided — the model leans toward `Civic Gripe` on this pair.
A direction like that usually points at your labelling rather than the
model. Your agreement report is the thing that can tell you which.

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

My biggest off-diagonal number is 6 real State Pride posts were labeled as Civic Gripe, so the model hasn't learned the boundary between these two labels."

---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | 0.60 accuracy for each seed | ≥ 0.60 | MISSED | The Run Log — Before shows 0.375 on seeds 42, 7 and 2024, a spread of 0.000. The confusion matrix explains it: all 24 test posts were predicted Civic Gripe, so 0.375 is just Civic Gripe's share of the test split (9 of 24). That is 0.225 short on every seed. |
| 2 | 0.50 F1 score for each seed | ≥ 0.50 | MISSED | Macro F1 is 0.136 on every seed. Only Civic Gripe has a non-zero F1 (0.545). The other three labels are 0.000, so the macro average is 0.545 divided by 4. That is 0.364 short on every seed. |
| 3 | Most common label has no more than twice least common | yes | MISSED | Test split support is Civic Gripe 9 and Local Recommendation 4, a ratio of 2.25, which is over the limit of 2. The split is the same for every seed, so the result is "no" on all three. |
| 4 | Labeling 30 posts a second time, 70% match first pass | ≥ 70% | MET | The agreement report has 24 of 30 posts matching the staff labels, which is 80% and above the 70% line. Four of the six disagreements were rules I applied loosely (pile a), so they are consistency problems in my labelling, not a reason to lower the rate. |
| 5 | Accuracy on most confident third must be 0.15 higher than least confident | yes | MISSED | Most-confident third minus least-confident third, from the confidence results: seed 42 is 0.375 − 0.375 = 0.000, seed 7 is 0.375 − 0.250 = +0.125, and seed 2024 is 0.375 − 0.500 = −0.125. The target needs at least +0.15 on every seed, so on these numbers this is MISSED. |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->

Criteria 1:

Cause: too few examples for a label, which is why the model collapses to one label. Every one of the 24 test posts was predicted Civic Gripe, so the 0.375 accuracy is only the share of Civic Gripe in the test split (9 of 24). Civic Gripe is also the label with the most labeled posts, so the model has learned the majority label and nothing else. The agreement report can't test this: it compares my analysis, hot_take and reaction calls to the staff set on 30 posts, not the four labels in labels.csv, so it can't rule a labelling cause in or out.

Criteria 2:

Cause: too few examples for a label, plus a genuinely hard label pair. Macro F1 is 0.136 because only Civic Gripe has a non-zero F1; the other three labels are 0.000 on every seed. The one pair the model did see is State Pride and Civic Gripe, where 6 real State Pride posts were called Civic Gripe and none went the other way. The agreement report can't check this pair, since it uses the staff taxonomy. It does show that I applied written rules loosely on 4 of its 6 disagreements (pile a). That means the 6-to-0 split could be partly my own inconsistent State Pride and Civic Gripe calls, and I haven't checked that yet. The pair may also just be hard to separate from this data, and the other labels never get predicted at all.

Criteria 3:

Cause: too few examples for a label. The test split has 9 Civic Gripe posts and 4 Local Recommendation posts, a ratio of 2.25 against a limit of 2. This is a property of how many posts I labelled per label, so the fix is more Local Recommendation posts rather than changing the model. This count is also partly a labelling result. The agreement report shows I applied written rules loosely on 4 of its 6 disagreements, and drift like that moves posts between labels, which changes how many land in each one. It only covers 30 posts, though, so it can't tell me how far that drift goes across all 154.

Criteria 5:

The model's confidence tells me nothing about whether it is right. On seed 42 the mean confidence is 0.315 when right and 0.316 when wrong, and the most-confident and least-confident thirds both score 0.375. Seed 7 and seed 2024 show a gap of +0.125 and −0.125, so the sign flips with the seed and the target is never reached. The cause is a task the model can't reach from this much data: the predictions collapse to Civic Gripe for all 24 test posts, so every third has the same accuracy as the majority class. The too-few-examples cause for the three labels the model never predicts is the reason it can't reach the task. The agreement report can't rule label noise in or out for these four labels, since it covers only 30 posts under the staff taxonomy. Its 80% match with staff does not show that my labels are clean. I also applied written rules loosely on 4 of its 6 disagreements (pile a), which is the kind of inconsistency that would add noise. The likely fix is still more examples for the three labels the model never predicts, but I should check label consistency before I rely on that.

---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 24 / 30 = 80%

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

| Post | Staff said | I said | My call, and why | Which pile |
|---|---|---|---|
| Checked the entry lists. 41% of this year's field have never run the … | `analysis` | `hot_take` | I said hot take because although a fact wast listed, the conclusion reached about finish line photos and standards dropping didn't appear to be related. Since analysis requires a checkable fact to be load bearing and not a decoration, and one can argue here that the fact could be impacting photos, I was right and the rule was ambigious. | c |
| Went back through the club results going back to 2012. Every single y… | `analysis` | `reaction` | I said reaction because the end sentence appeared to be an immediate response to being a bad luck year. However, the stat about how many people enter the marathon is specific, checkable and carries the argument. Staff is right, rule covered this, and I applied it loosely. | a |
| Three of us ran the same session on the same day, different shoes, wi… | `analysis` | `reaction` | I said reaction because there were stats initially stated, but the response to those stats started off by saying I'm not saying shoes do nothing which was reactionary after the race results. Rule 3 states recency doesn't outrank evidence, and the fact of them being 10 seconds apart is checkable. Staff is right and the rule covered this. I applied it loosely | a |
| The club needs to stop pretending the Tuesday session is for everyone… | `hot_take` | `reaction` | I said reaction because the post felt like a response to how the club handles Tuesday sessions at a Tuesday session. Rule 2 states hot take is something general about the world, while reaction is what just happened. Staff is right since this is general, and I applied the rule loosely. | a |
| Road racing peaked in about 2015 and everything since has been market… | `hot_take` | `analysis` | I said analysis because the stat about road racing peaking is something that is checkable and is the argument for the change after. The fact isn't necessarily load bearing, so for Rule 1, it fails and should be a hot take. I applied it loosely and staff is right. | a |
| Every injury thread on this forum is the same injury and the same thr… | `hot_take` | `reaction` | I said reaction because the writer is responding immediately to their experience reading posts in the forum and how it feels for them. This isn't general about the world which rule 2 states is what makes something a hot take. I believe it's ambiguous whether this one unique forum is considered the world or not, so I am sticking with my answer. | c |

**What the pattern in my disagreements tells me:**

The pattern shows that I either apply rules loosely or believe that sometimes posts are ambiguous enough that can go either way depending on a rule. This tells me that I need to pay attention more to sticking close to the rules and being specific, along with working out the best choice in ambiguous instances.

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

       [X] criteria.md has five numbered criteria, each naming a NUMBER
       [X] Each has a reason underneath tied to your data or taxonomy
       [X] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [X] No label above 70%
       [X] All five unit 5 sections have real content
       [X] Label Taxonomy includes the decision rule for your hardest boundary
       [X] The Dataset includes three hard cases
       [X] results.json and test_split.csv committed (the notebook does this)
       [X] At least four new commits
       [X] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [X] Baseline vs. Trained table, with your prediction written beforehand
       [X] Run Log — Before, five criteria across three seeds
       [X] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [X] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [X] At least four new commits
       [X] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
