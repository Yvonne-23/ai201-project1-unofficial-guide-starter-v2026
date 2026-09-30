# Acceptance criteria — The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"Retrieval works"* is an opinion. *"For at
least 4 of my 5 test questions, the top results include a chunk containing the
answer"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter or looser one. A reason that says something about your corpus or your
pipeline earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions, the retrieved chunks include one that
contains the answer.

**Why this target:**
<!-- The city guides are longer documents where useful information can be spread across different sections and paragraphs. I chose 4 out of 5 because I expect the system to retrieve the right information most of the time, while allowing for one question where the answer may be harder to locate because of how the information is organized." -->

---

## 2. Every answer names a source

Every answer the system produces names at least one source document.

**Why this target:**
<!--Since the system answers questions using information from the city guides, users should be able to see where the information came from. I chose every answer because source attribution is already part of the pipeline, so an answer without a source would make it harder to verify that the response is actually based on the provided documents. -->

---

## 3. The relevance gate stops out-of-corpus questions

When I ask a question my documents clearly don't cover, the relevance gate
stops it and the system returns "I don't have enough information about that" —
in at least 4 of 5 tries.

<!-- The five questions are the ones in `OUT_OF_SCOPE` at the bottom of
     `questions.py`, and `run_eval.py` puts them through the gate and writes
     what happened into your run log. Swap them for your own if you'd rather —
     just keep five of them, or the "4 of 5" above has nothing to be 4 of. -->

**Why this target:**
<!-- The system should avoid giving an answer when the city guides do not contain enough relevant information. I chose 4 out of 5 because I want the relevance gate to reliably recognize questions outside the corpus, while allowing for one case where an unrelated question may still retrieve text that appears similar enough to pass the cutoff. -->

---

## 4. Something about your chunks

<!-- When I look at 5 sample chunks, at least 4 of them should contain complete thoughts without sentences being cut off at the beginning or end. -->



**Why this target:**
I chose this target because the original chunker cut some sentences and words in half, which made some chunks difficult to understand on their own. I chose 4 out of 5 because most sampled chunks should contain enough complete information to make sense without needing the text before or after them.


---

## 5. Your choice

<!-- For at least 4 of my 5 test questions, the answer should cite a source document that actually contains information relevant to the answer. -->



**Why this target:**
I chose this target because naming a source is only useful if that source actually supports the answer being given. I chose 4 out of 5 because I want the system to provide relevant sources consistently, while allowing for one question where retrieval may return a less relevant document.


---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 2 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 1. Retrieved chunks contain the answer

         For at least 4 of my 5 test questions, the retrieved chunks include
         one that contains the answer.

         **Why this target:** ...

         > **Revised in unit 2:** For at least 4 of 5 questions, the top three
         > results contain the answer.
         >
         > **Why revised:** I couldn't judge "the chunks include one that
         > contains the answer" the same way twice — I scored two questions
         > differently on Monday than on Wednesday. The new version is
         > something I can actually check.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said 4 of 5 but got 2 of 5, so 2 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.

     The whole reason the originals stay visible is so someone can see what you
     said before you knew the answer.
     ───────────────────────────────────────────────────────────────────────── -->
