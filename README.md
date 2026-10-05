# The Unofficial Guide

<!-- Replace this line with your name and which corpus you picked. -->

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

<!-- This project is a retrieval-based unofficial guide using the `city_guides` corpus. It answers questions about fictional towns in the region, including transportation, food, accessibility, parking, walking routes, and seasonal information. The system splits the guides into sections, retrieves relevant information for a user's question, and uses that information to generate an answer with sources. A relevance gate prevents the system from answering questions that are not supported by the city guides.

     Milestone 5. -->

## Chunking Strategy

**Chunk size:** Variable — each chunk is based on a section of the guide rather than a fixed character size.

**Overlap:** 0 characters

I chose to split the city guides using their section headings instead of a fixed character size. The guides are long documents organized into sections such as "Getting there," "Eat and drink," and "When to go." When I tested the original fixed-size chunker, some chunks ended in the middle of words or sentences, which made them harder to understand on their own.

The section-based approach keeps related information together and creates chunks that contain more complete thoughts. After changing the chunker, the 14 documents produced 98 chunks averaging 291 characters, with the shortest at 23 characters and the longest at 708 characters. I did not use overlap because I am using the existing section boundaries instead of cutting the documents at arbitrary character positions.

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `guide_accessibility.md#0` — produced by: `chunker.py::split_documents`

```text
# Getting around the region with limited mobility

An honest assessment rather than a promotional one. Some of these places are
difficult and it is better to know in advance.
```

**Chunk 2** — source: `guide_corry_vale.md#6` — produced by: `chunker.py::split_documents`

```text
When to go

May to September. Outside those months the pub in the third village closes, the farm shop reduces its hours, and several footpaths become genuinely boggy rather than merely wet. The road is not gritted above the second village and is impassable in snow.
```

**Chunk 3** — source: `guide_givens_mill.md#3` — produced by: `chunker.py::split_documents`

```text
Eat and drink

A tearoom attached to the mill, open 10 to 4 daily except Tuesdays, which sells bread made from the flour ground twenty metres away and is the reason most people come. One pub, food served lunchtimes and Thursday to Saturday evenings.
```

**Chunk 4** — source: `guide_kestrelford.md#6` — produced by: `chunker.py::split_documents`

```text
When to go

Late spring and early autumn. The Saturday market runs year-round but is much reduced from November to February. August is busy with walkers. The single-track approach road is genuinely difficult in snow and the town can be cut off for a day or two most winters.
```

**Chunk 5** — source: `guide_regional_transport.md#1` — produced by: `chunker.py::split_documents`

```text
The railway

The line runs along the river valley, connecting Brightwater to the regional
hub in 50 minutes. Eleven services a day on weekdays, six on Sundays. The line
north of Brightwater closed in 1963 and everything beyond it is bus or car.

Tickets are cheaper booked the day before than on the day, and considerably
cheaper than that booked a week ahead. There is no ticket office at
Brightwater station outside weekday mornings; the machine on the platform takes
cards only.
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

## Sample Answer

**Question:** How do food prices on the Halden Bay harbour front compare with Fell Street?

**Answer:**

```
Prices on the Halden Bay harbour front are roughly double those on Fell Street for comparable food.

Sources: `guide_eating.md` and `guide_halden_bay.md`

Sources retrieved: guide_eating.md, guide_halden_bay.md, guide_pellew_sands.md, guide_regional_transport.md
```


**My relevance cutoff:** `0.72`

I tested five questions that should be answered by the city guides and five questions that were outside the corpus. The in-corpus distances ranged from 0.266 to 0.689, while the out-of-scope distances ranged from 0.754 to 0.899. The original cutoff of 0.6 incorrectly blocked an in-corpus question with a distance of 0.689. I chose 0.72 because it falls in the gap between the highest in-corpus distance (0.689) and the lowest out-of-scope distance (0.754).

| Question | In corpus? | Best distance |
|---|---|---:|
| Can I take a bus from Brightwater to Kestrelford on a Sunday? | Yes | 0.266 |
| How do food prices on the Halden Bay harbour front compare with Fell Street? | Yes | 0.301 |
| Can I use the same bus ticket with different bus operators in the region? | Yes | 0.689 |
| Where can I park for free in Pellew Sands? | Yes | 0.445 |
| When is the Halden Bay coastal path closed? | Yes | 0.311 |
| What is the capital of Mongolia? | No | 0.754 |
| How do I change the oil in a diesel engine? | No | 0.889 |
| Who won the 1994 World Cup? | No | 0.899 |
| What is the recommended dosage of ibuprofen? | No | 0.823 |
| How do I write a for loop in Rust? | No | 0.838 |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.** I used AI to help me think through a better chunking strategy after I noticed that the original fixed-size chunks were cutting off words and sentences. AI suggested using the existing section headings in the city guides as chunk boundaries. I used that approach in `split_documents()` and then inspected five new sample chunks to make sure they contained complete thoughts before keeping the change.

**2.** I used AI to help me interpret the retrieval distances when choosing a relevance cutoff. After testing five in-corpus and five out-of-scope questions, I shared the results with AI and compared the two groups. The highest in-corpus distance was 0.689 and the lowest out-of-scope distance was 0.754, so I changed the cutoff from 0.6 to 0.72. I then reran the previously blocked bus-ticket question to verify that it could now be answered correctly.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunks contain the answer | 4 of 5 | 4/5 | 4/5 | 4/5 | MET |
| 2. Every answer names a source | 5 of 5 | 4/5 | 5/5 | 5/5 | MISSED |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Sample chunks contain complete thoughts | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 5. Cited source is relevant to the answer | 4 of 5 | 4/5 | 4/5 | 4/5 | MET |

<!--### Baseline Evidence

Produced by `run_eval.py::main`, with retrieval from `store.py::search` and chunks from `chunker.py::split_documents`.

**Criterion 1 — Retrieved chunks contain the answer**

For 4 of the 5 test questions, the retrieved chunks contained enough information to answer the question. The Pellew Sands parking question was the exception. Although `guide_pellew_sands.md` was retrieved, the specific chunk containing the free parking information was not returned.

**Example successful output:**

```text
Question: Can I take a bus from Brightwater to Kestrelford on a Sunday?

No, you cannot take a bus from Brightwater to Kestrelford on a Sunday,
as the service does not run on Sundays.

Source: guide_regional_transport.md and guide_kestrelford.md
```

**Criterion 2 — Every answer names a source**

One answer in Run 1 did not name a source:

```text
Question: Where can I park for free in Pellew Sands?

I do not have enough information to answer where you can park for free
in Pellew Sands.
```

The other answers named at least one source document.

**Criterion 3 — Relevance gate**

```text
Gate refused 5 of 5 out-of-scope questions.

What is the capital of Mongolia? — refused
How do I change the oil in a diesel engine? — refused
Who won the 1994 World Cup? — refused
What is the recommended dosage of ibuprofen for a headache? — refused
How do I write a for loop in Rust? — refused
```

**Criterion 4 — Complete chunks**

The five sample chunks produced by `chunker.py::split_documents` remained complete section-based thoughts without sentences being cut off at the beginning or end.

**Criterion 5 — Relevant cited sources**

Four of the five test questions produced answers supported by the cited source documents. The Pellew Sands parking question did not produce the expected parking answer because the relevant parking chunk was not retrieved. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunks contain the answer | MET | All three runs scored 4/5. The Pellew Sands parking question was the only failure because the retrieved chunks did not contain the parking information. The 4/5 target was met in every run. |
| 2 | Every answer names a source | MISSED | Run 1 scored 4/5 because the Pellew Sands parking answer did not name a source. Runs 2 and 3 scored 5/5, but the target required every answer to name a source, so the target did not hold across all three runs. |
| 3 | Gate stops out-of-corpus questions | MET | The relevance gate refused all 5 out-of-scope questions, exceeding the target of 4/5. |
| 4 | Sample chunks contain complete thoughts | MET | All 5 inspected chunks contained complete thoughts without sentences being cut off at the beginning or end, exceeding the target of 4/5. |
| 5 | Cited source is relevant to the answer | MET | Four of the five questions produced answers supported by relevant cited source documents. The Pellew Sands parking question was the exception, giving a result of 4/5, which met the target. |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
