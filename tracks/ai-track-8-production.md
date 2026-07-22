# TRACK 8 — PRODUCTION: COST, CACHING, STREAMING & OBSERVABILITY

*The track that makes your AI systems real — fast, cheap, and operable. A system that works in a notebook but costs too much, feels slow, or can't be debugged in production isn't shippable. You'll learn to control cost (the thing that kills AI projects), make responses feel instant with streaming (where your frontend skills shine), cache aggressively, and add the observability that lets you actually run a system in production. This is the "prototype to production" gap, and crossing it is what makes you someone who ships, not just prototypes.*

*Prerequisites: Tracks 0–7. Python + the anthropic SDK; some of this connects to frontend (streaming UIs).*

*Spine courses (you own them): **AI Engineering Fundamentals** (cost, latency, production concerns) and **AI Agent: From Prototype to Production** (the production mindset). Your frontend knowledge directly applies to the streaming module.*

*Gentle rhythm: STUDY → TINKER → REPORT → EXERCISE 1 → EXERCISE 2.*

*🧊 Cold opens + track-end cold rebuild. 📏 Evals thread: production adds cost/latency budgets to your eval runs — a regression is now "quality dropped OR cost doubled." 🐛 Bug hunt in Module 8.5 (Level 3): a system that's mysteriously slow and expensive, found only through the observability you build. 🖼️ Capstone UI rep: true token streaming into a UI — the payoff of Module 8.3 and the most "frontend edge" moment in the primer.*

---

## Module 8.1 — Cost Management: The Thing That Kills AI Projects

**Difficulty:** ●●●○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Cost attribution**: knowing where your spend goes — which features, users, or calls cost the most.
- **Token optimization**: reducing tokens (shorter prompts, trimmed history, smaller context) to cut cost without hurting quality.
- **Model routing**: using a cheap/small model for easy requests and an expensive/large one only when needed.

### Prep — Watch & Read

**Watch (Frontend Masters — you own these):**
- **AI Engineering Fundamentals** → cost / optimization sections. Main reference.
- **AI for Software Engineers** → cost-aware AI architecture.

**Read (gentle → deeper):**
- **Anthropic — Pricing & reducing costs** — https://docs.claude.com/en/docs/about-claude/pricing — *Focus on: input/output pricing, how to reduce spend.*
- **"LLM cost optimization strategies"** — search `"llm cost optimization token reduction model routing caching"` — *Focus on: the main levers.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 8, Module 8.1 — cost management. Cost kills AI projects; I want
to build affordably (a real goal of mine). Gentle rhythm; decode cost attribution, token optimization,
model routing.

PHASE 1 STUDY: Show me how to measure and reduce cost: track per-call token usage and cost from the response
data (attribution), then apply levers — shorten prompts, trim conversation history, use smaller models for
easy requests (model routing), and cache stable context (preview of 8.2). Take one of my systems and cut
its cost measurably without hurting quality.

PHASE 2 TINKER: Have me measure a system's cost, then apply each lever and measure the savings; route easy
vs hard requests to small vs large models and compare cost/quality; find where most of the spend goes.
Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the cost levers (tokens × price, input/output separate — Track
1.6) and how to cut cost without hurting quality. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me reduce a real system's cost via attribution + the levers, proving the savings —
hints free.

PHASE 5 EXERCISE 2: Have me cost-optimize a different system solo (measure, attribute, reduce), then the
verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

Cost is the thing that kills AI projects — a system that's too expensive per use can't ship. **Cost attribution** is knowing where spend goes (which features/users/calls cost most), so you optimize what matters. The levers: **token optimization** (shorter prompts, trimmed history, smaller context — since cost is tokens × price, Track 1.6), **model routing** (cheap/small model for easy requests, expensive/large only when needed — Track 1.6), and **caching** (8.2, reuse stable context). The discipline is cutting cost *without hurting quality* — measure, attribute, optimize the biggest line items, re-measure.

**Why it's career-relevant:** this is directly your stated goal of building affordably, and it's a senior signal — an engineer who ships something that works *and* costs little is worth more than one who burns money on the biggest model for everything. "I cut our LLM cost 60% by routing easy requests to a smaller model and caching the system prompt, with no quality drop" is exactly the kind of impact that gets noticed.

The interview answer: *"How do you manage LLM cost in production?" → "Attribute cost first — measure per-call tokens and find where spend concentrates. Then optimize: trim prompts and history, route easy requests to smaller models and reserve large ones for hard cases, and cache stable context. Cost is tokens × price with input and output separate, so I minimize tokens and right-size the model per request — cutting cost without hurting quality, measured before and after."*

The principle: **AI cost is tokens × price, so you attribute spend, then cut it with token optimization, model routing, and caching — reducing cost without hurting quality, which is both what keeps AI projects viable and a clear senior-engineering signal.**
</details>

---

## Module 8.2 — Caching: Prompt Caching & Semantic Caching

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK + vector DB

### New words in this module

- **Prompt caching** (from 2.9, deeper here): reusing the processing of a stable prompt prefix across calls — saves cost and latency.
- **Semantic caching**: caching *answers* by meaning — if a new query is semantically very similar to one you already answered, return the cached answer instead of calling the model again. (Uses embeddings — Track 3!)
- **Cache invalidation**: knowing when a cached result is stale and must be refreshed.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → caching / efficiency sections.

**Read:**
- **Anthropic — Prompt caching** — https://docs.claude.com/en/docs/build-with-claude/prompt-caching — *Focus on: caching stable prefixes.*
- **"Semantic caching for LLMs"** — search `"semantic caching llm embeddings similar queries"` — *Focus on: cache by meaning using embeddings.*
- **"Cache invalidation strategies"** — search — *Focus on: the classic hard problem, applied to AI.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 8, Module 8.2 — caching (prompt + semantic). Big cost/latency
lever, and semantic caching cleverly reuses my embeddings skills. Gentle rhythm; decode semantic caching,
cache invalidation.

PHASE 1 STUDY: Show me (a) prompt caching (deeper than 2.9) for repeated stable context; (b) SEMANTIC
caching — embed incoming queries, and if a new query is very similar to a past one (cosine similarity above
a threshold — Track 3!), return the cached answer instead of calling the model, saving cost + latency.
Build a simple semantic cache. Discuss cache invalidation (when cached answers go stale).

PHASE 2 TINKER: Have me send similar-but-not-identical queries and watch the semantic cache hit; tune the
similarity threshold (too loose = wrong cached answers; too tight = few hits); find a case where a stale
cache would give a wrong answer (invalidation). Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: prompt vs semantic caching, how semantic caching uses
embeddings, the threshold tradeoff, and why invalidation matters. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me add semantic caching to a system and measure the cost/latency savings — hints free.

PHASE 5 EXERCISE 2: Have me add caching to a different system solo, tune the threshold, handle staleness,
then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

Two caching strategies. **Prompt caching** (deeper than 2.9) reuses the processing of a stable prompt prefix across calls — cost and latency savings for repeated large context. **Semantic caching** is cleverer and reuses your Track 3 skills: embed incoming queries, and if a new query is semantically very similar (cosine similarity above a threshold) to one you already answered, return the **cached answer** instead of calling the model — saving the entire call. **Cache invalidation** is knowing when a cached answer is stale (the underlying data changed) and must be refreshed.

**The elegant connection:** semantic caching is semantic search (Track 3) applied to your own past Q&A — embed the query, find the most similar previously-answered query, and if it's close enough, reuse that answer. The **threshold tradeoff** is key: too loose and you return a cached answer to a query that's actually different (wrong answer); too tight and you rarely get cache hits (few savings). And invalidation is the classic hard caching problem — a cached answer about "current status" goes stale when the status changes.

The interview answer: *"What caching strategies exist for LLM apps?" → "Prompt caching reuses processing of a stable prompt prefix across calls. Semantic caching embeds queries and returns a cached answer when a new query is similar enough to a past one — semantic search over your own Q&A — saving the whole call. The tradeoffs are the similarity threshold (too loose returns wrong cached answers, too tight gives few hits) and invalidation (cached answers go stale when underlying data changes)."*

The principle: **prompt caching reuses stable-prefix processing while semantic caching reuses whole answers for similar queries (semantic search over past Q&A) — major cost/latency levers whose key challenges are the similarity threshold and cache invalidation when data goes stale.**
</details>

---

## Module 8.3 — Streaming: Making Responses Feel Instant (Your Frontend Edge)

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK (+ frontend concepts)

### New words in this module

- **Streaming**: receiving the model's response token-by-token as it's generated, instead of waiting for the whole thing — so the user sees text appear immediately (like ChatGPT's typing effect).
- **Time to first token (TTFT)**: how long until the first bit of response appears. Streaming makes this feel fast even if the full response takes a while.
- **Server-Sent Events (SSE)**: the common web mechanism for streaming tokens to a browser. (Your frontend knowledge applies directly.)

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → streaming sections.
- **AI for Software Engineers** → integrating streaming into apps.

**Read:**
- **Anthropic — Streaming messages** — https://docs.claude.com/en/docs/build-with-claude/streaming — *Focus on: streaming the response in Python.*
- **"Why streaming improves perceived performance"** — search `"llm streaming time to first token perceived latency"` — *Focus on: feel-fast even when total time is similar.*
- **"Streaming LLM responses to a frontend SSE"** — search — *Focus on: token stream → UI (your edge).*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 8, Module 8.3 — streaming. This is where my FRONTEND skills are
an edge. Gentle rhythm; decode streaming, TTFT, SSE.

PHASE 1 STUDY: Show me how to STREAM a response in Python — receive and print tokens as they arrive instead
of waiting for the whole answer. Explain time-to-first-token and why streaming makes a response FEEL fast
(perceived performance — which I know well from frontend) even when total time is similar. Explain how this
reaches a frontend (SSE), connecting to my web skills.

PHASE 2 TINKER: Have me compare a non-streamed call (wait, then dump) vs streamed (tokens appear live) and
FEEL the difference; measure time-to-first-token vs total time; think about how I'd render this in a React
UI. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what streaming is, why it improves PERCEIVED performance (TTFT,
the typing effect), and how it connects to my frontend knowledge. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a streaming chat experience in Python (and sketch how it'd wire to a React
frontend) — hints free.

PHASE 5 EXERCISE 2: Have me build a different streaming interaction solo, then the verdict. Note: combining
streaming with a great frontend is literally my competitive edge.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Streaming** receives the model's response token-by-token as it's generated, instead of waiting for the whole thing — so the user sees text appear immediately (the ChatGPT typing effect). **Time to first token (TTFT)** is how long until the first bit appears, and streaming makes TTFT short even if the full response takes a while. **Server-Sent Events (SSE)** is the common web mechanism for streaming tokens to a browser.

**Why this is YOUR edge:** streaming is fundamentally a *perceived-performance* technique — the exact thing you know deeply from frontend (and from your performance primer!). The total generation time may be the same, but streaming makes it *feel* dramatically faster because the user gets immediate feedback instead of staring at a spinner. You already understand perceived performance, loading states, and rendering token streams in a UI better than most AI engineers — so "AI that feels fast and is beautifully built" is a genuine competitive advantage. Most AI builders treat the frontend as an afterthought; you can make it a strength.

The interview answer: *"What is streaming and why does it matter?" → "Streaming sends the response token-by-token as it's generated, so the user sees output immediately rather than waiting for the full response. It improves perceived performance — short time-to-first-token makes it feel fast even if total time is similar — and it's delivered to the browser via SSE. It's a UX technique as much as a technical one, which is where strong frontend skills make the AI product feel premium."*

The principle: **streaming delivers the response token-by-token for a short time-to-first-token, making it FEEL instant (perceived performance) even when total time is unchanged — and because it's fundamentally a frontend/UX technique, it's exactly where your frontend expertise becomes a competitive edge in building AI products.**
</details>

---

## Module 8.4 — Latency Optimization

**Difficulty:** ●●●○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Latency**: total time for a response. Distinct from *perceived* latency (which streaming helps) — this is actual speed.
- **Parallelization**: running independent LLM calls concurrently instead of sequentially, to cut total time.
- **Model/prompt latency tradeoffs**: smaller models and shorter outputs are faster; you trade some quality for speed where it's worth it.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → latency/performance sections.
- **AI Agent: From Prototype to Production** → making real systems fast.

**Read:**
- **"LLM latency optimization"** — search `"llm latency optimization parallel smaller model output length"` — *Focus on: the real-speed levers.*
- **Anthropic — Reducing latency** — https://docs.claude.com/en/docs/build-with-claude/latency-optimization — *Focus on: practical latency reductions.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 8, Module 8.4 — latency optimization (actual speed, vs perceived
from streaming). Gentle rhythm; decode latency, parallelization.

PHASE 1 STUDY: Show me the real-latency levers: parallelize independent calls (run concurrently, not
sequentially — connect to async, Track 0.13), use smaller/faster models where quality allows, reduce output
length, and cache (8.2). Take a slow multi-call workflow and speed it up measurably.

PHASE 2 TINKER: Have me convert sequential independent calls to parallel and measure the speedup; swap a big
model for a smaller one on a suitable task and measure; cap output length and measure. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the latency levers and the difference between actual latency
(this module) and perceived latency (streaming, 8.3). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me optimize a real slow workflow's latency (parallelize + right-size) and prove the
speedup — hints free.

PHASE 5 EXERCISE 2: Have me optimize a different system's latency solo, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Latency** is the actual time for a response — distinct from *perceived* latency (which streaming, 8.3, addresses). The real-speed levers: **parallelization** (run independent LLM calls concurrently instead of sequentially — connect to async, Track 0.13 — so a workflow with 3 independent calls takes ~1×, not 3×), **right-sizing the model** (smaller/faster models where quality allows), **reducing output length** (fewer output tokens = faster, since generation is sequential), and **caching** (8.2).

**The key distinction:** actual latency (this module — make it genuinely faster) vs perceived latency (8.3 — make it *feel* faster with streaming). Both matter, and they're complementary: stream so it feels instant, AND optimize actual latency so it really is fast. An interviewer who hears you distinguish these — and reach for parallelization of independent calls — sees real production understanding.

The interview answer: *"How do you reduce LLM latency?" → "Distinguish actual from perceived latency. For perceived, stream the response. For actual, parallelize independent calls instead of running them sequentially, right-size the model to the task, reduce output length since generation is sequential, and cache. The biggest wins are usually parallelizing independent calls and not using an oversized model — and streaming on top makes it feel fast regardless."*

The principle: **actual latency is reduced by parallelizing independent calls, right-sizing models, limiting output length, and caching — distinct from perceived latency (streaming) — and combining genuine speed with streaming's feel-fast is how production AI becomes responsive.**
</details>

---

## Module 8.5 — Observability: Logging, Tracing & Monitoring AI Systems

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Observability**: being able to see what your AI system is doing in production — inputs, outputs, tool calls, costs, latencies, errors — so you can debug and improve it.
- **Tracing**: following a single request through all its steps (especially for agents/pipelines with many calls).
- **Monitoring / alerting**: tracking key metrics over time (cost, error rate, latency, quality) and getting alerted when something goes wrong.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agent: From Prototype to Production** → observability / monitoring sections. Key course here.
- **AI Engineering Fundamentals** → production operations.

**Read:**
- **"LLM observability explained"** — search `"llm observability tracing logging monitoring production"` — *Focus on: what to log and why.*
- **"Tracing LLM agent pipelines"** — search `"tracing llm agent multi-step debugging"` — *Focus on: following a request through steps.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 8, Module 8.5 — observability. You can't run in production what
you can't see. Gentle rhythm; decode observability, tracing, monitoring.

PHASE 1 STUDY: Show me how to make an AI system observable: log each call's inputs, outputs, tokens, cost,
latency, and errors; trace a multi-step request (agent/pipeline) through all its steps; and track key
metrics over time (cost, error rate, latency, a quality proxy). Build basic observability around one of my
systems and use it to debug a problem.

PHASE 2 TINKER: Have me introduce a problem (a slow step, a failing tool, a cost spike) and use the logs/
traces to FIND it; watch a metric move when I change something. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what observability is, what to log/trace/monitor, and why it's
essential for running AI in production (you can't debug or improve what you can't see). Name the interview
question and answer.

PHASE 4 EXERCISE 1: Have me add observability to a real system and use it to diagnose an issue — hints free.

PHASE 5 EXERCISE 2 — 🐛 BUG HUNT, LEVEL 3: Hand me a working system that is mysteriously SLOW and
EXPENSIVE, and say nothing more — no counts, no locations. The only way in is to build observability
first (this module's skill) and let the traces tell me where the cost and latency actually go. Plant
sincere production leaks (e.g. a cache that never hits because of a shifting prompt prefix, redundant
calls in a loop, an oversized context assembled every request, a serial pipeline that should be
parallel). I characterize with numbers, hypothesize, fix, and PROVE each fix with before/after cost and
latency measurements in my eval run (📏 — budgets are part of evals now). Full Prime Directive. Then the
verdict. Finding money leaks with traces is a genuinely senior production skill.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Observability** is being able to see what your AI system does in production: log each call's inputs, outputs, tokens, cost, latency, and errors; **trace** a single request through all its steps (essential for agents/pipelines with many calls — connect to agent observability, 6.8); and **monitor** key metrics over time (cost, error rate, latency, a quality proxy) with **alerting** when something breaks. You can't debug, improve, or trust a system you can't see.

**Why it's the production capstone skill:** in development you eyeball one run; in production, thousands of requests run unattended, costs accumulate, and failures happen silently. Observability is what lets you answer "why did this request fail?", "why did cost spike?", "is quality degrading?" — and it's what makes a system *operable*. This is core to the prototype-to-production gap: a prototype has no observability; a production system is built to be seen into.

The interview answer: *"How do you operate an AI system in production?" → "Observability: log inputs, outputs, tokens, cost, latency, and errors per call; trace multi-step requests through every step; and monitor metrics over time — cost, error rate, latency, quality — with alerting. You can't debug or improve what you can't see, so production systems are instrumented from the start. It's what lets you answer why a request failed, why cost spiked, or whether quality is drifting."*

The principle: **observability — logging, tracing multi-step requests, and monitoring cost/error/latency/quality metrics with alerting — is what makes an AI system operable in production, because you can't debug, improve, or trust what you can't see; it's central to crossing the prototype-to-production gap.**
</details>

---

## Module 8.6 — Production Capstone: Ship-Ready AI

**Difficulty:** ●●●●● · **Format:** Guided capstone · **Stack:** Python + anthropic SDK

### New words in this module

- *(None new — integrates the track.)*

### Prep — Watch & Read

**Watch (Frontend Masters):**
- Revisit **AI Agent: From Prototype to Production** as the checklist — this capstone IS that transition.

**Read:**
- Revisit your `NOTES.md` from 8.1–8.5.

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 8, Module 8.6 — production capstone. Completed 8.1–8.5. Take a
real system from prototype to ship-ready. Calibrate toward independence (ZPD).

PHASE 1 STUDY (light): Map the production checklist: cost-optimized + cached (prompt/semantic) + streaming
for UX + latency-optimized + observable (logging/tracing/monitoring) — on top of Track 7's evals + safety.

PHASE 2 BUILD: Guide me to take a real system (my RAG app or agent) and make it genuinely ship-ready: reduce
and attribute cost, add caching, add streaming for a good UX, optimize actual latency, and add observability.
Combined with Track 7's evals + guardrails, this is a deployable system. Let me drive; hint when stuck.

PHASE 3 REPORT: I report. Make me present the system as production-ready: its measured cost, latency, the
streaming UX, and the observability I'd use to operate it — as if proposing to deploy it.

PHASE 4 EXERCISE 1: Have me improve the system's weakest production aspect (cost, latency, or observability)
and prove the gain — hints available.

PHASE 5 EXERCISE 2: Have me make a DIFFERENT system ship-ready solo — with eval numbers AND cost/latency
budgets reported (📏) — then the big verdict: am I ready for Track 9 (MCP & Tooling), or which production
concept to revisit? A system that's evaluated, safe, cheap, fast, and observable is genuinely deployable.

PHASE 6 — 🖼️ UI REP (expert mode — the streaming payoff): upgrade one of my existing capstone UIs (Track
4 or 6) to TRUE TOKEN STREAMING: the model's answer flowing into the page token by token, with correct
loading, partial-render, cancel, and error states. This is Module 8.3 made real end-to-end (Python
streaming out → API route → streamed to the browser → rendered progressively), and it is the single most
"frontend edge" moment in the primer — the difference between an AI app that feels dead and one that
feels alive is exactly this, and I'm one of the few builders equipped to do it beautifully. Full expert
rules; seam gentleness only for the server-sent-events/streaming-format plumbing between Python and Next.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The capstone integrates the track into a genuinely **ship-ready** system: cost-optimized and attributed (8.1), cached (8.2), streaming for great UX (8.3), latency-optimized (8.4), and observable (8.5) — on top of Track 7's evaluation and safety. That combination — evaluated, safe, cheap, fast, observable — is what "production-ready" actually means, and it's the full prototype-to-production transition your FM course is named for.

This is the difference between a notebook demo and something you can actually deploy and operate. Most self-taught builders stop at "it works in my notebook"; being able to take it all the way to ship-ready — and *explain* the cost, latency, and observability decisions — is a strong senior signal and makes your portfolio projects genuinely impressive (a deployed, operable AI app, not a screenshot).

The interview answer (whole track): *"What does it take to put an AI system in production?" → "Beyond it working: control and attribute cost, cache (prompt and semantic), stream for perceived performance, optimize actual latency by parallelizing and right-sizing, and instrument observability — logging, tracing, monitoring. Combined with evaluation and guardrails, that's a deployable, operable system. The prototype-to-production gap is cost, latency, safety, and observability — not the core functionality."*

The principle: **ship-ready AI is the prototype made cost-controlled, cached, streaming, latency-optimized, and observable — on top of evaluation and safety — which is the real prototype-to-production transition and what turns a notebook demo into a deployable, operable product worth putting in a portfolio.**
</details>

---

## 🧊 COLD REBUILD (do this before starting the next track — the honest gate)

**The task:** From an empty folder, take a bare Claude call and productionize it: prompt caching (proven with before/after token counts), streaming output, per-call cost+latency logging, and a one-line budget check that fails the run if cost exceeds a threshold.

**The rules (per AI-CONTRACT.md):** empty folder, ~20 minutes, NO notes, NO old code open. Paste this into Claude Code:

```text
Follow CLAUDE.md and AI-CONTRACT.md — this is a 🧊 COLD REBUILD. I'm cold-rebuilding Track 8's core: caching proven with numbers, streaming, cost/latency logging, and a budget check on a bare call. Time me (~20 min, soft limit).
Full Prime Directive: no worked examples, hint ladder only. When I finish (or the time is clearly gone),
grade the rebuild honestly — what came out fluently, what I fumbled, what I couldn't produce at all — log
it as evidence in LEARNING-RECORD, and give the gate verdict: proceed to the next track, or which modules
to re-run first. Recognizing is not retrieval; only what I PRODUCED counts.
```

*Why this exists: you can pattern-match through a capstone with your own notes open. You cannot fake a cold rebuild. This 20 minutes is the most honest signal in the whole track.*

---

*End of Track 8 — Production. 6 modules. Your AI systems can now be real: cost-controlled, cached, streaming (your frontend edge), latency-optimized, and observable. Combined with Track 7's evaluation and safety, you can take a prototype all the way to a deployable, operable product — crossing the gap most self-taught builders never cross.*

*Next: Track 9 — MCP & Modern AI Tooling, where you learn the Model Context Protocol (the standard way to connect AI to tools and data) and how to work as a professional in Cursor and Claude Code.*
