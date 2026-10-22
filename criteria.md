# Acceptance criteria — TakeMeter

Five criteria that say what "working" means for this classifier, written in
unit 5 **before** anything was trained.

**All five are yours this time.** None are given. You've had two projects of
practice.

An acceptance criterion names a number. *"The model is accurate"* is an
opinion. *"Every label has an F1 of at least 0.60 on the held-out set"* is a
criterion.

Under each, write a sentence or two on **why that number**. A reason that says
something about your data or your taxonomy earns credit — *"I picked 0.60 F1
for `reaction` because it's my smallest label and I only have about 50
examples of it"*. A reason that could be attached to any project does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## Pick numbers you can defend

Not numbers that sound impressive. Four labels means a random guesser gets
about 25%, so a target of 0.40 is barely a target. Your number should sit
somewhere you'd honestly call useful.

**Cover at least three of these five areas.** They're here as prompts, not as a
form to fill in — a criterion that fits none of them is fine if it names a
number.

| Area | A question it could answer |
|---|---|
| Overall accuracy | How often does it need to be right to be worth using? |
| Per-label performance | Is one label allowed to be much worse than the others? |
| Balance | How lopsided can your label counts get before it's a problem? |
| Consistency | If someone else labelled the same posts, how often should you agree? |
| Confidence | Should a confident prediction be right more often than an unsure one? |

Two things worth knowing before you pick numbers, because both will affect
whether you hit them:

- **Your smallest label will have the jumpiest score.** If a label has 50
  examples, about 8 land in the test split. An F1 computed on 8 examples moves
  a lot between seeds. A target for that label should be looser than one for
  your biggest label, and saying so is a good reason.
- **Unit 6 tests across three seeds, and the target has to hold across all
  three.** A target of 0.65 against results of 0.71, 0.62, 0.68 is a **miss**.
  Pick with that in mind — it is stricter than it first sounds.

---

## 1.

<!-- Your criterion. It must name a number. -->

The classifier must achieve at least 0.60 accuracy on the test split for each of the three seeds.

**Why this target:**

There are four labels, so uniform random guessing gives 0.25 accuracy. My 0.60 target is 0.35 higher than that chance baseline. The test split is small, so results may vary across seeds.

---

## 2.

<!-- Your criterion. -->

On each of the three test splits, both `State Pride` and `Local Recommendation` must have an F1 score (a measure combining precision and recall) of at least 0.50.

**Why this target:**

These labels can both mention New Jersey places, but `State Pride` is about state identity and `Local Recommendation` points to a specific place or business. If the four labels are balanced, uniform random guessing gives an expected per-label F1 of about 0.25, so 0.50 is twice that rough chance baseline. I chose a lower target here because this label boundary is difficult.

---

## 3.

<!-- Your criterion. -->

In my labelled dataset, the most common label must have no more than twice as many examples as the least common label.

**Why this target:**

An imbalanced dataset could lead the classifier to favor the most common label, Civic Gripe. A 2-to-1 limit helps keep examples available for every label; the project notes that per-label scores can jump around when only about 8 examples land in the test split.

---

## 4.

<!-- Your criterion. -->

When labelling 30 of my posts a second time without looking at my original labels, at least 70% of the labels must match my first pass.

**Why this target:**

The `State Pride` and `Local Recommendation` boundary can be tricky when a post mentions a place. Matching at least 21 of 30 labels checks whether I apply my own rules consistently.

---

## 5.

<!-- Your criterion. -->

For each seed, accuracy on the most-confident third of test predictions must be at least 0.15 higher than accuracy on the least-confident third.

**Why this target:**

I chose a 0.15 gap as a practical sign that confidence separates more reliable predictions from less reliable ones. With about three predictions in each group, one extra correct prediction creates a 0.33 gap, which meets this target. The threshold is a judgment call.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 6 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath, like this:

         ## 2. Every label performs acceptably

         The model performs well on all labels.

         **Why this target:** ...

         > **Revised in unit 6:** Every label has an F1 of at least 0.60 on
         > the held-out set.
         >
         > **Why revised:** "performs well" gave me nothing to check. I
         > couldn't produce a verdict from it at all.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "Overall accuracy of at least 0.65" → "at least 0.55", because
            0.65 turned out to be optimistic for 200 examples.

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.
     ───────────────────────────────────────────────────────────────────────── -->
