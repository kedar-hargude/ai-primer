# TRACK 2 — PROMPTING AS ENGINEERING

*You already "prompt" Claude daily. This track turns that casual skill into a deliberate, reliable, professional engineering discipline — the difference between "I asked and got something okay" and "I built a prompt that returns exactly the right shape, every time, even when the input is weird." You'll learn structured outputs, examples, step-by-step reasoning, system prompts, and — the part that breaks your frontend instincts — how to make a stochastic (randomness-producing) model behave reliably enough to build real software on top of. This is where you stop being a Claude *user* and become a Claude *engineer*.*

*Prerequisites: Tracks 0–1 (you can call Claude, parse JSON, handle errors, and you understand tokens/temperature/statelessness). Everything here is Python + the anthropic SDK.*

*Spine courses (you own both): **Practical Prompt Engineering** — the dedicated FM course for this exact track; watch the relevant section before each module. **AI Engineering Fundamentals** — for the reliability and structured-output material. Anthropic's own prompt-engineering docs are the primary text.*

*Gentle rhythm throughout: STUDY → TINKER → REPORT → EXERCISE 1 → EXERCISE 2. Jargon decoded inline. The scaffolding tapers as you get stronger.*

*🧊 Cold opens continue every module; cold rebuild at track end. 📏 THE EVALS THREAD IS BORN IN THIS TRACK (Module 2.8): from there to the end of the primer, everything you build carries a tiny `evals/` folder and a measured success rate. It starts at just 5 test cases — the habit matters more than the size.*

---

## Module 2.1 — What Makes a Good Prompt: Clarity, Specificity, Context

**Difficulty:** ●●○○○ · **Format:** STUDY → investigation · **Stack:** Python + anthropic SDK

### New words in this module

- **Prompt**: the text you send the model — your instruction plus any context. (You know this informally; now we get precise.)
- **Zero-shot**: asking the model to do a task with no examples, just an instruction. (Most of how you use Claude now.)
- **Prompt template**: a reusable prompt with placeholders you fill in (e.g. `f"Summarize this: {text}"`). Like a function for prompts.

### Prep — Watch & Read

**Watch (Frontend Masters — you own these):**
- **Practical Prompt Engineering** → the opening sections on what makes prompts effective (clarity, specificity, giving context). Your main reference for this whole track.
- **AI Engineering Fundamentals** → the prompting-basics section.

**Read (gentle → deeper):**
- **Anthropic — "Be clear and direct"** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct — *Focus on: specificity, telling the model exactly what you want.*
- **Anthropic — Prompt engineering overview** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview — *Focus on: the menu of techniques (you'll learn each in this track).*
- **"Prompt engineering guide — basics"** — https://www.promptingguide.ai/introduction — *Focus on: the elements of a prompt (instruction, context, input, output format).*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 2, Module 2.1 — what makes a good prompt. I prompt Claude
casually already; make me deliberate about it. Gentle rhythm.

PHASE 1 STUDY: Write me a script that runs the SAME task with a vague prompt and a clear, specific,
context-rich prompt, side by side, so I SEE the quality difference. Walk me through what made the good
one good (clear instruction, specificity, context, output format). Introduce "prompt template" (a reusable
f-string prompt) and "zero-shot."

PHASE 2 TINKER: Have me take a deliberately bad prompt and improve it step by step, running each version
and observing the output improve. Make me predict which change will help most.

PHASE 3 REPORT: I report. Make me explain TWO-LEVEL: (1) plain — what makes a prompt good? (2) technical —
WHY (connect to Track 1: the prompt is the conditioning text that shapes next-token prediction, so
specificity narrows the distribution toward what I want). Name the interview question and make me answer.

PHASE 4 EXERCISE 1: Have me write a reusable prompt template for a real task (e.g. summarize-with-
constraints) and test it on several inputs — hints free.

PHASE 5 EXERCISE 2: A different prompt-improvement task solo — take a vague prompt, make it excellent,
prove the improvement. Verify, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

A good prompt is **clear, specific, and gives the model the context it needs**, including the desired output format. Vague prompts give vague results; specific prompts narrow the model toward exactly what you want.

**The theory underneath (connects to Track 1):** the prompt is the *conditioning text* that shapes next-token prediction. A vague prompt leaves the probability distribution wide — many plausible continuations, most not what you wanted. A specific prompt with context and a clear format narrows the distribution toward your target. So "prompt engineering" isn't magic incantations — it's *deliberately shaping the conditioning text to steer the prediction*.

A **prompt template** (a reusable prompt with placeholders, like a function) is how you make this repeatable in real software, instead of hand-writing each prompt.

The interview answer: *"What makes a good prompt?" → "Clarity, specificity, relevant context, and an explicit output format. Mechanically, the prompt is the conditioning text for next-token prediction — being specific narrows the model's distribution toward the desired output, so vagueness is the main cause of bad results."*

The principle: **a good prompt is clear, specific, and context-rich because the prompt is the conditioning text that steers next-token prediction — and templating makes good prompts reusable in real software.**
</details>

---

## Module 2.2 — System Prompts: Setting the Model's Role and Rules

**Difficulty:** ●●○○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **System prompt**: a special instruction (separate from the user message) that sets the model's persona, rules, and constraints for the whole conversation. It's like the "standing orders" — it applies to every turn.
- **Role** (`system` / `user` / `assistant`): the three kinds of messages. System = standing instructions; user = you; assistant = the model's replies.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **Practical Prompt Engineering** → the system-prompt / role-setting sections.
- **AI Engineering Fundamentals** → how system prompts shape behavior.

**Read:**
- **Anthropic — System prompts / Giving Claude a role** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/system-prompts — *Focus on: using the system parameter to set persona and rules.*
- **"System prompt vs user prompt"** — search `"system prompt vs user prompt difference llm"` — *Focus on: standing instructions vs the per-turn request.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 2, Module 2.2 — system prompts. Gentle rhythm.

PHASE 1 STUDY: Write a script that runs the same user question with NO system prompt, then with a system
prompt that sets a clear role and rules (e.g. "You are a terse senior engineer; answer in at most 3
bullet points"), so I see how the system prompt steers tone, format, and behavior across the whole chat.
Explain the system/user/assistant roles and where the system prompt goes in the SDK call.

PHASE 2 TINKER: Have me write several different system prompts (a strict one, a friendly one, one that
enforces an output format) and observe how each changes the same conversation. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what a system prompt does and WHY it's powerful (it's
persistent conditioning applied to every turn, vs a user message that's just one turn). Name the
interview question and answer it.

PHASE 4 EXERCISE 1: Have me build a small "assistant with a personality and rules" using a system prompt
for a specific use case (e.g. a coding helper that always explains its reasoning) — hints free.

PHASE 5 EXERCISE 2: A different system-prompted assistant solo (different role/rules), prove it behaves
consistently, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

A **system prompt** sets the model's persona, rules, and constraints for the *entire* conversation — it's the "standing orders" applied to every turn, separate from the per-turn user message. The three **roles** are `system` (standing instructions), `user` (your messages), and `assistant` (the model's replies).

**Why it's powerful (theory):** because the system prompt is included in the context on every call, it's *persistent conditioning* — it shapes next-token prediction for every response, not just one. That's why it's the right place for tone, format rules, role, and guardrails, while the user message carries the specific request.

The interview answer: *"What's the difference between a system prompt and a user prompt?" → "The system prompt sets persistent role, rules, and constraints applied to every turn of the conversation; the user prompt is a single turn's request. Mechanically the system prompt is conditioning included on every call, so it steers all responses, which is why persona and guardrails go there."*

The principle: **the system prompt is persistent conditioning that sets the model's role and rules for the whole conversation — the right home for persona, format, and guardrails — while user messages carry per-turn requests.**
</details>

---

## Module 2.3 — Few-Shot Prompting: Teaching by Example

**Difficulty:** ●●●○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Few-shot**: giving the model a few examples of input→output *inside the prompt* so it learns the pattern you want, before your real input. ("Shot" = example.)
- **One-shot / zero-shot**: one example / no examples. Few-shot = a handful.
- **In-context learning**: the model "learning" the pattern from examples in the prompt — without any retraining. It's not really learning; it's conditioning on the examples.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **Practical Prompt Engineering** → the examples / few-shot sections.

**Read:**
- **Anthropic — Use examples (multishot prompting)** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/multishot-prompting — *Focus on: showing examples to lock in format and behavior.*
- **"Few-shot prompting"** — https://www.promptingguide.ai/techniques/fewshot — *Focus on: how examples improve consistency.*
- **"In-context learning explained"** — search `"in context learning few shot llm how it works"` — *Focus on: the model conditions on examples; no weights change.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 2, Module 2.3 — few-shot prompting. Gentle rhythm.

PHASE 1 STUDY: Write a script for a task where zero-shot gives inconsistent output (e.g. classify text
into custom categories, or format something in a specific unusual way), then add 2–3 examples (few-shot)
and show the output become consistent and correctly formatted. Explain "few-shot," "shot," and
"in-context learning" plainly.

PHASE 2 TINKER: Have me add/remove/change examples and watch the effect; try a tricky input the examples
didn't cover and see what happens; make the examples better and observe improvement. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what few-shot is and WHY it works (the examples are
conditioning context that shows the model the exact pattern, narrowing the distribution toward that
format — connect to Track 1). Why is it called "in-context learning" but isn't real learning? Name the
interview question and answer.

PHASE 4 EXERCISE 1: Have me build a few-shot prompt for a real classification or formatting task and test
it on varied inputs — hints free.

PHASE 5 EXERCISE 2: A different few-shot task solo (different domain), prove the examples improve
consistency vs zero-shot, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Few-shot prompting** gives the model a few examples of input→output inside the prompt, before your real input, so it picks up the exact pattern/format you want. It dramatically improves consistency for tasks where a bare instruction is ambiguous (custom classification, unusual formatting, specific style).

**Why it works (theory):** the examples are *conditioning context*. The model is a next-token predictor; showing it "input X → output Y" several times makes the pattern that produces Y highly probable for your new input. It's called **in-context learning**, but no weights change and nothing is permanently learned — the model is simply conditioning on the examples in this one prompt. Remove the examples and the "learning" is gone.

The interview answer: *"What's few-shot prompting and why does it work?" → "It's including a few input→output examples in the prompt so the model follows that pattern. It works because the examples are conditioning context that makes the desired output pattern high-probability for the new input — 'in-context learning,' though no model weights change; it's pure conditioning within that prompt."*

The principle: **few-shot prompting conditions the model on examples in the prompt to lock in a pattern or format — it's in-context learning (conditioning, not real training), and it's the go-to fix when a zero-shot instruction is too ambiguous for consistent output.**
</details>

---

## Module 2.4 — Chain-of-Thought: Making the Model Reason Step by Step

**Difficulty:** ●●●○○ · **Format:** STUDY → investigation · **Stack:** Python + anthropic SDK

### New words in this module

- **Chain-of-thought (CoT)**: prompting the model to *think step by step* and show its reasoning before giving the final answer — which improves accuracy on reasoning-heavy tasks.
- **Reasoning tokens**: the intermediate "thinking" the model generates before the answer. (Some models have a dedicated thinking mode.)

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **Practical Prompt Engineering** → the chain-of-thought / "let it think" sections.
- **AI Engineering Fundamentals** → reasoning-improvement techniques.

**Read:**
- **Anthropic — Let Claude think (chain of thought)** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/chain-of-thought — *Focus on: asking for step-by-step reasoning before the answer.*
- **"Chain-of-thought prompting"** — https://www.promptingguide.ai/techniques/cot — *Focus on: why it helps on multi-step problems.*
- **"Why does step-by-step improve LLM accuracy"** — search `"chain of thought why it works next token"` — *Focus on: each reasoning step conditions the next, building toward a better answer.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 2, Module 2.4 — chain-of-thought reasoning. Gentle rhythm,
and I'll enjoy the WHY here.

PHASE 1 STUDY: Write a script with a multi-step reasoning problem (e.g. a word problem or a multi-
constraint task) prompted two ways: "just answer" vs "think step by step, then answer." Show the
step-by-step version is more accurate. Explain chain-of-thought and reasoning tokens.

PHASE 2 TINKER: Have me try CoT on a few problems, including one where the no-CoT version gets it wrong
and CoT gets it right; have me make the model show then hide its reasoning. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what CoT is and WHY it works (connect to Track 1: each
reasoning step the model generates becomes part of the conditioning context for the next step, so it can
build up to an answer instead of predicting it in one leap). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me apply CoT to a real reasoning task and show it beats a direct prompt — hints
free.

PHASE 5 EXERCISE 2: A different reasoning task solo where I design the CoT prompt, prove the improvement,
then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Chain-of-thought (CoT)** prompting asks the model to think step by step and show its reasoning before the final answer, which significantly improves accuracy on multi-step/reasoning tasks. The intermediate steps are **reasoning tokens**.

**Why it works (the theory you'll love):** the model is a next-token predictor generating one token at a time. Asking for a direct answer forces it to predict the conclusion in essentially one leap. Asking it to reason step by step makes it generate intermediate steps — and *each step becomes part of the conditioning context for the next*, so the model builds toward the answer incrementally, the way showing your work helps a person. The reasoning literally gives the model more relevant context (that it generated itself) to condition the final answer on.

The interview answer: *"Why does 'think step by step' improve accuracy?" → "Because the model predicts one token at a time; generating intermediate reasoning steps adds them to the conditioning context, so each step informs the next and the final answer is conditioned on the worked-out reasoning rather than predicted in a single leap. It's the model 'showing its work,' which improves multi-step accuracy."*

The principle: **chain-of-thought prompting makes the model generate reasoning steps that become conditioning context for the next steps, so it builds toward an answer incrementally instead of guessing it in one leap — which is why 'think step by step' reliably improves reasoning accuracy.**
</details>

---

## Module 2.5 — Structured Outputs: Getting Reliable JSON

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK + json

### New words in this module

- **Structured output**: making the model return data in a specific machine-readable format (usually JSON) instead of free text, so your code can use it reliably.
- **Schema**: the expected shape of the JSON (which fields, what types). Like a TypeScript interface for the output.
- **Parsing failure**: when the model returns something that isn't valid JSON (or has the wrong shape), and your `json.loads` blows up. The thing you must handle.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → the structured-output / JSON-mode sections (critical for building real apps).
- **Practical Prompt Engineering** → output-formatting sections.

**Read:**
- **Anthropic — Increase output consistency (JSON)** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/increase-consistency — *Focus on: techniques to get reliable structured output (specifying the schema, prefilling).*
- **Anthropic — Tool use for structured output / prefill** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response — *Focus on: prefilling `{` to force JSON.*
- **"Reliable JSON from LLMs"** — search `"reliable json output from llm parsing failures"` — *Focus on: validating and handling malformed output.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 2, Module 2.5 — structured outputs (reliable JSON). This is
where I learn to make the model's output USABLE by code. The non-determinism rule kicks in here. Gentle
rhythm but push me on reliability.

PHASE 1 STUDY: Write a script that asks the model to return JSON in a specific schema (e.g. extract
{name, date, sentiment} from a review), parses it with json.loads, and uses it. Show techniques that make
the JSON reliable (clearly specifying the schema, low temperature, prefilling the opening brace). Explain
"schema" (like a TS interface) and why parsing can fail.

PHASE 2 TINKER: Have me feed weird/edge-case inputs and watch the JSON sometimes break or drift from the
schema. Make me SEE a parsing failure (and read the error). Then have me add handling: try/except around
the parse, validation of the shape, a retry. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: how to get reliable structured output and WHY it's never
100% guaranteed (the model samples tokens; it can drift) — so I must validate and handle failure. Connect
to non-determinism (Track 1.5). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a robust "extract structured data from text" function that returns
validated JSON and gracefully handles malformed output (retry or fallback) — hints free.

PHASE 5 EXERCISE 2: A different structured-extraction task solo, with proper validation and failure
handling, tested on messy inputs. Verify the failure path actually works, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Structured output** means making the model return machine-readable data (usually JSON in a specific **schema** — the expected fields and types, like a TS interface) so your code can use it reliably instead of parsing free text. Techniques: specify the schema explicitly, use low temperature (1.5) for consistency, and *prefill* the assistant's response with `{` to force it into JSON.

**The crucial lesson (non-determinism, made real):** because the model **samples tokens**, structured output is *never 100% guaranteed* — it can occasionally return malformed JSON or drift from the schema, especially on weird inputs. So a real system ALWAYS: wraps the parse in `try`/`except`, **validates the shape** (right fields, right types), and has a **fallback** (retry, or a default). This is the exact "handle the failure path" discipline — a demo ignores it, a real system depends on it.

The interview answer: *"How do you get reliable structured output from an LLM, and why isn't it guaranteed?" → "Specify the schema explicitly, lower the temperature, and prefill the opening brace to force JSON. But because the model samples tokens, output can still be malformed or off-schema — so I always parse defensively (try/except), validate the shape, and add a retry or fallback. Reliability comes from validation, not from trusting the model."*

The principle: **structured output makes the model return code-usable JSON via schema-specification, low temperature, and prefilling — but because the model samples, it's never guaranteed, so robust systems validate the shape and handle parsing failures with retries/fallbacks.** This is the first place your frontend "the API returns a guaranteed shape" instinct must be replaced with "validate everything."
</details>

---

## Module 2.6 — Prompt Failure Modes & How to Debug Them

**Difficulty:** ●●●○○ · **Format:** STUDY → investigation · **Stack:** Python + anthropic SDK

### New words in this module

- **Failure mode**: a characteristic way a prompt goes wrong (ignores an instruction, hallucinates, wrong format, refuses, gets distracted by part of the input).
- **Prompt injection** (preview): when text *inside the input* tricks the model into ignoring your instructions. (Covered fully in Track 7; introduced here.)
- **Iterating**: improving a prompt through cycles of test → observe failure → adjust.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **Practical Prompt Engineering** → the debugging / common-mistakes / iteration sections.
- **AI Engineering Fundamentals** → reliability and failure-handling.

**Read:**
- **Anthropic — prompt engineering (troubleshooting across the guides)** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview — *Focus on: when a technique fails, which other technique to reach for.*
- **"Common prompt engineering mistakes"** — search `"common prompt engineering mistakes and fixes"` — *Focus on: ambiguity, conflicting instructions, overlong prompts.*
- **"Prompt injection intro"** — search `"prompt injection explained simple"` — *Focus on: the concept; full treatment later.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 2, Module 2.6 — prompt failure modes & debugging. Gentle
rhythm. This builds my debugging intuition for prompts.

PHASE 1 STUDY: Show me a gallery of broken prompts, each failing a DIFFERENT way (ignores an instruction;
hallucinates; wrong format; gets distracted by part of the input; conflicting instructions), and for each,
diagnose WHY and fix it. Briefly introduce prompt injection as one failure mode (full coverage later).
Teach me the debugging loop: test → observe the specific failure → form a hypothesis → adjust → re-test.

PHASE 2 TINKER: Hand me a couple of subtly broken prompts and have ME diagnose and fix them, running to
confirm. Make me name the failure mode each time.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the common failure modes and how I'd systematically debug a
misbehaving prompt. Name the interview question and answer.

PHASE 4 EXERCISE 1: Give me a prompt that fails in a non-obvious way; have me diagnose and fix it,
naming the failure mode — hints free.

PHASE 5 EXERCISE 2: A different broken prompt solo — diagnose, fix, prove it works on edge cases, then the
verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

Prompts fail in characteristic **failure modes**: ignoring an instruction (often buried or conflicting with another), hallucinating (1.1), wrong/inconsistent format (fix with few-shot + structured-output techniques), getting distracted by part of the input, or being subverted by **prompt injection** (malicious text in the input — Track 7). The debugging loop mirrors any engineering debugging: **test → observe the specific failure → hypothesize the cause → adjust one thing → re-test.**

**The theory thread:** most failures trace back to the conditioning being wrong — ambiguous instructions leave the distribution too wide, conflicting instructions pull it in two directions, important instructions buried in the middle get "lost" (1.4). Debugging a prompt is debugging the conditioning.

The interview answer: *"How do you debug a prompt that misbehaves?" → "Identify the specific failure mode — ignored instruction, hallucination, wrong format, distraction, injection — then iterate: change one thing, re-test. Most failures come from ambiguous or conflicting conditioning, instructions buried where attention is weak, or expecting structure without enforcing it, so the fixes are specificity, examples, structure, and placement."*

The principle: **prompts fail in identifiable modes (ignored instructions, hallucination, format drift, distraction, injection), and you debug them by isolating the failure mode and iterating one change at a time — because a misbehaving prompt is almost always a conditioning problem.**
</details>

---

## Module 2.7 — Prompt Templates & Building a Reusable Prompt Library

**Difficulty:** ●●●○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Prompt template** (now formalized): a parameterized prompt — fixed structure with `{placeholders}` you fill at runtime. The reusable unit of real AI software.
- **Prompt versioning**: keeping track of different versions of a prompt as you improve it (so you can compare and roll back). Like git for prompts, conceptually.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **Practical Prompt Engineering** → any sections on reuse, templates, and maintaining prompts.
- **AI Engineering Fundamentals** → structuring prompts in a real codebase.

**Read:**
- **Anthropic — Prompt templates and variables** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/prompt-templates-and-variables — *Focus on: separating fixed structure from variable input.*
- **"Managing prompts in production"** — search `"managing prompt templates versioning production"` — *Focus on: why prompts deserve the same care as code.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 2, Module 2.7 — prompt templates & a reusable library. This
makes my prompting production-ready. Gentle rhythm but encourage independence (I've done a few modules now).

PHASE 1 STUDY: Show me how to structure prompts as reusable TEMPLATES in Python (functions or f-string
templates that take parameters), cleanly separated from the calling code, and a simple way to organize a
small library of them. Touch on prompt versioning (keeping/labeling versions as I improve them).

PHASE 2 TINKER: Have me convert a couple of inline prompts into clean parameterized templates and call
them with different inputs; tweak a template and see all its uses benefit.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: why templating/versioning matters in a real codebase (reuse,
consistency, maintainability, ability to improve one place). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a small prompt library (3–4 reusable templates for related tasks) with
a clean interface — hints available, but let me lead.

PHASE 5 EXERCISE 2: Have me design and build a different small prompt library solo for a new use case,
then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

A **prompt template** is a parameterized prompt — fixed, well-engineered structure with `{placeholders}` filled at runtime — and it's the reusable unit of real AI software. Instead of hand-writing prompts inline, you build a small **library** of templates, cleanly separated from your calling code, so prompts are reusable, consistent, and maintainable. **Prompt versioning** (tracking versions as you improve a prompt) lets you compare and roll back, treating prompts with the same discipline as code.

**Why it matters (engineering maturity):** in a real app, the same prompt logic is used in many places; templating means you improve it once and everywhere benefits, and you can test and version it. This is the difference between scattered ad-hoc prompts and a maintainable prompt layer — a sign of an engineer, not a tinkerer.

The interview answer: *"How do you manage prompts in a real codebase?" → "As reusable, parameterized templates separated from calling code, organized into a small library, and versioned so I can compare and roll back. Prompts are core logic, so they get the same engineering care as code — reuse, testing, and version control."*

The principle: **prompt templates turn one-off prompts into reusable, parameterized, versioned units — the maintainable prompt layer of real AI software — so prompting becomes engineering, not ad-hoc tinkering.**
</details>

---
## Module 2.8 — Making a Stochastic Model Reliable Enough to Build On

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Reliability**: the system produces correct, usable output a high percentage of the time, even across varied/weird inputs — not just once in a demo.
- **Retry / fallback**: re-attempting a failed call (maybe with an adjusted prompt), or falling back to a safe default when it keeps failing.
- **Validation**: checking the output meets your requirements (right shape, allowed values) before trusting it.
- **Guardrails** (preview): checks that catch bad output before it reaches the user. (Full treatment in Track 7.)

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → the reliability / production-readiness sections.
- **AI for Software Engineers** → building dependable software on top of unpredictable models.

**Read:**
- **Anthropic — Reduce hallucinations / increase consistency** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/reduce-hallucinations — *Focus on: techniques that raise reliability.*
- **"Building reliable LLM applications"** — search `"building reliable llm application validation retry"` — *Focus on: validate, retry, fall back.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 2, Module 2.8 — reliability: building dependable software on an
unpredictable model. This is THE mindset shift from my deterministic frontend habits. Gentle rhythm but
push hard on the "works reliably, not just once" point.

PHASE 1 STUDY: Write a script that takes a task and makes it RELIABLE: validate the output (shape +
allowed values), retry on failure (with a tweak), and fall back to a safe default if it still fails — and
run it across many varied inputs to measure how often it succeeds. Explain reliability, validation, retry,
fallback, and preview guardrails.

PHASE 2 TINKER: Have me throw adversarial/weird inputs at a naive version and watch it fail, then at the
robust version and watch it hold up. Make me measure a rough success rate before and after.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: why "it worked once" ≠ "it works reliably," and the standard
reliability toolkit (validate, retry, fallback) — rooted in the model being stochastic (1.5). Name the
interview question and answer.

PHASE 4 EXERCISE 1: Have me take an unreliable single-shot task and make it robust (validation + retry +
fallback), proving the improved success rate across many inputs — hints free.

PHASE 5 EXERCISE 2: A different task solo — make it reliable, measure before/after success across varied
inputs, then the verdict. This is a core production skill; the verdict should reflect whether I've
internalized "build for the failure case."

📏 EVALS THREAD — THIS IS WHERE IT'S BORN: In Exercise 1, have me turn my "many varied inputs" into a
PERMANENT artifact: an `evals/` folder with `cases.json` (5 cases: input + what correct looks like) and
`run.py` (a ~30-line runner that runs all cases and prints a success rate). Teach me the shape once,
gently — this is the first one. Then in Exercise 2 I build the evals folder for the new task MYSELF.
From this module to the end of the primer, every artifact I build carries one of these, and no Solid
verdict exists without a measured number. Explain WHY this discipline exists: a stochastic system can
only be trusted statistically, so "tested" must always mean "measured."
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

This is the mindset shift from frontend: an LLM is **stochastic** (1.5), so you cannot treat its output as a guaranteed-correct return value. **Reliability** means the system produces correct, usable output a high percentage of the time across varied inputs — and you achieve it with a standard toolkit: **validate** the output (right shape, allowed values) before trusting it, **retry** on failure (often with a small prompt adjustment), and **fall back** to a safe default if it keeps failing. **Guardrails** (Track 7) extend this to catching unsafe output.

**The key realization:** "it worked once in my demo" is not "it works reliably for real users." A demo runs the happy path once; a real system runs across thousands of weird inputs, where the model *will* occasionally misbehave. Engineering the failure path — validate, retry, fallback — is what makes AI software dependable, and it's exactly what most self-taught builders skip (and what interviewers probe).

The interview answer: *"How do you build reliable software on an unpredictable model?" → "Treat output as untrusted: validate its shape and values, retry on failure with adjustments, and fall back to a safe default. Because the model samples tokens it will occasionally misbehave, so reliability comes from engineering the failure path and measuring success rate across varied inputs — not from assuming the model is correct."*

The principle: **an LLM is stochastic, so reliability is engineered through validation, retries, and fallbacks measured across many inputs — 'it worked once' is never 'it works reliably,' and building the failure path is the core production discipline that separates a demo from a product.**

**And one more thing was born here:** your first `evals/` folder — 5 cases and a runner that prints a success rate. It looks tiny. It is the single most professional habit in this primer: from now on, every claim you make about a system you built comes with a number attached. By Track 7, when evaluation becomes the explicit topic, you'll realize you've been doing it for months — that's the plan working.
</details>

---

## Module 2.9 — Prompt Caching & Efficient Prompting (a first taste)

**Difficulty:** ●●●○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Prompt caching**: reusing the model's processing of a large, repeated chunk of prompt (e.g. a long system prompt or document) across calls, so you don't pay to re-process it every time. Saves cost and latency.
- **Cache hit / miss**: whether a cached chunk was reused (hit) or had to be reprocessed (miss).

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → cost/efficiency sections (prompt caching if covered).

**Read:**
- **Anthropic — Prompt caching** — https://docs.claude.com/en/docs/build-with-claude/prompt-caching — *Focus on: marking a stable prefix as cacheable to cut cost/latency on repeated calls.*
- **"Prompt caching explained"** — search `"llm prompt caching cost savings how it works"` — *Focus on: reuse of a stable prefix.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 2, Module 2.9 — prompt caching (a first taste of efficiency).
Gentle rhythm. (Deeper production caching comes in Track 8; this is the prompting-side intro.)

PHASE 1 STUDY: Write a script that calls the model repeatedly with a large STABLE chunk (e.g. a long
system prompt or a document) plus a small changing question. Show how to mark the stable part as
cacheable, and from the usage data show the cost/latency drop on cached calls. Explain prompt caching,
cache hit/miss, and why only stable prefixes are cacheable.

PHASE 2 TINKER: Have me run with and without caching and compare cost/latency from the usage info; change
the stable chunk and watch the cache miss. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what prompt caching does and WHY it works (the stable prefix's
processing is reused — connect to tokens/cost from Track 1.6). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me apply caching to a realistic repeated-large-context scenario and measure the
savings — hints free.

PHASE 5 EXERCISE 2: A different caching scenario solo, measure the savings, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Prompt caching** lets you reuse the model's processing of a large, *stable* chunk of prompt (a long system prompt, a document, a big set of examples) across many calls — so you don't pay (in cost or latency) to re-process that chunk every time. You mark a stable prefix as cacheable; subsequent calls that share it get a **cache hit** (cheap/fast); change the prefix and you get a **cache miss** (reprocessed).

**Why it works (theory, ties to Track 1):** processing tokens costs compute. If the first N tokens are identical across calls, their processing can be reused. This only works for a *stable prefix* (the unchanging start), because the model processes left-to-right and caching depends on an identical prefix — which is why your changing question goes at the *end*, after the cacheable stable part.

**Why it matters:** real apps often send the same big system prompt or document on every call. Caching it can cut cost dramatically — directly relevant to your "build real things affordably" goal.

The interview answer: *"What is prompt caching and when does it help?" → "It reuses the processing of a stable prompt prefix across calls, cutting cost and latency when you repeatedly send the same large context (system prompt, document, examples). It requires the prefix to be identical, so the stable part goes first and the changing input last. It's a major cost optimization for repeated-large-context workloads."*

The principle: **prompt caching reuses the processing of a stable prompt prefix across calls to cut cost and latency — so you structure prompts with the large stable content first (cacheable) and the changing input last, a key efficiency lever for real apps.**
</details>

---

## Module 2.10 — Prompting Capstone: Build a Reliable, Structured AI Feature

**Difficulty:** ●●●●● · **Format:** Guided capstone · **Stack:** Python + anthropic SDK

### New words in this module

- *(None new — this integrates the whole track.)*

### Prep — Watch & Read

**Watch (Frontend Masters):**
- Revisit whichever **Practical Prompt Engineering** sections you found hardest — this capstone uses all of them.

**Read:**
- Revisit your `NOTES.md` from 2.1–2.9.
- **Anthropic — prompt engineering overview (full menu)** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview — *Use as a checklist of techniques to apply.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 2, Module 2.10 — prompting capstone. Completed 2.1–2.9. This
integrates the whole track. I'm stronger now — calibrate (ZPD): less hand-holding, but rescue me if I
stall.

PHASE 1 STUDY (light): Briefly map the techniques I now have (clarity, system prompts, few-shot, CoT,
structured output, failure handling, templates, reliability, caching) as a checklist for the capstone.

PHASE 2 BUILD: Guide me to build ONE real, useful AI feature that combines them — e.g. a "review analyzer"
that takes messy product reviews and returns reliable structured JSON {sentiment, key_points, summary,
rating_guess} — using: a good system prompt, few-shot examples, CoT where it helps, structured output
with validation, retry/fallback for reliability, a clean prompt template, and caching for any stable
context. Let me drive; hint where I stall. Measure its reliability across many varied reviews.

PHASE 3 REPORT: I report. Make me justify each technique choice and prove the feature is RELIABLE (not just
working once) with a success rate across varied inputs.

PHASE 4 EXERCISE 1: Have me extend the feature (a new field, a new input type) applying the same rigor —
hints available.

PHASE 5 EXERCISE 2: Have me build a DIFFERENT reliable structured AI feature solo (e.g. an email-to-
structured-task extractor), combining the whole track, measured for reliability. Big verdict: am I ready
to leave prompting and move to Track 3 (Embeddings), or which technique to revisit? Confirm I could build
a reliable AI feature in an interview/take-home.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The capstone integrates the whole track into one real feature, the way actual AI software is built: a good **system prompt** sets role and rules; **few-shot examples** lock in the format; **chain-of-thought** improves any reasoning step; **structured output** with **validation** makes it code-usable; **retry/fallback** makes it **reliable** across messy real inputs; a **prompt template** makes it reusable; **caching** makes any stable context cheap. And critically, you *measure reliability across many inputs* rather than trusting one good demo run.

This is the difference between "I can get Claude to do something" and "I can build a dependable AI feature a real product can rely on" — the latter is what gets you hired.

The interview answer (the whole track): *"How do you build a reliable AI feature?" → "Engineer the prompt (clear instruction, system prompt, few-shot examples, chain-of-thought where needed), force structured output and validate it, handle failure with retries and fallbacks, template it for reuse, cache stable context for cost, and measure reliability across varied inputs — because the model is stochastic, so dependability comes from engineering around it, not from the prompt alone."*

The principle: **a reliable AI feature combines every prompting technique — clarity, system prompt, examples, reasoning, structured-and-validated output, retry/fallback, templating, caching — and proves itself by measured reliability across varied inputs, not a single demo.** You're now a prompt *engineer*, not just a prompt user.
</details>

---

## 🧊 COLD REBUILD (do this before starting the next track — the honest gate)

**The task:** From an empty folder, build a reliable structured-extraction task: a prompt that returns validated JSON, with retry and fallback — plus its `evals/` folder (5+ cases and a runner) reporting a success rate.

**The rules (per AI-CONTRACT.md):** empty folder, ~20 minutes, NO notes, NO old code open. Paste this into Claude Code:

```text
Follow CLAUDE.md and AI-CONTRACT.md — this is a 🧊 COLD REBUILD. I'm cold-rebuilding Track 2's core: validated-JSON extraction with retry/fallback, plus its evals folder reporting a success rate. Time me (~20 min, soft limit).
Full Prime Directive: no worked examples, hint ladder only. When I finish (or the time is clearly gone),
grade the rebuild honestly — what came out fluently, what I fumbled, what I couldn't produce at all — log
it as evidence in LEARNING-RECORD, and give the gate verdict: proceed to the next track, or which modules
to re-run first. Recognizing is not retrieval; only what I PRODUCED counts.
```

*Why this exists: you can pattern-match through a capstone with your own notes open. You cannot fake a cold rebuild. This 20 minutes is the most honest signal in the whole track.*

---

*End of Track 2 — Prompting as Engineering. 10 modules. Your daily Claude use is now a deliberate engineering discipline: you can shape the model's behaviour, force reliable structured output, make a stochastic model dependable, and build a real AI feature you'd stake a product on.*

*Next: Track 3 — Embeddings & Vector Search, where you learn how meaning becomes math — the foundation that makes RAG (and the "search your own documents" superpower) possible.*
