# TRACK 5 — TOOL USE & FUNCTION CALLING

*This is where the model stops just talking and starts DOING. Until now, Claude could only produce text. Tool use (a.k.a. function calling) lets the model call YOUR code — fetch live data, do a calculation, query a database, hit an API, take an action — and use the result in its answer. This is the bridge from "chatbot" to "agent," and understanding it deeply (especially the control-flow: the model doesn't run your code, it asks YOU to) is what makes the agents in Track 6 finally click. It's also some of the most impressive portfolio material you can build.*

*Prerequisites: Tracks 0–4 (you can call Claude reliably, handle structured output and failures, and build real pipelines). Python + the anthropic SDK.*

*Spine courses (you own them): **AI Agents Fundamentals v2** (tool use is the foundation of agents — your main reference) and **Build an AI Agent from Scratch v2** (you'll build the tool-use loop by hand, which is the best way to understand it). Anthropic's free **Building with the Claude API** course covers tool use directly too.*

*Gentle rhythm: STUDY → TINKER → REPORT → EXERCISE 1 → EXERCISE 2. Jargon decoded inline.*

*🧊 Cold opens + track-end cold rebuild. 📏 Evals thread: tool-call correctness cases ("this request must call THAT tool with THOSE arguments"). 🐛 Bug hunt in Module 5.5 (Level 2 — you'll know failures exist, not how many). 🔬 Autopsy in 5.7A: real shipped tool-use code. 🖼️ Capstone UI rep: tool-call progress states.*

---

## Module 5.1 — What Tool Use Is: The Model Asks, You Execute

**Difficulty:** ●●●○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Tool use / function calling**: giving the model a set of "tools" (functions it can request), so when it needs one, it outputs a structured request to call it — and YOUR code runs the function and returns the result.
- **The key control-flow fact**: the model does NOT run your code. It *asks* you to run a function (with arguments), your code runs it, and you send the result back. The model then continues.
- **Tool definition / schema**: a description of a tool — its name, what it does, and what arguments it takes (in a JSON schema). This is how the model knows what's available.

### Prep — Watch & Read

**Watch (Frontend Masters — you own these):**
- **AI Agents Fundamentals v2** → the tool-use / function-calling foundation sections. Main reference.
- **Build an AI Agent from Scratch v2** → the early parts where the tool-calling loop is built by hand.

**Read (gentle → deeper):**
- **Anthropic — Tool use overview** — https://docs.claude.com/en/docs/build-with-claude/tool-use — *Focus on: defining tools, the model requesting a tool, you returning a result.*
- **"Function calling explained simply"** — search `"llm function calling tool use explained simply"` — *Focus on: the model requests, your code executes.*
- **"Why function calling doesn't run code itself"** — search `"does the llm execute the function or just request it"` — *Focus on: the critical control-flow point.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 5, Module 5.1 — what tool use is, with the crucial control-flow
point. I find it confusing who actually runs the code, so make that crystal clear. Gentle rhythm; decode
tool use, schema, the request-vs-execute distinction.

PHASE 1 STUDY: Write a complete minimal tool-use example: define one simple tool (e.g. a calculator or a
get_current_time function), give it to the model, ask a question that needs it, and show the full loop:
the model RESPONDS WITH A REQUEST to call the tool (not the answer) → MY code runs the real function →
I send the result back → the model uses it to answer. Emphasize repeatedly: the model asks, MY code
executes. Show me each message in the exchange.

PHASE 2 TINKER: Have me trace the message sequence carefully (user → model's tool request → my tool result
→ model's final answer); change the tool's logic and see the answer change; ask something that DOESN'T need
the tool and watch the model just answer. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: (1) plain — what is tool use? (2) technical — the control flow
(model emits a structured tool-call request; the application executes; result returned; model continues),
and WHY it's designed this way (the model can't and shouldn't run arbitrary code; you stay in control).
Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a tool-use interaction with my own simple tool (e.g. look up something,
do a computation) and run the full loop — hints free.

PHASE 5 EXERCISE 2: Have me build a DIFFERENT single-tool interaction solo, trace the control flow, then
the verdict. Getting this control-flow mental model right is essential for agents.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Tool use (function calling)** gives the model a set of tools (functions it can request). When the model needs one, it doesn't answer — it outputs a structured **tool-call request** naming the tool and arguments. **Your code executes the actual function** and sends the result back; the model then continues to its answer. You define each tool with a **schema** (name, description, argument types) so the model knows what's available and how to call it.

**The crucial control-flow fact (the thing that confuses everyone):** the model does NOT run your code. It *asks* you to run a function; your application runs it and returns the result. The loop is: user message → model emits a tool-call request → your code executes the function → you send the result back as a tool-result message → model produces the final answer.

**Why it's designed this way:** the model is a text predictor — it can't execute code, access your database, or hit an API by itself, and you wouldn't want it to run arbitrary code unsupervised. Tool use keeps the model as the *decider* (which tool, what arguments) and your code as the *executor* (actually running it, safely). This separation is the foundation of everything agentic.

The interview answer: *"How does function calling work — does the model run the function?" → "No. The model emits a structured request to call a named tool with arguments; the application executes the real function and returns the result, then the model continues. The model decides what to call; your code executes it. This keeps the model as the reasoning layer and your code as the safe execution layer — the basis of agents."*

The principle: **tool use lets the model REQUEST a function call (deciding which tool and arguments), while YOUR code executes it and returns the result — the model decides, your code acts — and this request-vs-execute separation is the control-flow foundation of every agent.**
</details>

---

## Module 5.2 — Defining Good Tools: Schemas & Descriptions

**Difficulty:** ●●●○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Tool schema**: the structured definition (JSON schema) of a tool's name, description, and parameters — including types and which are required.
- **Tool description**: the natural-language explanation of what a tool does and when to use it. The model reads this to decide whether/how to call the tool — so it's basically a prompt.
- **Parameter description**: explaining each argument so the model fills it correctly.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agents Fundamentals v2** → the tool-definition / schema sections.
- **Build an AI Agent from Scratch v2** → defining tools the model can use.

**Read:**
- **Anthropic — Tool use (defining tools)** — https://docs.claude.com/en/docs/build-with-claude/tool-use — *Focus on: the input_schema, clear descriptions, required params.*
- **"Writing good tool descriptions for LLMs"** — search `"writing good tool descriptions function calling reliability"` — *Focus on: descriptions are prompts; clarity drives correct tool use.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 5, Module 5.2 — defining good tools. The schema and descriptions
are how the model knows what to do — they're effectively prompts. Gentle rhythm.

PHASE 1 STUDY: Write examples showing how tool DEFINITIONS drive behavior: a vague tool description (model
misuses or ignores the tool) vs a clear one (model uses it correctly); a schema with unclear vs clear
parameter descriptions. Show that the description is essentially a prompt the model reads to decide
when/how to call the tool. Explain schema, description, required params.

PHASE 2 TINKER: Have me degrade a good tool description and watch the model misuse the tool; improve a bad
one and watch it work; make a parameter ambiguous and see the model fill it wrong. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: why tool descriptions/schemas are critical (the model chooses
and fills tools based on them, like a prompt) and what makes a good one. Name the interview question and
answer.

PHASE 4 EXERCISE 1: Have me define a couple of well-described tools for a real use case and verify the
model uses them correctly — hints free.

PHASE 5 EXERCISE 2: Have me solo define tools for a different use case with excellent schemas/descriptions,
prove correct usage, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

A tool is defined by a **schema** (name, description, parameters with types, which are required) plus **descriptions** in natural language. The key insight: **the tool description is effectively a prompt.** The model reads it to decide *whether* to use the tool, *when*, and *how to fill the arguments* — so a vague description leads to the model misusing or ignoring the tool, while a clear one leads to correct use. Same for **parameter descriptions**: ambiguous ones get filled wrong.

**The theory (connects to prompting, Track 2):** the model selects and populates tools by predicting tokens conditioned on the tool definitions. So tool definitions are conditioning context — and all your prompt-engineering instincts (clarity, specificity, examples) apply to writing them. "Tool engineering" is prompt engineering for the model's actions.

**Why it matters:** in real agents with many tools, the model constantly decides which tool fits — and it can only decide well if each tool's description clearly states what it does and when to use it. Poor tool descriptions are a top cause of agents calling the wrong tool or filling bad arguments.

The interview answer: *"What makes a tool definition good?" → "Clear, specific descriptions of what the tool does and when to use it, plus well-described, correctly-typed parameters. The description functions as a prompt — the model decides whether and how to call the tool based on it — so clarity and specificity directly drive correct tool selection and argument filling. Vague tool descriptions are a common cause of misuse."*

The principle: **a tool definition's schema and descriptions are effectively prompts — the model selects and fills tools based on them — so clear, specific tool and parameter descriptions are what make tool use reliable, and 'tool engineering' is prompt engineering for actions.**
</details>

---

## Module 5.3 — The Tool-Use Loop: Multi-Step Tool Calling

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Tool-use loop**: the cycle of model-requests-tool → you-execute → return-result → model-continues, repeated until the model has everything it needs and gives a final answer. (This loop IS the skeleton of an agent.)
- **Multi-step / sequential tool use**: the model calls a tool, sees the result, then decides to call another based on it — chaining tools to solve a multi-part task.
- **Stop condition**: how you know the loop is done (the model stops requesting tools and returns a final answer).

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **Build an AI Agent from Scratch v2** → building the tool-calling loop (this is the core of the course and this module).
- **AI Agents Fundamentals v2** → multi-step tool use.

**Read:**
- **Anthropic — Tool use (handling the loop / multiple tools)** — https://docs.claude.com/en/docs/build-with-claude/tool-use — *Focus on: looping until no more tool calls, the stop reason.*
- **"Agent loop / tool calling loop"** — search `"llm tool calling loop until final answer"` — *Focus on: repeat execute-and-return until done.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 5, Module 5.3 — the tool-use loop. THIS loop is the skeleton of
an agent, so really nail it. Gentle rhythm; decode tool-use loop, multi-step, stop condition.

PHASE 1 STUDY: Write a complete tool-use LOOP: give the model a few tools and a task needing several of
them in sequence; loop — let the model request a tool, execute it, return the result, repeat — until the
model stops requesting tools and gives a final answer. Show how to detect the stop condition. Trace a
multi-step run where the model uses tool A, sees the result, then decides to use tool B.

PHASE 2 TINKER: Have me give it tasks needing 0, 1, and several tool calls and watch the loop adapt; add a
print at each loop iteration to SEE the model's tool choices unfold; create a task where the second tool
choice depends on the first tool's result. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the tool-use loop mechanics and why it's the foundation of
agents (the model reasons, acts via tools, observes results, and repeats). Name the interview question and
answer.

PHASE 4 EXERCISE 1: Have me build a multi-tool loop for a real multi-step task (e.g. "look up X, then
compute Y from it, then format Z") — hints free.

PHASE 5 EXERCISE 2: Have me build a different multi-step tool-using task solo, trace the loop, then the
verdict. This loop is literally the agent skeleton for Track 6 — the verdict should reflect readiness.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The **tool-use loop** is the cycle: model requests a tool → your code executes it → you return the result → the model continues (maybe requesting another tool based on what it just learned) → repeat until the model stops requesting tools and gives a final answer. You detect the **stop condition** from the model's response (it returns a final answer rather than another tool request). **Multi-step / sequential tool use** is the model chaining tools — using tool A, observing the result, then deciding to use tool B based on it.

**The huge realization:** this loop *is the skeleton of an agent.* An agent is, mechanically, this loop — the model reasoning about what to do, acting through tools, observing the results, and repeating until done. Everything in Track 6 (ReAct, plan-and-execute) is a structured version of this loop with added reasoning and planning. If you understand this loop deeply, agents will click immediately instead of feeling like magic.

**The theory:** each tool result becomes part of the conditioning context for the model's next decision (connect to Track 1) — so the model's later tool choices are informed by earlier tool results. That's how it solves multi-part tasks: it builds up context by acting and observing.

The interview answer: *"What's the tool-use loop, and how does it relate to agents?" → "It's the cycle: the model requests a tool, your code executes it and returns the result, the model continues — repeating until it stops requesting tools and answers. Each result conditions the next decision, so the model chains tools to solve multi-step tasks. This loop is the core of an agent — reason, act, observe, repeat — which is why understanding it is the foundation for everything agentic."*

The principle: **the tool-use loop — model-requests, you-execute, return-result, repeat-until-done — is the mechanical skeleton of an agent, where each tool result conditions the next decision so the model chains tools to solve multi-step tasks.** Master this loop and agents become obvious.
</details>

---

## Module 5.4 — Parallel Tool Calls & Real-World Tools

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK + real APIs

### New words in this module

- **Parallel tool use**: the model requesting multiple independent tools at once (e.g. check weather in 3 cities) so you can execute them together, faster.
- **Real-world tools**: tools that hit actual external services — a web search, a public API, a database query, a file reader — not just toy functions.
- **Tool result formatting**: returning the tool's output in a clean form the model can use (often a concise string or JSON, not a raw dump).

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agents Fundamentals v2** → tools that interact with real systems.
- **Build an AI Agent from Scratch v2** → wiring real tools.

**Read:**
- **Anthropic — Tool use (parallel tools)** — https://docs.claude.com/en/docs/build-with-claude/tool-use — *Focus on: multiple tool calls in one turn.*
- **"Giving LLMs real tools (web search, APIs)"** — search `"llm agent tools web search api database"` — *Focus on: connecting to real services safely.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 5, Module 5.4 — parallel tool calls & real-world tools. Now the
tools DO real things. Gentle rhythm.

PHASE 1 STUDY: Show me (a) parallel tool use — the model requesting several independent tools at once and
me executing them together; (b) a couple of REAL tools — e.g. a real public API call and a local data
lookup — wired in properly, with the results formatted cleanly for the model. Explain parallel tool use and
clean result formatting.

PHASE 2 TINKER: Have me build a task triggering parallel calls and see them handled together; feed the
model messy vs clean tool results and observe which it uses better; handle a real API returning unexpected
data. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: when parallel tool use helps (independent calls) and why clean
result formatting matters (the result becomes conditioning context). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a useful multi-tool assistant with at least one REAL external tool (e.g.
fetch live data + process it) — hints free.

PHASE 5 EXERCISE 2: Have me build a different real-tool assistant solo, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Parallel tool use** is the model requesting multiple *independent* tools in one turn (e.g. "weather in Delhi, Mumbai, and Pune") so you can execute them together rather than one-by-one — faster and more efficient. **Real-world tools** hit actual services: a web search, a public API, a database query, a file reader — turning the model from a talker into something that acts on live data. **Tool result formatting** matters because the result becomes conditioning context (Track 1): a clean, concise result the model can parse beats a raw data dump.

**Why this is where it gets exciting (and portfolio-worthy):** once the model can call real tools — fetch live data, query your database, search the web, read files — you can build genuinely useful assistants, not demos. "An assistant that answers questions using live data from [real API]" is impressive portfolio material, especially with your frontend skills wrapping it.

The interview answer: *"What's parallel tool use, and what matters when wiring real tools?" → "Parallel tool use is the model requesting multiple independent tools in one turn so they can be executed together for speed. When wiring real tools, the result becomes the model's context, so I format it cleanly and concisely, handle the external service's failures and unexpected responses, and keep execution safe — the model decides, but my code controls what actually runs."*

The principle: **parallel tool use executes independent tool calls together for speed, and real-world tools (APIs, search, databases) turn the model into something that acts on live data — with clean result formatting and robust failure handling, because each tool result becomes the model's conditioning context.**
</details>

---

## Module 5.5 — Handling Tool Failures & Errors Gracefully

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Tool failure**: a tool throws an error, times out, or returns something unexpected (the API was down, the input was bad). The model needs to know so it can adapt.
- **Error-as-result**: returning the error back to the model as the tool result, so it can react (retry differently, try another tool, or tell the user) instead of the whole thing crashing.
- **Validation of tool arguments**: checking the arguments the model wants to pass before executing (it can hallucinate bad arguments).

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agent: From Prototype to Production** → the robustness / error-handling sections (this course is about making agents reliable — perfect here).
- **Build an AI Agent from Scratch v2** → handling failures in the loop.

**Read:**
- **Anthropic — Tool use (error handling)** — https://docs.claude.com/en/docs/build-with-claude/tool-use — *Focus on: returning errors to the model.*
- **"Handling tool errors in agents"** — search `"agent tool error handling retry graceful"` — *Focus on: error-as-result, letting the model recover.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 5, Module 5.5 — handling tool failures gracefully. Real tools
fail; robust systems handle it. This is the production mindset from Track 2.8 applied to tools. Gentle
rhythm but push on robustness.

PHASE 1 STUDY: Write examples where a tool fails (error, timeout, bad/unexpected output, or the model
passes bad arguments) and show the robust pattern: catch the failure, return the ERROR back to the model as
the tool result so it can ADAPT (retry differently, try another tool, or explain to the user) — instead of
crashing. Also show validating the model's tool arguments before executing (it can hallucinate bad args).

PHASE 2 TINKER: Have me make tools fail in different ways and watch the model recover when the error is
returned to it vs the program crashing when it isn't; have the model pass an invalid argument and add
validation. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: why returning errors to the model (error-as-result) makes
agents robust, and why I must validate tool arguments. Connect to the non-determinism/reliability mindset.
Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me make a multi-tool assistant robust to tool failures (error-as-result + arg
validation) and prove it recovers — hints free.

PHASE 5 EXERCISE 2 — 🐛 BUG HUNT, LEVEL 2: Scaffold a multi-tool assistant that LOOKS robust but isn't —
plant realistic failure-handling flaws (e.g. an error swallowed instead of returned to the model, a retry
that repeats the identical failing call, an unvalidated argument path, a timeout that hangs instead of
failing). Confirm only that flaws exist — NOT how many. At least one plant should misbehave only
intermittently, so I must distinguish "flaky" from "fixed" with repeated runs, not one lucky pass. I hunt
under full Prime Directive; standard hint ladder. I must prove each fix with tool-call eval cases (📏),
then the verdict. This robustness is essential before agents (Track 6).
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

Real tools fail — APIs go down, time out, return unexpected data, or the model passes bad arguments. The robust pattern is **error-as-result**: catch the failure and return the *error* back to the model as the tool result, so the model can **adapt** — retry with different arguments, try another tool, or explain to the user — instead of your program crashing. You also **validate the model's tool arguments** before executing, because the model can hallucinate invalid arguments (a non-existent ID, a malformed value).

**The theory (connects to Track 2.8 reliability + the control-flow of 5.1):** because your code is the executor, your code owns failure handling. Returning the error *to the model* (rather than crashing) leverages the model's ability to reason about the failure and recover — turning a brittle pipeline into an adaptive one. This is the same "build the failure path" discipline, now applied to actions: a demo assumes tools succeed; a real agent assumes they sometimes won't.

**Why it's essential before agents:** an agent runs many tool calls autonomously; if any failure crashes it, it's useless. Graceful tool-failure handling is what lets an agent run a long multi-step task without falling over.

The interview answer: *"How do you handle tool failures in an agent?" → "Catch the failure and return the error to the model as the tool result, so it can adapt — retry, switch tools, or inform the user — instead of crashing. I also validate the model's tool arguments before executing, since it can hallucinate bad ones. Because the application is the executor, robust failure handling lives in my code, and surfacing errors back to the model makes the system adaptive rather than brittle."*

The principle: **robust tool use returns failures back to the model as results (error-as-result) so it can adapt, and validates the model's tool arguments before executing — applying the 'build the failure path' discipline to actions, which is what lets an agent run long autonomous tasks without crashing.**
</details>

---
## Module 5.6 — Tool Use vs RAG vs Just-Prompting: Choosing the Right Approach

**Difficulty:** ●●●○○ · **Format:** STUDY → investigation · **Stack:** concepts + Python

### New words in this module

- **Approach selection**: deciding, for a given problem, whether you need plain prompting, RAG (retrieve knowledge), tool use (take actions/get live data), or a combination.
- **Live vs static data**: tool use fetches *live/changing* data (today's weather, current stock price, a database row); RAG retrieves from a *known corpus* of documents.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI for Software Engineers** → architecture/decision sections on when to use what.
- **AI Engineering Fundamentals** → comparing approaches.

**Read:**
- **"RAG vs tool use vs fine-tuning when to use"** — search `"rag vs tools vs fine tuning decision"` — *Focus on: matching approach to problem.*
- **"When does an LLM need tools vs retrieval"** — search — *Focus on: live actions/data vs document knowledge.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 5, Module 5.6 — choosing the right approach. Now that I have
prompting, RAG, and tools, I need judgment about which to use. Gentle rhythm; this is a thinking module.

PHASE 1 STUDY: Walk me through a decision framework with concrete examples: when plain prompting suffices;
when I need RAG (answer from a known document corpus); when I need tool use (live data, actions,
computations); and when I combine them (e.g. a tool that does retrieval, or RAG + tools). Emphasize live/
changing data + actions → tools; static document knowledge → RAG.

PHASE 2 TINKER: Give me several problem statements and have me classify each (prompt-only / RAG / tools /
combo) and justify; build one tiny example of the right choice for two of them. Predict, then check
reasoning.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the decision framework and why matching approach to problem
matters (using the wrong one is over-engineering or won't work). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me design (not fully build) the right architecture for a real problem I care
about, justifying the approach — hints free.

PHASE 5 EXERCISE 2: Have me solo choose and justify the approach for a different real problem, sketch it,
then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

Now that you have three tools in your kit — prompting, RAG, tool use — the skill is **choosing the right one**: plain **prompting** when the model's own capabilities suffice (summarizing, rewriting, reasoning over provided text); **RAG** when you need to answer from a *known corpus* of documents (your docs, a knowledge base); **tool use** when you need *live/changing data* (today's price, a current database row) or to *take actions* (send an email, run a calculation, hit an API); and **combinations** (a tool that does retrieval, or RAG plus tools) for complex systems.

**The key distinction:** RAG retrieves from a static document corpus; tool use fetches live data or performs actions. "What does our policy say about X?" → RAG. "What's the current status of order #123?" → tool use (live database). "Summarize this text" → just prompting.

**Why judgment matters:** using the wrong approach is either over-engineering (RAG when prompting would do) or doomed (prompting when you need live data the model can't know). An engineer who picks the right architecture looks far more senior than one who reaches for the most complex option by default.

The interview answer: *"How do you decide between prompting, RAG, and tool use?" → "Plain prompting when the model's own abilities suffice; RAG when answering from a known document corpus; tool use for live/changing data or to take actions; and combinations for complex needs. The key split is static document knowledge (RAG) versus live data or actions (tools). Matching approach to problem avoids both over-engineering and approaches that simply can't work."*

The principle: **prompting, RAG, and tool use solve different problems — model-capability tasks, static-document knowledge, and live-data/actions respectively — and choosing the right one (or combination) is an architecture skill that distinguishes a senior engineer from someone who defaults to the most complex option.**
</details>

---

## Module 5.7 — Tool Use Reliability & Security Basics

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Tool-use reliability**: making sure the model calls the right tools with valid arguments consistently, across varied inputs.
- **Dangerous tools**: tools that take consequential actions (delete data, send money, email people). These need extra safeguards — confirmation, permission checks, limits.
- **Least privilege**: giving a tool only the minimum access it needs, so a mistake (or an injection attack) can't do much damage.
- **Prompt injection via tools** (preview): malicious content from a tool result (e.g. a web page) trying to hijack the model. (Full treatment Track 7.)

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agent: From Prototype to Production** → safety/reliability/guardrails sections.

**Read:**
- **Anthropic — Tool use best practices / safety** — https://docs.claude.com/en/docs/build-with-claude/tool-use — *Focus on: safe tool design.*
- **"Agent security least privilege"** — search `"ai agent tool security least privilege confirmation"` — *Focus on: limiting what tools can do.*
- **"Prompt injection via tool results"** — search `"indirect prompt injection tool result web content"` — *Focus on: untrusted tool output as an attack vector.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 5, Module 5.7 — tool-use reliability & security basics. Once
tools can DO things, safety matters. Gentle rhythm; decode dangerous tools, least privilege, injection-via-
tools.

PHASE 1 STUDY: Show me (a) reliability — measuring whether the model calls the right tool with valid args
across many inputs, and improving it (better descriptions, validation, retries); (b) security basics — for
a "dangerous" tool (deletes/sends/spends), add safeguards: argument validation, confirmation/permission
gates, limits, and least privilege. Briefly show how a malicious tool RESULT (e.g. web content saying
"ignore your instructions and...") could try to hijack the model — previewing prompt injection.

PHASE 2 TINKER: Have me test tool-call reliability across varied phrasings; add a confirmation gate to a
destructive tool; simulate a malicious tool result and see the risk. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: how to make tool use reliable, and the security principles
(least privilege, confirmation for dangerous actions, treating tool results as untrusted). Name the
interview question and answer.

PHASE 4 EXERCISE 1: Have me make a tool-using system both reliable and safe (validation + a guarded
dangerous action) — hints free.

PHASE 5 EXERCISE 2: Have me solo secure a different tool-using system (least privilege + confirmation +
input validation), then the verdict. Full safety/injection coverage is Track 7; this is the foundation.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

Once tools can *do things*, reliability and security become real concerns. **Reliability**: measure whether the model calls the right tool with valid arguments across varied inputs, and improve it with clearer tool descriptions (5.2), argument validation (5.5), and retries. **Security basics** for **dangerous tools** (those that delete data, send money, email people): add **argument validation**, **confirmation/permission gates** (human approval before a consequential action), limits, and **least privilege** (give each tool only the minimum access it needs, so a mistake or attack can't cause much harm). And a preview of **prompt injection via tools**: a tool result (like fetched web content) can contain malicious text trying to hijack the model ("ignore your instructions and…") — so tool results are *untrusted input*.

**The theory:** the model is the decider but can be wrong or manipulated, and your code is the executor — so safety lives in your code's constraints on what tools can do and what gets executed. Least privilege and confirmation gates ensure the model's mistakes (or an injection) have limited blast radius.

The interview answer: *"How do you make tool use safe?" → "Validate the model's arguments, require confirmation for consequential actions, apply least privilege so each tool has minimal access, and treat tool results as untrusted input — they can carry prompt-injection attempts. Because the model decides but my code executes, safety is enforced by constraining what the code will actually do, limiting the blast radius of any mistake or attack."*

The principle: **tool-use safety means validating arguments, gating dangerous actions behind confirmation, applying least privilege, and treating tool results as untrusted — because the model can be wrong or manipulated, so your code (the executor) must constrain the blast radius of any action.**
</details>

---

## Module 5.7A — 🔬 AUTOPSY: Read Real, Shipped Tool-Use Code

**Difficulty:** ●●●●○ · **Format:** 🔬 Autopsy (read, predict, compare) · **Stack:** reading real code (Python)

### New words in this module

- *(None new — the autopsy protocol from 4.7A applies.)*

### Prep — Watch & Read

**Read (live, during the autopsy):**
- **Anthropic Cookbook — tool use recipes** (e.g. the calculator tool, the customer-service agent) — https://github.com/anthropics/anthropic-cookbook — *real reference implementations of the exact loop you hand-built in 5.3.*
- *(Repo moved? Find the closest current real tool-use implementation — the protocol is the point.)*

### 🔬 AUTOPSY (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md — 🔬 AUTOPSY, full protocol. Track 5, Module 5.7A. I hand-built the
tool-use loop in 5.3 and hardened it in 5.5; now I read how shipped code does it.

SETUP: Locate a real tool-use implementation in the Anthropic Cookbook (or closest equivalent) — the loop,
the schemas, the execution, the error paths. One focused implementation, ~45–60 minutes.

THE AUTOPSY: (1) PREDICT before reading each function — especially the loop's control flow and the error
paths — then read and verify, gaps said out loud and logged. (2) COMPARE to my 5.3/5.5 loop: how do their
schemas/descriptions differ from mine and why might theirs steer the model better? How do they handle a
failing tool vs my error-as-result approach? At least one difference named and judged (better vs just
different, and what constraint drove it). (3) INTERROGATE: what does their code assume the model might do
wrong, and where is that assumption visible in the code? (4) EXTRACT one production trick into NOTES.md,
with where I'll apply it in the 5.8 capstone.

Verdict: engineer or tourist?
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

Tool-use code is where defensive engineering against a stochastic model becomes visible: shipped implementations encode, in their schemas, validation, and error paths, a catalogue of everything the model does wrong in practice. Reading them is reading a map of failure modes you haven't personally hit yet — cheaper than hitting them all yourself. Your 5.3 loop was correct; theirs is correct AND scarred, and the difference between those two is precisely what this autopsy extracts.

The principle: **shipped tool-use code is a fossil record of model misbehavior — its defensive choices teach you failure modes secondhand, before production teaches you them firsthand.**
</details>

---

## Module 5.8 — Tool Use Capstone: A Useful Multi-Tool Assistant

**Difficulty:** ●●●●● · **Format:** Guided capstone · **Stack:** Python + anthropic SDK + real tools

### New words in this module

- *(None new — integrates the track.)*

### Prep — Watch & Read

**Watch (Frontend Masters):**
- Revisit **Build an AI Agent from Scratch v2** and **AI Agents Fundamentals v2** as a checklist — you're about to build the thing they lead to.

**Read:**
- Revisit your `NOTES.md` from 5.1–5.7.

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 5, Module 5.8 — tool use capstone. Completed 5.1–5.7. Build a
genuinely useful multi-tool assistant. Calibrate toward independence (ZPD).

PHASE 1 STUDY (light): Map the pieces: well-defined tools (good schemas/descriptions) + the tool-use loop +
some real tools + parallel calls where useful + graceful failure handling + basic safety. That's a real
assistant.

PHASE 2 BUILD: Guide me to build a useful multi-tool assistant for a real use case I care about — e.g. a
research assistant (web search + summarize + save), or a data assistant (query a dataset + compute +
format), or a personal-productivity assistant. Several well-described tools, the loop, at least one real
external tool, failure handling, and a safety gate on anything consequential. Let me drive; hint when stuck.

PHASE 3 REPORT: I report. Make me justify the tool design, demonstrate multi-step tool use solving a real
task, and show it handles failures + unsafe actions gracefully.

PHASE 4 EXERCISE 1: Have me add a new tool/capability to the assistant with the same rigor — hints
available.

PHASE 5 EXERCISE 2: Have me build a DIFFERENT useful multi-tool assistant solo, prove it with tool-call
eval cases (📏), then the big verdict: am I ready for Track 6 (Agents), or which tool-use concept to
revisit? This is strong portfolio material — note it.

PHASE 6 — 🖼️ UI REP (expert mode, from scratch): wrap the assistant in a minimal Next.js UI that shows
TOOL-CALL PROGRESS STATES — each tool invocation rendered live as pending → running → succeeded/failed,
with the result. This is the step up from Track 4's rep: the UI now reflects a multi-step process, which
means real state management for an async sequence — squarely my expert domain, so full CLAUDE.md rules
(no hand-holding, rigorous review). Gentleness only at the Python↔frontend seam (how the backend
communicates step events — polling or a stream, my choice, but make me justify it).
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The capstone integrates the track into a genuinely useful multi-tool assistant: well-defined **tools** (good schemas/descriptions — 5.2), the **tool-use loop** (5.3), **real external tools** (5.4), **parallel calls** where useful, graceful **failure handling** (5.5), and basic **safety** (5.7). This is a real assistant that *does things*, not a chatbot — and it's the direct precursor to an agent (Track 6 adds reasoning, planning, and memory on top of exactly this).

You've now built the bridge from "talks" to "does." With your frontend skills, a multi-tool assistant with a clean UI is excellent portfolio material — "an assistant that researches/computes/acts using live tools" demonstrates real applied-AI capability.

The interview answer (whole track): *"How would you build an assistant that can take actions?" → "Define clear tools with good schemas and descriptions, run the tool-use loop (model requests, my code executes, return result, repeat until done), wire in real external tools, handle failures by returning errors to the model so it adapts, run independent calls in parallel, and gate consequential actions with validation and confirmation. The model decides; my code safely executes — that separation is the foundation, and adding reasoning and planning on top turns it into an agent."*

The principle: **a useful multi-tool assistant combines well-defined tools, the tool-use loop, real external tools, parallel execution, graceful failure handling, and safety gates — turning the model from a talker into something that acts, and forming the direct foundation for agents.** You've crossed from chatbot to action.
</details>

---

## 🧊 COLD REBUILD (do this before starting the next track — the honest gate)

**The task:** From an empty folder, hand-build the tool-use loop with two tools: schemas, the request-execute-return cycle, a multi-step task requiring both tools, and error-as-result when one tool fails — with tool-call eval cases.

**The rules (per AI-CONTRACT.md):** empty folder, ~20 minutes, NO notes, NO old code open. Paste this into Claude Code:

```text
Follow CLAUDE.md and AI-CONTRACT.md — this is a 🧊 COLD REBUILD. I'm cold-rebuilding Track 5's core: the tool-use loop from scratch with two tools, multi-step calling, and error-as-result on failure. Time me (~20 min, soft limit).
Full Prime Directive: no worked examples, hint ladder only. When I finish (or the time is clearly gone),
grade the rebuild honestly — what came out fluently, what I fumbled, what I couldn't produce at all — log
it as evidence in LEARNING-RECORD, and give the gate verdict: proceed to the next track, or which modules
to re-run first. Recognizing is not retrieval; only what I PRODUCED counts.
```

*Why this exists: you can pattern-match through a capstone with your own notes open. You cannot fake a cold rebuild. This 20 minutes is the most honest signal in the whole track.*

---

*End of Track 5 — Tool Use & Function Calling. 9 modules (including one 🔬 autopsy). The model can now DO things — call your code, fetch live data, take actions — through the request-execute loop you built and understand deeply. You've built the skeleton of an agent and a useful multi-tool assistant, and you know how to make tool use reliable and safe.*

*Next: Track 6 — Agents, where you add reasoning, planning, and memory on top of the tool-use loop to build systems that autonomously accomplish multi-step goals — the frontier of applied AI and the most impressive thing you can build.*
