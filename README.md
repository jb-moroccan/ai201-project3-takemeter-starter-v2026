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
| Local Recommendation | 34 | 22.1% |
| Blunt Directive | 26 | 16.9% |
| **Total** | 154 | 100% |

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
