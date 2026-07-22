# TRACK 7 — EVALUATION, SAFETY & RELIABILITY

*This is the track that separates someone who ships demos from someone who ships products — and almost nobody self-taught has it, which makes it a huge differentiator. How do you KNOW your AI system works? Not "it looked good when I tried it twice" — actually know, measured, across many cases, including adversarial ones. You'll build evaluation sets, use LLM-as-judge, test against attacks like prompt injection, and add guardrails. This is unglamorous, deeply professional, and exactly what an interviewer probes to find out if you've built real systems or just demos.*

*Prerequisites: Tracks 0–6 (you have systems worth evaluating now). Python + the anthropic SDK.*

*Spine courses (you own them): **AI Engineering Fundamentals** (evaluation and reliability) and **AI Agent: From Prototype to Production** (production safety/reliability). Anthropic's safety and evaluation docs are the primary text.*

*Gentle rhythm: STUDY → TINKER → REPORT → EXERCISE 1 → EXERCISE 2.*

*🧊 Cold opens + track-end cold rebuild. 📏 THIS TRACK FORMALIZES THE EVALS THREAD you've been living since 2.8 — you arrive already owning eval folders for prompting, retrieval, tools, and agents, so this track upgrades a habit rather than introducing a stranger. 🐛 Bug hunt in Module 7.5 (Level 3). 🔬 Autopsy in 7.5A: a real open-source eval harness — in TypeScript, your home turf, which is exactly why it was chosen.*

---

## Module 7.1 — Why Evaluation Is Everything (and Why "It Worked" Isn't Enough)

**Difficulty:** ●●●○○ · **Format:** STUDY → investigation · **Stack:** Python + anthropic SDK

### New words in this module

- **Evaluation (eval)**: systematically measuring whether your AI system produces correct/good outputs, across many test cases — not just eyeballing one run.
- **Eval set / test set**: a collection of inputs (and ideally expected outputs or quality criteria) you run your system against to measure quality.
- **Regression**: when a change makes the system worse on cases it used to handle. Evals catch these.

### Prep — Watch & Read

**Watch (Frontend Masters — you own these):**
- **AI Engineering Fundamentals** → the evaluation / testing sections. Main reference.
- **AI Agent: From Prototype to Production** → why eval matters for shipping.

**Read (gentle → deeper):**
- **"Why you need evals for LLM apps"** — search `"why evals matter llm application testing"` — *Focus on: you can't improve what you don't measure; demos lie.*
- **Anthropic — Evaluating prompts / building evals** — https://docs.claude.com/en/docs/test-and-evaluate/develop-tests — *Focus on: building an eval set and measuring.*
- **"Eval-driven development for AI"** — search `"eval driven development llm"` — *Focus on: evals as the dev loop for AI.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 7, Module 7.1 — why evaluation is everything. This is the
discipline most self-taught people skip; I want it cold. Gentle rhythm; decode eval, eval set, regression.

PHASE 1 STUDY: Take a system from an earlier track (e.g. my structured-extraction or RAG system) and show
me the difference between "it worked when I tried it" and a real EVAL: build a small eval set (many varied
test inputs with expected outputs or quality criteria), run the system against ALL of them, and measure a
success rate. Show how this reveals failures eyeballing missed. Then change a prompt and show the eval
catching a regression (it got worse on some cases).

PHASE 2 TINKER: Have me add tricky/edge cases to the eval set and watch the measured success rate drop to
something honest; make a "improvement" that actually regresses some cases and catch it with the eval.
Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: why evaluation is essential (you can't improve or trust what
you don't measure; demos mislead; non-determinism means one run proves nothing — Track 1.5), and what an
eval set is. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build an eval set and measure one of my earlier systems honestly — hints free.

PHASE 5 EXERCISE 2: Have me build an eval set for a DIFFERENT system solo, measure it, catch a regression,
then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Evaluation** is systematically measuring whether your AI system produces good outputs across many test cases — versus eyeballing one or two runs. An **eval set** is a collection of varied test inputs (ideally with expected outputs or quality criteria) you run the system against to get a real **success rate**. A **regression** is when a change makes the system worse on cases it used to handle — and evals are how you catch them.

**Why it's everything (and connects to Track 1.5):** the model is non-deterministic, so one successful run proves almost nothing — it might fail the next time, or on inputs you didn't try. Demos systematically mislead because you naturally test the happy path. Without evals, you're flying blind: you can't tell if a change helped or hurt, can't trust the system, and can't improve it methodically. *Eval-driven development* — building an eval set and using it to measure every change — is the professional dev loop for AI, the equivalent of tests in regular software.

**Why it's your differentiator:** most self-taught AI builders ship based on "it looked good." An engineer who says "I built an eval set, measured 87% success, found the failures cluster in X, and caught a regression when I changed the prompt" sounds dramatically more professional — and it's exactly what interviewers probe to separate demo-builders from product-builders.

The interview answer: *"How do you know your LLM app works?" → "Evaluation: a test set of many varied inputs with expected outputs or quality criteria, run systematically to measure a success rate — not eyeballing a run or two. Because the model is non-deterministic and demos test the happy path, one good run proves nothing. Evals catch regressions when I change prompts or models, and turn 'it seemed fine' into a measured number I can improve."*

The principle: **evaluation measures system quality across many varied cases to give an honest success rate and catch regressions — essential because non-determinism and happy-path demos make single runs meaningless, so eval-driven development is the professional dev loop for AI and a top differentiator.**
</details>

---

## Module 7.2 — Building Eval Sets: Test Cases for Non-Deterministic Systems

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Ground truth**: the known-correct answer for a test case, to compare the system's output against.
- **Exact-match vs fuzzy evaluation**: some outputs you can check exactly (a label, a number); others (a summary, an answer) need fuzzy/qualitative judgment because there are many valid phrasings.
- **Coverage**: making sure your eval set includes the variety of real inputs — easy, hard, edge, and adversarial cases.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → building test cases / eval design.

**Read:**
- **Anthropic — Create strong empirical evaluations** — https://docs.claude.com/en/docs/test-and-evaluate/develop-tests — *Focus on: designing test cases, coverage, grading.*
- **"How to build an eval set for LLMs"** — search `"build eval dataset llm test cases coverage"` — *Focus on: representative + edge cases, ground truth where possible.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 7, Module 7.2 — building eval sets. Gentle rhythm; decode ground
truth, exact vs fuzzy, coverage.

PHASE 1 STUDY: Show me how to build a GOOD eval set for one of my systems: choose representative inputs +
edge cases + adversarial cases (coverage), define ground truth or quality criteria for each, and decide
which outputs can be checked exactly (labels, numbers) vs need fuzzy judgment (summaries, answers). Build a
small but well-designed eval set and run it.

PHASE 2 TINKER: Have me deliberately make a thin eval set (only easy cases) and see it report a misleadingly
high score; then add edge/adversarial cases and watch the honest score appear; classify which test cases
need exact vs fuzzy grading. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what makes an eval set good (coverage, ground truth, right
grading method) and why a thin eval set lies. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a well-covered eval set (easy/hard/edge/adversarial) for one of my
systems with appropriate grading — hints free.

PHASE 5 EXERCISE 2: Have me build an eval set for a DIFFERENT system solo with good coverage, then the
verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

A good **eval set** needs **coverage** (representative inputs PLUS edge cases PLUS adversarial cases — not just easy happy-path examples), **ground truth** or clear quality criteria for each case (so you can grade), and the right grading method: **exact-match** for outputs you can check precisely (a classification label, an extracted number) and **fuzzy/qualitative** evaluation for outputs with many valid forms (a summary, an open answer — where you can't string-match because there are many correct phrasings).

**The key insight:** a thin eval set (only easy cases) reports a misleadingly high score and gives false confidence — the score is only as honest as the eval set is representative. Including edge and adversarial cases is what makes the measured success rate trustworthy. And recognizing that many AI outputs can't be exact-matched (which is why you need fuzzy grading, including LLM-as-judge in the next module) is central to evaluating non-deterministic systems.

The interview answer: *"How do you build an eval set for an LLM system?" → "Cover representative, edge, and adversarial cases — not just easy ones, or the score lies. Define ground truth or quality criteria per case. Use exact-match grading for checkable outputs like labels or numbers, and fuzzy/qualitative grading for open outputs like summaries where many phrasings are valid. The eval set's honesty depends on its coverage, so I deliberately include the hard and adversarial cases."*

The principle: **a good eval set has broad coverage (representative + edge + adversarial), ground truth or quality criteria, and the right grading method (exact-match for checkable outputs, fuzzy for open ones) — because a thin happy-path eval set reports a misleadingly high score, and honest evaluation requires the hard cases.**
</details>

---

## Module 7.3 — LLM-as-Judge: Using AI to Evaluate AI

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **LLM-as-judge**: using an LLM to grade another LLM's outputs against criteria — for the fuzzy/qualitative cases you can't exact-match.
- **Rubric**: the explicit criteria you give the judge (what makes an answer good: accurate? complete? grounded? well-formatted?).
- **Judge reliability**: the judge is itself an LLM, so it has biases and errors — you must validate that the judge agrees with human judgment.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → automated evaluation / LLM-as-judge sections.

**Read:**
- **Anthropic — Use LLM-based grading** — https://docs.claude.com/en/docs/test-and-evaluate/develop-tests — *Focus on: grading outputs with a model + rubric.*
- **"LLM as a judge explained"** — search `"llm as a judge evaluation rubric reliability"` — *Focus on: how, and the pitfalls (bias, needs validation).*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 7, Module 7.3 — LLM-as-judge. How to auto-grade fuzzy outputs.
Gentle rhythm; decode LLM-as-judge, rubric, judge reliability.

PHASE 1 STUDY: Show me LLM-as-judge: give a judge model a clear RUBRIC (criteria) and have it grade my
system's outputs on the fuzzy cases (summaries, answers) that can't be exact-matched. Build it for one of
my systems. THEN show the catch: the judge is itself an LLM with biases — show me how to validate the
judge against a few human judgments to trust it.

PHASE 2 TINKER: Have me write rubrics of varying clarity and see grading quality change; find a case where
the judge is wrong/biased; check judge-vs-my-own-judgment agreement on a sample. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what LLM-as-judge is, why it's needed (scales fuzzy
evaluation), and its key caveat (the judge is fallible — validate it against humans, write clear rubrics).
Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build LLM-as-judge with a good rubric for a system, and validate the judge —
hints free.

PHASE 5 EXERCISE 2: Have me build LLM-as-judge for a different system solo, including judge validation,
then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**LLM-as-judge** uses an LLM to grade another LLM's outputs against an explicit **rubric** (criteria: is the answer accurate? complete? grounded in sources? well-formatted?). It's how you scale evaluation of the **fuzzy** outputs (summaries, open answers) that you can't exact-match — a human can't grade thousands of outputs, but an LLM judge can.

**The crucial caveat (judge reliability):** the judge is *itself* an LLM, with its own biases and errors — it can be inconsistent, favor longer answers, or be fooled. So you don't blindly trust it: you **validate the judge** by checking its grades against human judgment on a sample, and you write **clear rubrics** (a vague rubric gives unreliable grades, just like a vague prompt gives bad outputs — Track 2). A validated judge with a clear rubric is a powerful, scalable evaluation tool; an unvalidated one gives false confidence.

The interview answer: *"What's LLM-as-judge and what are its limits?" → "Using an LLM with an explicit rubric to grade another model's outputs — essential for scaling evaluation of fuzzy outputs like summaries that can't be exact-matched. The limit: the judge is itself a fallible, biased LLM, so I write clear rubrics and validate the judge against human judgment on a sample before trusting it. A validated judge scales evaluation; an unvalidated one gives false confidence."*

The principle: **LLM-as-judge scales evaluation of fuzzy, non-exact-matchable outputs by grading them against a rubric — but the judge is a fallible LLM, so clear rubrics and validation against human judgment are required before trusting its grades.**
</details>

---

## Module 7.4 — Prompt Injection & Adversarial Inputs

**Difficulty:** ●●●●● · **Format:** STUDY → investigation · **Stack:** Python + anthropic SDK

### New words in this module

- **Prompt injection**: an attack where malicious text in the input (or in data the system processes) tricks the model into ignoring its instructions and doing something else.
- **Direct injection**: the user themselves types the malicious instruction. **Indirect injection**: malicious text hides in data the system pulls in (a web page, a document, a tool result).
- **Jailbreak**: getting the model to bypass its safety guidelines.
- **Untrusted input**: any input (user text, retrieved docs, tool results) that could be adversarial — which is all of it.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → security / safety sections.
- **AI Agent: From Prototype to Production** → safety against adversarial input.

**Read:**
- **"Prompt injection explained"** — search `"prompt injection direct indirect explained"` — *Focus on: the attack and why it's hard to fully prevent.*
- **OWASP — LLM security / prompt injection** — search `"owasp top 10 llm prompt injection"` — *Focus on: it's the #1 LLM security risk.*
- **Anthropic — Mitigating jailbreaks & prompt injection** — https://docs.claude.com/en/docs/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks — *Focus on: mitigations.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 7, Module 7.4 — prompt injection & adversarial inputs. Critical
security topic. Gentle rhythm; decode prompt injection (direct/indirect), jailbreak, untrusted input. (Keep
this educational/defensive — I want to DEFEND against these, not attack others.)

PHASE 1 STUDY: Explain prompt injection with safe, defensive examples: direct (user types "ignore your
instructions and...") and indirect (malicious text hidden in a document or tool result the system ingests —
connect to Track 5.7). Show why it's fundamentally hard to fully prevent (the model can't perfectly
distinguish instructions from data — connect to Track 1: it's all just tokens). Then show practical
MITIGATIONS: treating all input as untrusted, separating instructions from data, input/output filtering,
least privilege, and not giving the model unchecked power (Track 5.7, 6.5).

PHASE 2 TINKER: Have me try (defensively, on my own system) a simple injection and see it work, then apply
mitigations and see them help; test an indirect injection via a document. Make me think like a defender.
Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what prompt injection is, WHY it's hard to fully solve (the
model can't cleanly separate instructions from data — it's all tokens), and the layered mitigations. Name
the interview question and answer (this one strongly signals security awareness).

PHASE 4 EXERCISE 1: Have me harden one of my systems against injection with layered mitigations and test
it — hints free.

PHASE 5 EXERCISE 2: Have me harden a different system solo (especially one ingesting external data —
indirect injection risk), then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Prompt injection** is an attack where malicious text tricks the model into ignoring its instructions. **Direct injection**: the user types it ("ignore previous instructions and…"). **Indirect injection** (the scarier one): malicious text hides in *data the system ingests* — a web page, a document, a tool result (connect to 5.7) — so the attack comes through content you didn't write. **Jailbreaks** bypass safety guidelines. The mindset: treat *all* input — user text, retrieved documents, tool results — as **untrusted**.

**Why it's fundamentally hard (the deep point, connects to Track 1):** to the model, instructions and data are *all just tokens* in the context — there's no hard boundary between "my instructions" and "the data I'm processing." So a cleverly-worded piece of data can read like an instruction. This is why prompt injection can't be *perfectly* solved, only mitigated — it's the #1 LLM security risk (OWASP). **Mitigations** are layered: treat input as untrusted, separate instructions from data as much as possible, filter inputs/outputs, apply least privilege (5.7), and keep humans in the loop for consequential actions (6.5) so a successful injection has limited blast radius.

**Why it impresses:** security awareness is rare in self-taught AI builders. Explaining *why* injection is hard (instructions and data are both tokens) and naming layered mitigations signals genuine depth.

The interview answer: *"What is prompt injection and how do you defend against it?" → "An attack where malicious text — typed directly or hidden in ingested data (indirect injection) — tricks the model into ignoring its instructions. It's hard to fully prevent because to the model, instructions and data are all just tokens with no firm boundary. So I treat all input as untrusted, separate instructions from data, filter inputs and outputs, apply least privilege, and keep humans in the loop for consequential actions — layered mitigation that limits the blast radius rather than assuming perfect prevention."*

The principle: **prompt injection tricks the model via malicious text (direct or indirect via ingested data) and can't be perfectly prevented because instructions and data are all just tokens to the model — so you treat all input as untrusted and apply layered mitigations (separation, filtering, least privilege, human-in-the-loop) to limit the blast radius.**
</details>

---

## Module 7.5 — Guardrails: Catching Bad Output Before the User Sees It

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Guardrail**: a check (before or after the model) that catches unsafe, off-topic, or wrong output and blocks/fixes it before it reaches the user or triggers an action.
- **Input guardrail**: validates/filters what goes in (e.g. block obvious injection, off-topic, or disallowed requests). **Output guardrail**: validates what comes out (e.g. check for unsafe content, wrong format, hallucination).
- **Content moderation**: detecting harmful/disallowed content.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agent: From Prototype to Production** → guardrails / safety sections.
- **AI Engineering Fundamentals** → output validation/safety.

**Read:**
- **Anthropic — Strengthen guardrails** — https://docs.claude.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations — *Focus on: input/output checks, reducing harmful or wrong output.*
- **"LLM guardrails explained"** — search `"llm guardrails input output validation safety"` — *Focus on: layered checks around the model.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 7, Module 7.5 — guardrails. Layered checks around the model.
Gentle rhythm; decode guardrail, input/output guardrail, moderation.

PHASE 1 STUDY: Show me input guardrails (validate/filter what goes in — off-topic, disallowed, obvious
injection) and output guardrails (validate what comes out — unsafe content, wrong format, unsupported
claims), with worked examples wrapping one of my systems. Connect to validation (2.8) and injection
defense (7.4).

PHASE 2 TINKER: Have me add an input guardrail that blocks disallowed requests, and an output guardrail
that catches a bad/unsafe/malformed output; test them with cases that should pass and fail. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what guardrails are, input vs output, and why layered checks
around a fallible model are necessary. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me add appropriate input + output guardrails to one of my real systems — hints free.

PHASE 5 EXERCISE 2 — 🐛 BUG HUNT, LEVEL 3: Hand me a system whose safety layer LOOKS complete and say
nothing more. Plant realistic gaps (e.g. an output guardrail that validates format but not content, an
input filter bypassable by a rephrase, an injection path through retrieved/tool content that 7.4 taught me
to fear, a guardrail that silently fails open on error). I must ATTACK my way to the gaps — build
adversarial cases, probe, characterize what gets through — then fix, and prove the fixes with an
adversarial eval set (📏) that becomes a permanent regression suite. Full Prime Directive. Then the
verdict. Red-teaming my own system is the exact skill this track exists to build.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Guardrails** are checks around the model that catch bad output before it reaches the user or triggers an action. **Input guardrails** validate/filter what goes in (block off-topic, disallowed, or obviously malicious requests); **output guardrails** validate what comes out (catch unsafe content, wrong format, or unsupported claims/hallucination). **Content moderation** detects harmful content specifically. These layer with everything from earlier: output validation (2.8), injection defense (7.4), and tool/action safety (5.7, 6.5).

**The mental model:** the model is fallible and manipulable, so you wrap it in checks — like input validation and output sanitization around any untrusted component in regular software. Guardrails are how you make a system *safe to put in front of real users*, where someone will eventually send a malicious or weird input and the model will eventually produce something bad. They turn "usually fine" into "safe by design."

The interview answer: *"What are guardrails and why do you need them?" → "Checks around the model: input guardrails filter/validate what goes in (off-topic, disallowed, injection attempts), and output guardrails validate what comes out (unsafe content, wrong format, unsupported claims). Because the model is fallible and manipulable, you wrap it in layered checks — like validating untrusted input and sanitizing output in normal software — so bad inputs and bad outputs are caught before they reach users or trigger actions."*

The principle: **guardrails are layered input and output checks around the fallible model that catch unsafe, off-topic, or wrong content before it reaches users or triggers actions — turning a system that's 'usually fine' into one that's safe by design for real-world, sometimes-adversarial use.**
</details>

---

## Module 7.5A — 🔬 AUTOPSY: Read a Real Eval Harness (on Your Home Turf — TypeScript)

**Difficulty:** ●●●●○ · **Format:** 🔬 Autopsy (read, predict, compare) · **Stack:** reading real code (TypeScript!)

### New words in this module

- **Eval harness**: a tool that runs your test cases against your AI system, scores the outputs (assertions, model-graded judgments), and reports results — the industrial version of the `evals/run.py` you've been writing since 2.8.

### Prep — Watch & Read

**Read (live, during the autopsy):**
- **promptfoo** — https://github.com/promptfoo/promptfoo — *a widely used, real, open-source eval harness — written in TypeScript, which is exactly why it was chosen: for once, the LANGUAGE is your expert domain and only the DOMAIN is new. Target: the core evaluation runner and an assertion/grader implementation.*
- *(Closest-equivalent rule applies if needed: any real, maintained eval harness with a readable core.)*

### 🔬 AUTOPSY (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md — 🔬 AUTOPSY, full protocol — with a twist: this codebase is
TypeScript, my expert language, so do NOT decode TS idioms for me (mode-flip per the UI-rep rule: expert
language, beginner domain). Track 7, Module 7.5A. I've hand-rolled tiny eval runners since 2.8 and built
proper eval sets + LLM-as-judge in 7.2–7.3; now I read an industrial harness.

SETUP: Clone promptfoo (or closest equivalent), locate the core eval runner and one assertion/grader
implementation. ONE subsystem, ~60 minutes.

THE AUTOPSY: (1) PREDICT before reading: from my own run.py experience, what MUST an industrial runner
handle that mine doesn't (concurrency? caching results? retries? cost tracking? diffing runs?)? Then read
and verify — gaps out loud, logged. (2) COMPARE: their assertion/grading model vs my Solid/Shaky judgments
and my LLM-as-judge from 7.3 — what's better, what's just different, what constraint drove it? At least
one difference judged. (3) INTERROGATE: how do they treat the non-determinism — repeated runs, thresholds,
flakiness? That's the heart of the domain. (4) EXTRACT one production trick into NOTES.md — and decide,
explicitly, whether my primer evals should keep hand-rolled runners or adopt a harness, and defend the
choice.

Verdict: engineer or tourist?
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

Two lessons in one. First, the domain: an industrial eval harness is your 30-line `run.py` grown up — concurrency, cached results, model-graded assertions, run-to-run diffing, flakiness handling — and reading it shows you what evaluation looks like at production scale, so you can decide (a real engineering decision) when hand-rolled stops being enough. Second, the meta-lesson: because this codebase is in YOUR language, you experienced what reading unfamiliar-domain code feels like when the syntax costs you nothing — that's how readable ALL of this will eventually feel in Python too.

The principle: **an eval harness is the evals thread industrialized — and reading one in your native language cleanly separates domain difficulty from language difficulty, proving the domain was the easy part all along.**
</details>

---

## Module 7.6 — Evaluation & Safety Capstone: A System You Can Trust

**Difficulty:** ●●●●● · **Format:** Guided capstone · **Stack:** Python + anthropic SDK

### New words in this module

- *(None new — integrates the track.)*

### Prep — Watch & Read

**Watch (Frontend Masters):**
- Revisit **AI Engineering Fundamentals** (eval) and **AI Agent: From Prototype to Production** (safety) as a checklist.

**Read:**
- Revisit your `NOTES.md` from 7.1–7.5.

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 7, Module 7.6 — evaluation & safety capstone. Completed 7.1–7.5.
Make one of my earlier systems genuinely trustworthy. Calibrate toward independence (ZPD).

PHASE 1 STUDY (light): Map the trust checklist: a well-covered eval set + measured success rate + LLM-as-
judge for fuzzy cases (validated) + injection hardening + input/output guardrails + regression checking.

PHASE 2 BUILD: Guide me to take a real system from an earlier track (my RAG system or an agent) and make it
TRUSTWORTHY: build a proper eval set, measure it honestly, add LLM-as-judge where needed, harden it against
prompt injection, add appropriate guardrails, and set it up so I can catch regressions on future changes.
Let me drive; hint when stuck.

PHASE 3 REPORT: I report. Make me present the system's measured quality honestly (including where it's
weak), demonstrate it resists a prompt injection, and show the guardrails working — the way I'd present a
system to a skeptical senior engineer.

PHASE 4 EXERCISE 1: Have me improve the system's measured weakest area and prove the gain via the eval —
hints available.

PHASE 5 EXERCISE 2: Have me make a DIFFERENT system trustworthy solo (evals + safety + guardrails), then
the big verdict: am I ready for Track 8 (Production), or which eval/safety concept to revisit? Being able
to SHOW measured quality + safety is a massive interview differentiator — note it.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The capstone integrates the track into making a real system *trustworthy*: a well-covered **eval set** (7.2) with a measured **success rate** (7.1), **LLM-as-judge** for fuzzy cases (7.3, validated), **prompt-injection hardening** (7.4), **input/output guardrails** (7.5), and **regression checking** for future changes. This is the difference between "it works on my machine" and "I can show you it works, measured, and it's safe."

This is one of the strongest interview differentiators you can have. Most candidates can build something; very few can say "here's my eval set, here's the measured success rate, here's where it's weak and why, here's how it resists injection, here are the guardrails." That demonstrates you build *products*, not demos — exactly the senior signal.

The interview answer (whole track): *"How do you make an AI system trustworthy?" → "Evaluate it: a well-covered eval set with a measured success rate, using LLM-as-judge (validated) for fuzzy outputs. Secure it: harden against prompt injection by treating input as untrusted, and add input/output guardrails. Maintain it: re-run evals to catch regressions on every change. The result is measured, defensible quality and safety — not 'it seemed fine,' which is what separates a product from a demo."*

The principle: **a trustworthy AI system has measured quality (covered eval set, success rate, validated LLM-as-judge), security (injection hardening, guardrails), and regression protection — and being able to SHOW measured quality and safety, rather than claiming 'it works,' is one of the strongest signals that you build products, not demos.**
</details>

---

## 🧊 COLD REBUILD (do this before starting the next track — the honest gate)

**The task:** From an empty folder, given a small AI task, build its trust kit: an eval set with edge and adversarial cases, a runner reporting success rate, one LLM-as-judge criterion (with a sanity check that the judge itself is right), and one input + one output guardrail — all demonstrated.

**The rules (per AI-CONTRACT.md):** empty folder, ~20 minutes, NO notes, NO old code open. Paste this into Claude Code:

```text
Follow CLAUDE.md and AI-CONTRACT.md — this is a 🧊 COLD REBUILD. I'm cold-rebuilding Track 7's core: an eval set with adversarial cases, a runner, a validated LLM-as-judge criterion, and input/output guardrails. Time me (~20 min, soft limit).
Full Prime Directive: no worked examples, hint ladder only. When I finish (or the time is clearly gone),
grade the rebuild honestly — what came out fluently, what I fumbled, what I couldn't produce at all — log
it as evidence in LEARNING-RECORD, and give the gate verdict: proceed to the next track, or which modules
to re-run first. Recognizing is not retrieval; only what I PRODUCED counts.
```

*Why this exists: you can pattern-match through a capstone with your own notes open. You cannot fake a cold rebuild. This 20 minutes is the most honest signal in the whole track.*

---

*End of Track 7 — Evaluation, Safety & Reliability. 7 modules (including one 🔬 autopsy). You can now KNOW your AI systems work — measured across many cases with eval sets and LLM-as-judge — and make them safe against prompt injection and bad output with guardrails. This is the unglamorous, deeply professional discipline that separates shipping demos from shipping products, and almost no self-taught builder has it.*

*Next: Track 8 — Production: Cost, Caching, Streaming & Observability, where you make your AI systems fast, cheap, and operable in the real world.*
