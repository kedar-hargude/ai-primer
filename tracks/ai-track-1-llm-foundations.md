# TRACK 1 — FOUNDATIONS: HOW LLMs ACTUALLY WORK

*Now that you can comfortably run Python and call Claude (Track 0), this track teaches you what the thing you're calling actually IS — not as magic, but as mechanism. It turns "the AI does stuff" into "a model predicts the next token, here's exactly how, and here's what that one fact explains about everything it does well and badly." This is the theory foundation your theory-loving brain has been waiting for, and it's what makes you sound like someone who truly understands AI in an interview, not someone who just calls an API. By the end, nothing about LLM behaviour — hallucination, cost, context limits, randomness — will feel mysterious.*

*Prerequisites: Track 0 (you can run Python, call Claude, parse JSON, handle errors). A few dollars of Anthropic API usage across the track.*

*This track leans on two Frontend Masters courses you own: **AI Engineering Fundamentals** (the practical "how LLMs work and how to use them" backbone) and, for the deeper theory you'll love, **The Hard Parts of AI: Neural Networks** (optional but perfect for your theory appetite — dip into it where noted for the under-the-hood mechanism). **AI for Software Engineers** also frames a lot of this well.*

*Each module uses the gentle rhythm from AI-CONTRACT.md: **STUDY** (Claude writes a worked example, you interrogate it) → **TINKER** (you modify and observe) → **REPORT** (you explain back) → **EXERCISE 1** (you build, hints free) → **EXERCISE 2** (transfer, verified, verdict). You're never asked to build cold.*

*🧊 From this track onward, every module opens COLD: a ≤3-minute, no-notes quiz on earlier material before any new teaching (see AI-CONTRACT.md). Don't dread it — it's how the primer refuses to let your knowledge decay silently. The track ends with a timed COLD REBUILD.*

---

## Module 1.1 — What a Language Model Actually Is: Next-Token Prediction

**Difficulty:** ●●○○○ · **Format:** STUDY → investigation · **Stack:** Python + anthropic SDK

### New words in this module

- **Token**: a chunk of text the model works in (roughly a word or word-piece) — covered properly in Module 1.3; for now just "a piece of text."
- **Next-token prediction**: the model's one job — given the text so far, predict the most likely next chunk, then the next, and so on.
- **Autoregressive**: a fancy word meaning "each new piece is predicted using everything that came before it (including its own previous outputs)."
- **Hallucination**: when the model states something false but plausible-sounding, as a natural result of predicting *plausible* text rather than *true* text.

### Prep — Watch & Read

**Watch (Frontend Masters — you own these):**

- **AI Engineering Fundamentals** → the "how do LLMs work / what is a language model" sections. Your main practical reference for this module.
- **The Hard Parts of AI: Neural Networks** → the early conceptual sections on what a neural network/model is doing. Optional but great for your theory love — dip in for the mechanism, don't get lost in the deep math.

**Read (gentle → deeper, for the theory lover):**

- **Andrej Karpathy — "Intro to Large Language Models" (1hr talk)** — search `"Karpathy intro to large language models"` — *Focus on: the "it's a next-word predictor trained on the internet" framing. The single best beginner-friendly mental model.*
- **"How LLMs work, explained simply"** — search `"how do large language models work next token prediction simple"` — *Focus on: prediction → append → repeat.*
- **Why models hallucinate** — search `"why do llms hallucinate plausible not true"` — *Focus on: hallucination as a consequence of the prediction mechanism, not a random glitch.*
- *(Surrounding context, optional)* **3Blue1Brown — "But what is a neural network / transformer"** (YouTube) — *Focus on: visual intuition only; skip the heavy math for now. Great for a visual, theory-loving learner.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 1, Module 1.1 — what an LLM fundamentally IS (next-token
prediction). I can call Claude now (Track 0). I LOVE theory and want to understand the mechanism, but
I'm still a beginner — use the gentle rhythm and decode jargon. This is a concept module, so STUDY here
means "show me experiments that reveal the mechanism," not a big program.

PHASE 1 STUDY: Write me a small, commented script that makes the model's next-token nature VISIBLE — e.g.
give it a half-finished sentence and show how it continues, and show how the continuation depends
entirely on the preceding text. Walk me through what's happening conceptually: predict next piece →
append → repeat (autoregressive). Explain it plainly first, then a bit deeper for my theory appetite.
Let me ask anything.

PHASE 2 TINKER: Have me change the starting text and watch the continuation change; set up a prompt that
makes the model confidently state something FALSE (a plausible-but-wrong "fact") so I witness
hallucination as a direct result of "predict plausible text." Predict-then-run.

PHASE 3 REPORT: I report what I saw. Then make me explain, TWO-LEVEL: (1) plain — what is a language model
doing, to a non-techie? (2) technical — what does "autoregressive next-token prediction" mean, and why
does it make hallucination intrinsic (plausible ≠ true)? Name the interview question and make me answer.

PHASE 4 EXERCISE 1: Have me design and run my own little experiment that demonstrates next-token
prediction shaping the output — hints free.

PHASE 5 EXERCISE 2: A DIFFERENT experiment solo — e.g. show that a strong leading pattern makes the model
continue that pattern (proving output is shaped by conditioning text, not "understanding"). Verify, then
the Solid/Shaky/Not-yet verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The single most important idea in the whole field: an LLM is a **next-token predictor**. Given the text so far, it predicts the most likely next chunk (token), appends it, and repeats. **Autoregressive** names this — each new token is conditioned on all prior tokens, including its own just-generated ones.

**Everything emerges from this.** Answering = predicting the tokens that plausibly follow a question. Reasoning = predicting tokens that look like reasoning. Coding = predicting plausible code. There's no separate "understanding" engine — the capabilities are emergent from being extremely good at prediction over vast training text.

**Hallucination, demystified forever:** the model optimizes for *plausible* continuations and has no ground-truth database to check against. A confident, fluent, false statement is just a high-probability token sequence that happens to be untrue. You don't fix this by scolding the model — you address it by changing what it's conditioned on (giving it real sources — that's RAG, Track 4) or checking its output (evaluation, Track 7).

The interview answer: *"Why do LLMs hallucinate?" → "Because a language model predicts plausible next tokens from learned patterns — it has no ground-truth lookup. A fluent false statement is just a high-probability sequence. So you reduce hallucination by conditioning it on real sources (RAG) or verifying outputs, not by expecting prediction to 'know' truth."*

The principle: **a language model is an autoregressive next-token predictor, and every capability AND every failure mode — reasoning, coding, hallucination, prompt-sensitivity — is a consequence of that one mechanism.** Internalize this and nothing in AI feels like magic again.

</details>

---

## Module 1.2 — Statelessness: Why the Model "Forgets"

**Difficulty:** ●●○○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Stateless**: the API server keeps NO memory between calls. Each call is independent; the server doesn't remember your last message unless you resend it.
- **Conversation history / `messages` list**: the running list of turns (user/assistant) you send with each call so the model "remembers" the conversation. The memory lives in what YOU send, not in the server.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **AI Engineering Fundamentals** → the sections on managing conversation/messages and how chat is built on a stateless API.
- **Practical Prompt Engineering** → any part showing multi-turn conversations (how history is passed).

**Read:**

- **Anthropic — Messages API (multi-turn)** — https://docs.claude.com/en/api/messages — *Focus on: that you send the whole `messages` list each call; the API doesn't store it for you.*
- **"How does ChatGPT remember the conversation"** — search `"how do llm chatbots remember conversation stateless"` — *Focus on: the app resends history; the model is stateless.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 1, Module 1.2 — statelessness. (I may have glimpsed this in
Track 0.10's Exercise 2; now we make it rock-solid.) Gentle rhythm, theory-aware.

PHASE 1 STUDY: Write a commented script that does a two-turn conversation the WRONG way (each call sends
only the new message), so the model "forgets" turn 1 on turn 2. Then show the RIGHT way (build up the
`messages` list including prior turns) so it remembers. Walk me through exactly why the first version
forgets — the API is stateless, so memory is whatever I resend.

PHASE 2 TINKER: Have me run both versions, watch the forgetting then the remembering, and grow a
3–4 turn conversation by appending to the messages list. Have me notice the list getting longer each
turn (foreshadowing cost/context).

PHASE 3 REPORT: I report. Make me explain TWO-LEVEL why the model forgets and how memory actually works
(it's engineered by resending history). Name the interview question and make me answer it.

PHASE 4 EXERCISE 1: Have me build a tiny working chat loop in the terminal that maintains history across
turns by appending to the messages list — hints free. (This is my first real "chatbot"!)

PHASE 5 EXERCISE 2: Have me build a slightly different multi-turn interaction solo (e.g. a Q&A that
references earlier answers), verify it remembers correctly, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The API is **stateless** — the server remembers nothing between calls. A chatbot "remembers" only because the app **resends the conversation history** (the `messages` list, with each prior user/assistant turn) on every call; the model re-reads it each time. Your wrong-version "forgot" turn 1 because you didn't include it; the right version remembers because you did.

This is the seed of an enormous idea across the AI tracks: **"memory" is engineered, not innate.** Chat history, agent memory, and RAG are all sophisticated versions of the same move — *deciding what text to put into the messages/context each call*. And since the history grows each turn, this connects to cost and context limits (next modules): longer conversations cost more and eventually must be trimmed or summarized.

The interview answer: *"How does an LLM chatbot remember the conversation?" → "It doesn't — the API is stateless. The application resends the full conversation history with each call, so the model re-reads it every time. 'Memory' is the app choosing what context to include, not the model retaining anything."*

The principle: **the LLM API is stateless, so conversation memory is engineered by resending history each call — which is why all 'memory' in AI systems (chat, agents, RAG) is really the art of deciding what to put into the context.** You also just built your first real chatbot loop.

</details>

---

## Module 1.3 — Tokens: The Unit of Everything

**Difficulty:** ●●○○○ · **Format:** STUDY → build · **Stack:** Python + tokenizer/SDK

### New words in this module

- **Token**: a subword chunk of text the model actually processes (not whole words, not single characters). Roughly ~4 English characters on average, but it varies.
- **Tokenization**: the step that splits text into tokens.
- **Tokenizer**: the tool/code that does the splitting. (A "library" for tokenizing, or an API endpoint that counts tokens.)
- **Endpoint** (recall from Track 0.9): a specific URL/operation of an API — here, the operation that counts tokens for you.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **AI Engineering Fundamentals** → the sections on tokens, context, and pricing (tokens as the unit of measurement and billing).

**Read:**

- **OpenAI Tokenizer (visual tool)** — https://platform.openai.com/tokenizer — *DO THIS: paste text and SEE it split into tokens. Makes the abstract concrete in 2 minutes.*
- **Anthropic — Token counting** — https://docs.claude.com/en/docs/build-with-claude/token-counting — *Focus on: counting tokens before sending.*
- **"What are tokens / byte-pair encoding, simply"** — search `"llm tokens byte pair encoding explained simply"` — *Focus on: why subword chunks (the efficiency tradeoff).*
- *(Surrounding, optional)* **"Why can't ChatGPT count letters in a word"** — search — *Focus on: a fun, memorable consequence of tokenization.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 1, Module 1.3 — tokens. THIS is the module whose earlier
version overwhelmed me ("write a script that counts tokens"), so go EXTRA gently and write the worked
example WITH me first. Decode "tokenizer" and "endpoint" before using them. Gentle rhythm.

PHASE 1 STUDY: First, point me to the visual tokenizer tool so I SEE text split into tokens before any
code. THEN write me a small, commented script that counts the tokens in a piece of text — explaining what
a tokenizer is and, if we use the Anthropic token-counting endpoint, reminding me what an "endpoint" is
(from Track 0.9). Walk through every line. Don't assume I know what any of it means.

PHASE 2 TINKER: Have me count tokens for several texts and notice patterns: how token count compares to
word/character count, how a made-up/rare word splits into multiple tokens, how another language tokenizes
differently. Also have me ask the model to count letters in a word and watch it struggle — then connect
it to tokenization. Predict-then-run.

PHASE 3 REPORT: I report the patterns I found. Make me explain TWO-LEVEL: (1) plain — what's a token?
(2) technical — why subword tokens (vocabulary-vs-length tradeoff), and why it explains the letter-counting
weakness. Name the interview question and make me answer it.

PHASE 4 EXERCISE 1: Have me write a small "token counter" for arbitrary text and print the count — hints
free and generous, since this is the exact thing that scared me before.

PHASE 5 EXERCISE 2: Have me build a tiny "cost estimator" solo: given text and a per-token price, compute
the input cost; given an expected reply length, estimate total — proving tokens are the billing unit.
Verify, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

Models process **tokens** — subword chunks, not words or characters. A token averages ~4 English characters but varies: common words are one token, rare/made-up words split into several, other languages can use more tokens per word. A **tokenizer** is the tool that splits text; an endpoint (or a library) can count tokens for you.

**Why subword tokens?** Character-level would make sequences too long/expensive; whole-word vocabularies would be huge and break on unseen words. Subword tokenization (byte-pair encoding and friends) is the efficient middle — frequent words are efficient, rare words combine pieces, any text is representable.

**What it explains:** the model "sees" tokens, not letters, so counting letters in "strawberry" is genuinely hard. Tokens are the unit of *everything* — context windows are measured in tokens, pricing is per-token (input and output priced separately), and because of statelessness you resend (and pay for) the whole history each turn, so cost grows with conversation length.

The interview answer: *"Why are LLMs bad at counting letters?" → "They see subword tokens, not letters — a word's letters are hidden inside tokens, so character-level operations are unnatural. Tokenization is a vocabulary-vs-sequence-length tradeoff via byte-pair encoding, and tokens are also the unit of context limits and billing."*

The principle: **the token is the atomic unit of LLMs — text is split into subword tokens for efficiency, and everything (context limits, pricing, certain weaknesses) is measured in and explained by tokens.** And notice: the thing that overwhelmed you before is now something you can do and explain. That's the ramp working.

</details>

---

## Module 1.4 — The Context Window: The Model's Working Memory

**Difficulty:** ●●●○○ · **Format:** STUDY → investigation · **Stack:** Python + anthropic SDK

### New words in this module

- **Context window**: the maximum number of tokens the model can consider in one call — everything you send (instructions + history + documents) plus what it generates must fit inside it. The model's entire working memory for that single call.
- **Lost in the middle**: a known effect where models pay most attention to the *start* and *end* of a long context and can miss information buried in the *middle*.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **AI Engineering Fundamentals** → the context-window / context-management sections.
- **The Hard Parts of AI: Neural Networks** → (optional, theory) any section touching "attention" — the mechanism behind why context costs what it does. Dip in for intuition; skip heavy math.

**Read:**

- **Anthropic — Models & context windows** — https://docs.claude.com/en/docs/about-claude/models — *Focus on: the window size, and that input + output share it.*
- **"Lost in the middle" (research, explained)** — search `"lost in the middle long context llm explained"` — *Focus on: position within the context affects retrieval.*
- **"Why do context windows have limits"** — search `"why context window limited attention quadratic"` — *Focus on: the limit comes from attention's cost growing with length (you don't need the math — just the why).*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 1, Module 1.4 — the context window. Gentle rhythm, and feed my
theory appetite on WHY the limit exists (attention cost) once the basics land.

PHASE 1 STUDY: Explain the context window plainly (the model's working memory for one call; input +
output must fit). Write a small commented script that builds a long input and shows what happens as it
grows toward the limit. Then explain, at the depth I enjoy, WHY a limit exists at all — attention compares
every token to every other, so cost grows ~quadratically with length. Keep the theory intuitive, no heavy
math.

PHASE 2 TINKER: Have me run a "lost in the middle" mini-experiment: put a specific fact at the start,
middle, and end of a long context and test whether the model retrieves it equally well from each spot.
Predict-then-run; let me see the effect myself.

PHASE 3 REPORT: I report. Make me explain TWO-LEVEL: (1) plain — what's a context window? (2) technical —
why the limit exists, what "lost in the middle" is, and the practical implication for WHERE I put important
content. Name the interview question and make me answer it.

PHASE 4 EXERCISE 1: Have me write something that estimates whether a given set of messages will fit in a
model's window (sum the tokens, compare to the limit) — hints free.

PHASE 5 EXERCISE 2: Have me build a tiny "context manager" solo: when a conversation would exceed the
window, decide what to keep/drop/summarize so it still fits (e.g. keep system + recent turns, summarize
older). This is the core engineering problem of the whole field in miniature. Verify, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The **context window** is the maximum tokens the model handles in one call — system instructions + conversation history + any documents + the tokens it generates all share one budget. It's the model's working memory for that single call; nothing outside it exists to the model.

**Why the limit exists (the mechanism you'll enjoy):** transformers use *attention*, which compares every token with every other token, so compute/memory cost grows roughly with the **square** of the length. Doubling context roughly quadruples cost — hence finite windows even as they grow.

**"Lost in the middle":** even within the window, models attend most reliably to the start and end; info buried in the middle can be effectively missed. Practical upshot: put critical instructions and the most relevant retrieved documents near the beginning or end (this shapes RAG design in Track 4).

**Why this is THE central problem:** stateless API (1.2) + everything-is-tokens (1.3) + finite window means applied AI is largely *deciding what goes into the context* — include, drop, summarize, retrieve. Your Exercise 2 keep/drop/summarize manager is a tiny version of what every chat app, agent, and RAG system does.

The interview answer: *"Why is there a context window limit, and why does position within it matter?" → "Attention compares every token to every other, so cost scales ~quadratically with length — hence a finite window. Within it, models attend most reliably to the start and end ('lost in the middle'), so I place critical instructions and retrieved context where attention is strongest."*

The principle: **the context window is the model's finite working memory (limited because attention scales quadratically), models attend unevenly within it, and managing what goes into it is the central engineering discipline of applied AI.**

</details>

---

## Module 1.5 — Temperature & Sampling: Where the Randomness Comes From

**Difficulty:** ●●●○○ · **Format:** STUDY → investigation · **Stack:** Python + anthropic SDK

### New words in this module

- **Probability distribution**: for the next token, the model produces a list of all possible tokens with a probability for each (e.g. "the" 12%, "a" 8%, …). It doesn't output one token directly — it outputs these odds.
- **Sampling**: the step that *picks* one token from that distribution. This is where randomness enters.
- **Temperature**: a knob (often 0 to 1+) that reshapes the distribution before sampling. Low = focused/repeatable; high = varied/creative. Temperature 0 ≈ always pick the most likely token.
- **top-p / top-k**: alternative knobs that limit which tokens are even eligible before sampling.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **AI Engineering Fundamentals** → the sections on model parameters (temperature) and controlling output.
- **Practical Prompt Engineering** → any part on temperature / determinism for reliable outputs.

**Read:**

- **Anthropic — Messages API parameters (temperature)** — https://docs.claude.com/en/api/messages — *Focus on: what `temperature` does.*
- **"LLM temperature and sampling explained"** — search `"llm temperature top-p sampling explained beginner"` — *Focus on: temperature reshapes the distribution; sampling picks from it.*
- **"Why are LLMs non-deterministic"** — search `"why are llms non deterministic same prompt different answer"` — *Focus on: sampling is the source of run-to-run variation.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 1, Module 1.5 — temperature & sampling, the source of LLM
randomness. This connects to my frontend instinct breaking (same input ≠ same output). Gentle rhythm,
theory once basics land.

PHASE 1 STUDY: Explain plainly: the model outputs a PROBABILITY DISTRIBUTION over the next token, and
SAMPLING picks one — that's where randomness comes from. TEMPERATURE reshapes the distribution (low =
sharpen toward top tokens, near-repeatable; high = flatten, more varied). Write a commented script that
sends the SAME prompt several times at temperature 0 and at a high temperature so I SEE near-identical vs
varied outputs.

PHASE 2 TINKER: Have me run it, observe the difference, then pick a task where I'd want LOW temperature
(e.g. extracting a date, classifying) and one where I'd want HIGH (brainstorming names), and justify each.
Predict-then-run.

PHASE 3 REPORT: I report. Make me explain TWO-LEVEL: (1) plain — what does temperature do? (2) technical —
where randomness enters (sampling from the distribution), why temp 0 is ~deterministic, what top-p/top-k
are. Name the interview question and make me answer it.

PHASE 4 EXERCISE 1: Have me write a script that demonstrates temperature's effect on a reliability-sensitive
task (e.g. "always return one of three labels") — showing low temp is more consistent — hints free.

PHASE 5 EXERCISE 2: A different temperature experiment solo (e.g. creative vs precise task), reasoning about
which temperature fits and why. Verify, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

Recall from 1.1: the model outputs a **probability distribution** over the next token — it's deterministic up to that distribution (same input → same odds). The randomness you experience enters at **sampling**: picking one token from the distribution.

**Temperature** reshapes the distribution before sampling. Low temperature **sharpens** it toward the highest-probability tokens (focused, repeatable); high temperature **flattens** it (gives lower-probability tokens a real chance — diverse, "creative"). **Temperature 0** ≈ greedy decoding (always the single most likely token), as deterministic as it gets. **top-p** and **top-k** are companion knobs that bound *which* tokens are eligible before temperature acts.

**The practical theory:** this answers "why do I get different answers to the same prompt?" — sampling. And it tells you how to choose deliberately: **low temperature for extraction, classification, structured output, and code** (consistency, parseability); **higher temperature for brainstorming and creative variety**. This directly serves the reliability you'll engineer in Track 2.

The interview answer: *"Why are LLMs non-deterministic, and how do I control it?" → "The model produces a fixed probability distribution over next tokens; non-determinism comes from sampling one from it. Temperature reshapes that distribution — low sharpens toward top tokens (temp 0 ~greedy/repeatable), high flattens for diversity; top-p/top-k bound the candidate set. So I use low temperature for consistency/structured tasks, higher for creativity."*

The principle: **LLM randomness comes from sampling a token out of the model's probability distribution, and temperature (with top-p/top-k) controls how random that pick is — so you choose it deliberately per task.**

</details>

---

## Module 1.6 — Cost, Latency, and Choosing a Model

**Difficulty:** ●●○○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Input tokens / output tokens**: you're billed separately for the tokens you send (input) and the tokens the model generates (output); output is usually pricier.
- **Latency**: how long a response takes. Bigger models are usually smarter but slower and costlier; smaller models are faster and cheaper.
- **Model tiers**: providers offer a range (e.g. small/fast vs large/smart). Choosing the right one per task is a real engineering decision.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **AI Engineering Fundamentals** → the sections on pricing, model selection, and cost/latency tradeoffs.
- **AI for Software Engineers** → any part on choosing models and managing cost in real apps.

**Read:**

- **Anthropic — Pricing & Models** — https://docs.claude.com/en/docs/about-claude/pricing (and the models page) — *Focus on: per-million-token input vs output pricing, and the model lineup.*
- **"LLM cost optimization basics"** — search `"llm api cost optimization input output tokens model choice"` — *Focus on: pick the smallest model that does the job; cost = tokens × price.*
- **"Latency vs quality LLM tradeoff"** — search — *Focus on: bigger ≠ always better for the job.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 1, Module 1.6 — cost, latency, model choice. Practical and
career-relevant (I want to build real things affordably). Gentle rhythm.

PHASE 1 STUDY: Explain input vs output token pricing, latency, and model tiers (small/fast/cheap vs
large/smart/slow/costly). Write a commented script that runs the same task on a small and a larger model
and reports, from the response usage data, the token counts, an estimated cost, and roughly the latency.
Explain how to read the usage info.

PHASE 2 TINKER: Have me run a few tasks across models and observe the quality/cost/latency tradeoffs — some
tasks a small model handles fine (cheap win), some need a bigger one. Predict which is which first.

PHASE 3 REPORT: I report. Make me explain TWO-LEVEL: how cost is computed (tokens × price, input/output
separate) and how I'd CHOOSE a model for a given task. Name the interview question and make me answer it.

PHASE 4 EXERCISE 1: Have me build a small "model picker" helper that, given a task description, reasons
about which tier to use and estimates cost — hints free.

PHASE 5 EXERCISE 2: Have me solo design a tiny experiment proving a small model is sufficient (and far
cheaper) for some task while a big one is needed for another, with cost numbers. Verify, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

You're billed per **token**, with **input** and **output** priced separately (output usually costs more), so cost = tokens × price for each. **Latency** (response time) and **quality** trade off against cost: bigger models are generally smarter but slower and pricier; smaller models are faster and cheaper and often *good enough*. Choosing the right **model tier per task** is a real engineering decision — using a flagship model for a task a small one handles is wasted money.

This is directly career-relevant: an engineer who can build something that works *and* costs little is more valuable than one who reaches for the biggest model every time. It connects to the model advice in your START-HERE (Sonnet vs Opus) — same logic, applied to your own tutoring.

The interview answer: *"How do you manage LLM cost?" → "Cost is tokens × price, with input and output billed separately, so I minimize tokens (concise prompts, trimmed history) and use the smallest model that meets the quality bar — reserving large models for tasks that genuinely need them. I measure cost and latency per task rather than defaulting to the biggest model."*

The principle: **LLM cost is per-token (input and output priced separately) and trades off against latency and quality — so the engineering skill is choosing the smallest model and fewest tokens that meet the bar, measured per task.**

</details>

---

## Module 1.7 — Synthesis: A Mental Model of LLMs You Can Defend in Any Interview

**Difficulty:** ●●●○○ · **Format:** Synthesis · **Stack:** Python + anthropic SDK

### New words in this module

- *(None new — this integrates the whole track.)*

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **AI Engineering Fundamentals** → revisit the "how LLMs work" overview now that each piece makes sense — it'll click at a new level.

**Read:**

- Revisit your own `NOTES.md` from 1.1–1.6.
- **Anthropic — Prompt engineering overview** (preview of Track 2) — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview — *Skim, to see where you're headed.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 1, Module 1.7 — synthesis into one interview-ready mental model.
I can do more myself now; calibrate (Zone of Proximal Development) — less hand-holding if I'm flowing.

PHASE 1 STUDY: Help me assemble the whole pipeline as one picture: text → tokens → autoregressive next-token
prediction within a finite context window → probability distribution → sampling (temperature) → token →
repeat → text out, statelessly, billed per token. For EACH step, have me state the real-world consequence
it causes (engineered memory, hallucination, cost, lost-in-the-middle, non-determinism, model choice).

PHASE 2 TINKER / WRITE: Have me write a single-page "How an LLM Works" explainer in my OWN words (save as
NOTES.md) that I could use to ace this part of any interview. Review it with me and tighten it.

PHASE 3 REPORT: I present my explainer; you push on any vague spot until it's crisp and two-level.

PHASE 4 EXERCISE 1 (mock interview): Act as an interviewer and ask me 6–8 conceptual questions across the
whole track in random order, escalating, demanding two-level answers. Hints if I stall.

PHASE 5 EXERCISE 2 (harder mock): A tougher rapid-fire round solo, then the big verdict: do I have an
interview-grade foundational understanding of LLMs (proceed to Track 2 — Prompting), or which module to
revisit? Be honest; name my weakest spot.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

No new mechanism — this welds Track 1 into one defensible model:

**The pipeline:** Your text is **tokenized** (1.3) into subword units. Those tokens, plus the whole conversation (since the API is **stateless** — 1.2), are fed within the finite **context window** (1.4) to the **autoregressive** model (1.1), which predicts a **probability distribution** over the next token. **Sampling**, controlled by **temperature** (1.5), picks one. It's appended, and the loop repeats. Out comes text. You're billed per token, input and output (1.6).

**Each piece explains a reality:** statelessness → memory is engineered; next-token prediction → hallucination is intrinsic; tokens → everything is counted/limited/billed in tokens; context window → finite working memory, position matters; sampling/temperature → non-determinism by design; cost → choose the right model.

**Why this is the foundation:** every later track applies this model. Prompting (Track 2) = shaping the conditioning tokens. RAG (Track 4) = putting real source tokens in context to fight hallucination. Agents (Track 5) = managing context across many calls. Evals (Track 7) = checking sampled outputs. You're not memorizing tricks — you're applying one mechanism.

The principle: **an LLM is a stateless, autoregressive, token-based, finite-context, sampled next-token predictor — and that single integrated model explains every behaviour and grounds every technique in the rest of applied AI.** If you can explain this cold, two-level, you already understand LLMs better than most working engineers — exactly the interview edge you wanted.

</details>

---

## 🧊 COLD REBUILD (do this before starting the next track — the honest gate)

**The task:** From an empty file, write and run a script that (a) demonstrates statelessness (the model "forgetting" without conversation history, then remembering with it), (b) counts tokens for a few inputs, and (c) shows temperature changing output variability across repeated calls — narrating, as you go, the five-word model of what an LLM is.

**The rules (per AI-CONTRACT.md):** empty folder, ~20 minutes, NO notes, NO old code open. Paste this into Claude Code:

```text
Follow CLAUDE.md and AI-CONTRACT.md — this is a 🧊 COLD REBUILD. I'm cold-rebuilding Track 1's core: a script demonstrating statelessness, token counting, and temperature effects, narrating the mechanism as I go. Time me (~20 min, soft limit).
Full Prime Directive: no worked examples, hint ladder only. When I finish (or the time is clearly gone),
grade the rebuild honestly — what came out fluently, what I fumbled, what I couldn't produce at all — log
it as evidence in LEARNING-RECORD, and give the gate verdict: proceed to the next track, or which modules
to re-run first. Recognizing is not retrieval; only what I PRODUCED counts.
```

*Why this exists: you can pattern-match through a capstone with your own notes open. You cannot fake a cold rebuild. This 20 minutes is the most honest signal in the whole track.*

---

*End of Track 1 — Foundations: How LLMs Actually Work. 7 modules. You now understand what an LLM fundamentally is — stateless, autoregressive, token-based, finite-context, sampled next-token prediction — and you can explain it, two-level, well enough to impress any interviewer. The thing that overwhelmed you (counting tokens) is now something you can do and explain. Nothing in AI feels like magic anymore.*

*Next: Track 2 — Prompting as Engineering, where your daily Claude use becomes a deliberate, reliable, professional skill — and where you learn to make a stochastic model behave.*
