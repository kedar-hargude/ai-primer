# TRACK 6 — AGENTS

*The frontier of applied AI, and the most impressive thing you can build. An agent is a system where the LLM autonomously decides what to do, takes actions (via the tool use you just learned), observes the results, and keeps going until it accomplishes a goal — with reasoning, planning, and memory layered on top of the tool-use loop. This track demystifies agents completely: you'll see they're not magic, just a well-structured loop, and you'll build several from scratch. You'll also learn the most senior lesson of all — when an agent is the WRONG tool and a simpler approach wins. Agents are where the "hunted by recruiters" portfolio projects live.*

*Prerequisites: Track 5 especially (the tool-use loop IS the agent skeleton) and Tracks 0–4. Python + the anthropic SDK.*

*Spine courses (you own all three — this is the track they're for): **AI Agents Fundamentals v2** (the concepts), **Build an AI Agent from Scratch v2** (build the loop by hand — the best way to truly get it), and **AI Agent: From Prototype to Production** (making agents reliable and real). Lean on all three.*

*Gentle rhythm: STUDY → TINKER → REPORT → EXERCISE 1 → EXERCISE 2. You're experienced in this primer's style by now, so scaffolding tapers — but hints stay available.*

*🧊 Cold opens + track-end cold rebuild. 📏 Evals thread: agent evals judge trajectories and outcomes, not just final text. 🐛 Bug hunt in Module 6.8 (Level 3 — full Prime Directive: the agent simply "behaves badly" and you characterize the failure before hunting it). 🔬 Autopsy in 6.8A: a real open-source agent loop. 🖼️ Capstone UI rep: render the agent's live trajectory.*

---

## Module 6.1 — What an Agent Actually Is: The Reasoning Loop

**Difficulty:** ●●●○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Agent**: an LLM-powered system that pursues a goal autonomously — it decides actions, takes them (tools), observes results, and repeats until done, rather than answering in one shot.
- **Autonomy**: the model drives the process — choosing what to do next based on what it has observed — instead of following a fixed script you wrote.
- **The agent loop**: think (what should I do?) → act (use a tool) → observe (see the result) → repeat. This is the tool-use loop from Track 5 with explicit reasoning.

### Prep — Watch & Read

**Watch (Frontend Masters — you own all three):**
- **AI Agents Fundamentals v2** → the "what is an agent" foundation. Main reference.
- **Build an AI Agent from Scratch v2** → the core loop being built.

**Read (gentle → deeper):**
- **Anthropic — Building effective agents** — https://www.anthropic.com/research/building-effective-agents — *Read this; it's the definitive practical guide. Focus on: agents vs workflows, and keeping things simple.*
- **"What is an AI agent, explained"** — search `"what is an ai agent llm explained simply"` — *Focus on: autonomous think-act-observe loop.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 6, Module 6.1 — what an agent actually is. I built the tool-use
loop in Track 5; show me an agent is that loop + reasoning, NOT magic. Gentle rhythm; decode agent,
autonomy, the loop.

PHASE 1 STUDY: Write a minimal but real agent: give the model a goal and some tools, and loop — the model
reasons about what to do, picks a tool, my code executes it, the result goes back, and it repeats until it
decides the goal is met. Make the model's REASONING visible at each step (have it say what it's doing and
why). Show it's the Track 5 loop with explicit think-act-observe. Use a goal needing a few steps.

PHASE 2 TINKER: Have me give it goals of varying complexity and watch the loop adapt; print each
think-act-observe cycle to SEE the agent "thinking"; give it a goal it can't fully achieve and see what it
does. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: (1) plain — what is an agent? (2) technical — the
think-act-observe loop, how it extends tool use with autonomous decision-making, and why it's not magic
(it's structured looping over the tool-use mechanism). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a simple agent for a real multi-step goal I care about — hints free.

PHASE 5 EXERCISE 2: Have me build a different simple agent solo, trace its reasoning loop, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

An **agent** is an LLM system that pursues a goal **autonomously**: it reasons about what to do, **acts** (via tools — Track 5), **observes** the result, and repeats until the goal is met — instead of answering in one shot. The **agent loop** is *think → act → observe → repeat*, which is exactly the tool-use loop from Track 5 with **explicit reasoning** added at each step.

**The big demystification:** an agent is NOT magic and NOT a special kind of model. It's the tool-use loop you already built, wrapped with the model reasoning about what to do next based on what it has observed. The "intelligence" is the model making each decision conditioned on the goal plus everything it has done and seen so far (Track 1: each observation becomes conditioning context). Once you see this, agents stop being intimidating — they're a control structure around the model, not a mystery.

**Autonomy** is the distinguishing feature: in a fixed pipeline, *you* decide the steps; in an agent, the *model* decides the next step based on observations. That's more flexible (handles varied/unpredictable tasks) but also less predictable (Track 7 evaluation becomes essential).

The interview answer: *"What is an AI agent?" → "An LLM system that autonomously pursues a goal via a think-act-observe loop: it reasons about what to do, takes an action through tools, observes the result, and repeats until done. Mechanically it's the tool-use loop with explicit reasoning — the model decides each next step conditioned on the goal and everything observed so far. The defining feature is autonomy: the model chooses the steps rather than following a fixed script."*

The principle: **an agent is the tool-use loop plus autonomous reasoning — think, act, observe, repeat — where the model decides each next step based on observations, making it flexible for unpredictable tasks but less predictable; it's a control structure around the model, not magic.**
</details>

---

## Module 6.2 — ReAct: Reasoning + Acting Interleaved

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **ReAct (Reason + Act)**: the foundational agent pattern where the model alternates between *reasoning* ("I need to find X, so I'll use tool Y") and *acting* (using the tool), using each observation to inform the next reasoning step.
- **Thought / Action / Observation**: the three repeating parts of a ReAct step — the model thinks, acts, then sees the observation.
- **Trajectory**: the full sequence of thought-action-observation steps an agent took to reach its answer.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agents Fundamentals v2** → the reasoning-pattern sections (ReAct).
- **Build an AI Agent from Scratch v2** → building reasoning into the loop.

**Read:**
- **"ReAct prompting / agents explained"** — search `"react reasoning acting agents explained"` — *Focus on: interleaving thought and action.*
- **Anthropic — Building effective agents** — https://www.anthropic.com/research/building-effective-agents — *Focus on: the augmented-LLM loop.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 6, Module 6.2 — ReAct (reason + act). The foundational agent
pattern. Gentle rhythm; decode ReAct, thought/action/observation, trajectory.

PHASE 1 STUDY: Write an agent that follows ReAct explicitly: at each step the model produces a THOUGHT
(reasoning about what to do), an ACTION (a tool call), then sees an OBSERVATION (the result) — and uses
that to inform the next thought. Make the thought-action-observation trajectory fully visible. Show how
the reasoning makes the agent smarter than blind tool-calling.

PHASE 2 TINKER: Have me watch the trajectory on a multi-step problem; remove the explicit reasoning and see
the agent get worse (connect to chain-of-thought, Track 2.4); give it a problem where it must adjust its
plan based on an observation. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what ReAct is and WHY interleaving reasoning with action helps
(each thought conditions a better action; each observation conditions a better next thought — it's
chain-of-thought applied to actions). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a ReAct agent for a real problem and show its trajectory — hints free.

PHASE 5 EXERCISE 2: Have me build a different ReAct agent solo, inspect its reasoning quality, then the
verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**ReAct (Reason + Act)** is the foundational agent pattern: the model alternates **Thought** (reasoning about what to do next) → **Action** (a tool call) → **Observation** (the result) → and uses that observation to inform the next thought, repeating until done. The full sequence of these steps is the agent's **trajectory**.

**Why it works (connects to chain-of-thought, 2.4):** explicit reasoning before each action makes the action better (the model "thinks" about what it needs before acting), and each observation becomes conditioning context for the next thought (so the agent adapts based on what it learns). It's literally chain-of-thought applied to actions — the reasoning steps improve decision quality the same way they improve answer quality. An agent that just blindly calls tools without reasoning is markedly worse than one that thinks first.

**Why it's foundational:** ReAct is the base pattern most agents build on. Understanding the thought-action-observation cycle — and being able to inspect a trajectory to see where an agent's reasoning went wrong — is core agent literacy.

The interview answer: *"What is ReAct?" → "An agent pattern interleaving Thought (reasoning), Action (a tool call), and Observation (the result), repeating until the goal is met. The reasoning before each action improves decision quality, and each observation conditions the next thought so the agent adapts — it's chain-of-thought applied to actions. The trajectory of thought-action-observation steps is also how you debug an agent's behavior."*

The principle: **ReAct interleaves reasoning and acting — Thought, Action, Observation, repeat — so each action is informed by reasoning and each observation informs the next decision; it's chain-of-thought for actions, the foundational agent pattern, and the trajectory is how you inspect agent behavior.**
</details>

---

## Module 6.3 — Planning: Plan-and-Execute Agents

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Plan-and-execute**: an agent pattern where the model first makes a *plan* (a list of steps) for the whole task, then executes the steps — rather than deciding purely one step at a time (ReAct).
- **Decomposition**: breaking a complex goal into smaller sub-tasks.
- **Re-planning**: updating the plan when something unexpected happens during execution.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agents Fundamentals v2** → planning patterns.
- **AI Agent: From Prototype to Production** → structuring complex agent tasks.

**Read:**
- **"Plan and execute agents"** — search `"plan and execute agent llm decomposition"` — *Focus on: plan first, then execute, then re-plan.*
- **Anthropic — Building effective agents (orchestrator-workers, planning)** — https://www.anthropic.com/research/building-effective-agents — *Focus on: when to plan vs react.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 6, Module 6.3 — plan-and-execute agents. Gentle rhythm; decode
plan-and-execute, decomposition, re-planning.

PHASE 1 STUDY: Write an agent that first PLANS (decomposes the goal into steps) then EXECUTES each step,
and RE-PLANS if something unexpected happens. Contrast with pure ReAct (decide one step at a time). Show a
complex task where planning first produces a more coherent result than reacting step-by-step.

PHASE 2 TINKER: Have me give it a complex multi-part goal and watch it plan then execute; introduce a
surprise mid-execution and see it re-plan; compare plan-and-execute vs ReAct on the same task. Predict-
then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: plan-and-execute vs ReAct, when each is better (planning for
complex multi-step tasks with dependencies; ReAct for exploratory/uncertain tasks), and why re-planning
matters. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a plan-and-execute agent for a complex real task — hints free.

PHASE 5 EXERCISE 2: Have me build a different planning agent solo, including re-planning, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Plan-and-execute** agents first make a **plan** (decompose the goal into a sequence of steps), then execute the steps — versus ReAct, which decides one step at a time. **Decomposition** is breaking a complex goal into manageable sub-tasks; **re-planning** is updating the plan when execution reveals something unexpected.

**When each is better:** plan-and-execute shines for **complex, multi-step tasks with dependencies** where thinking through the whole approach first produces a more coherent result (and is more token-efficient than re-reasoning every step). Pure ReAct shines for **exploratory or uncertain tasks** where you can't plan ahead because each step depends heavily on what you discover. Many real agents combine them: plan at a high level, react within steps, and re-plan when reality diverges from the plan.

**The theory:** planning front-loads the reasoning (decompose once, execute many), which keeps a long task coherent and reduces the chance of the agent wandering. Re-planning adds adaptivity so the plan isn't brittle. This mirrors how a person tackles a big task: sketch a plan, work it, adjust when surprised.

The interview answer: *"ReAct vs plan-and-execute — when do you use each?" → "Plan-and-execute decomposes the goal into a plan up front, then executes (with re-planning when surprised) — good for complex multi-step tasks with dependencies, keeping them coherent and efficient. ReAct decides step-by-step — good for exploratory, uncertain tasks. Often you combine them: plan high-level, react within steps, re-plan when execution diverges."*

The principle: **plan-and-execute agents decompose a goal into a plan then execute it (re-planning when surprised), suiting complex dependent tasks, while ReAct decides step-by-step for exploratory tasks — and mature agents combine planning for coherence with reactive adaptation.**
</details>

---

## Module 6.4 — Agent Memory: Short-Term and Long-Term

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK + vector DB

### New words in this module

- **Short-term memory**: what the agent keeps in its current context (the running conversation/trajectory). Limited by the context window (Track 1.4).
- **Long-term memory**: information the agent stores outside the context (e.g. in a vector DB) and retrieves when relevant — so it can "remember" across sessions or beyond the window.
- **Memory management**: deciding what to keep in context, what to summarize, what to store externally, and what to retrieve — the context-window problem from Track 1.4, now for agents.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agents Fundamentals v2** → memory sections.
- **AI Agent: From Prototype to Production** → managing state/memory in real agents.

**Read:**
- **"Agent memory short term long term"** — search `"llm agent memory short term long term vector"` — *Focus on: context vs external store.*
- **"Memory management for LLM agents"** — search — *Focus on: summarization, retrieval, what to keep.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 6, Module 6.4 — agent memory. This is the Track 1.4 context-
window problem applied to agents, plus long-term memory via retrieval (Track 3–4). Gentle rhythm.

PHASE 1 STUDY: Show me (a) short-term memory — the agent's running context/trajectory, and what happens as
it grows toward the context limit; (b) long-term memory — storing information in a vector DB and retrieving
relevant bits when needed, so the agent "remembers" beyond the window or across sessions. Show memory
management: keep recent, summarize old, store/retrieve important facts.

PHASE 2 TINKER: Have me run an agent long enough to strain short-term memory and apply summarization; give
the agent a long-term memory store and watch it recall a fact from a previous session; observe what to keep
vs retrieve. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: short vs long-term memory, and how memory management is the
context-window problem (1.4) plus retrieval (Track 3–4) applied to agents. Name the interview question and
answer.

PHASE 4 EXERCISE 1: Have me add long-term memory (vector-store recall) to an agent and demonstrate cross-
session memory — hints free.

PHASE 5 EXERCISE 2: Have me build a different agent with both memory types solo, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

Agents need memory. **Short-term memory** is what's in the current context — the running trajectory/conversation — limited by the context window (1.4). **Long-term memory** is information stored *outside* the context (typically in a vector DB) and retrieved when relevant, so the agent can "remember" across sessions or beyond the window. **Memory management** is deciding what to keep in context, what to summarize, what to store externally, and what to retrieve.

**The beautiful connection:** this is the **context-window problem from Track 1.4** (keep/drop/summarize) combined with **retrieval from Tracks 3–4** (store in a vector DB, retrieve by similarity when relevant), now applied to agents. Long-term agent memory is literally RAG over the agent's own past experiences. Once you see this, agent memory isn't a new concept — it's two things you already deeply understand, composed.

**Why it matters:** a long-running agent (or one that should remember a user across sessions) would otherwise forget everything when the context fills or the session ends. Memory management is what makes agents persistent and coherent over time.

The interview answer: *"How does agent memory work?" → "Short-term memory is the running context — the trajectory — bounded by the context window, so you summarize or trim as it grows. Long-term memory stores information externally, usually in a vector DB, and retrieves relevant pieces when needed — essentially RAG over the agent's own past. Memory management is the context-window keep/drop/summarize problem combined with retrieval, applied to the agent's state."*

The principle: **agent memory is short-term (the context-window-bounded trajectory, managed by summarization) plus long-term (external vector storage retrieved by relevance — RAG over the agent's past) — so it's the context-management and retrieval skills you already have, composed to make agents persistent and coherent over time.**
</details>

---

## Module 6.5 — Human-in-the-Loop: Keeping Control

**Difficulty:** ●●●○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Human-in-the-loop (HITL)**: designing the agent to pause and get human approval or input at key points (especially before consequential actions) rather than running fully autonomously.
- **Approval gate / checkpoint**: a point where the agent stops and waits for a human "yes" before proceeding (e.g. before sending an email or spending money).
- **Oversight**: monitoring what the agent is doing, with the ability to intervene.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agent: From Prototype to Production** → human-in-the-loop / safety / control sections. Key course for this.
- **AI Agents Fundamentals v2** → control patterns.

**Read:**
- **"Human in the loop AI agents"** — search `"human in the loop agent approval gate"` — *Focus on: when and how to require approval.*
- **Anthropic — Building effective agents (guardrails, human oversight)** — https://www.anthropic.com/research/building-effective-agents — *Focus on: keeping humans in control of consequential actions.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 6, Module 6.5 — human-in-the-loop. Autonomy is powerful but
risky; HITL keeps control. Gentle rhythm; decode HITL, approval gate, oversight.

PHASE 1 STUDY: Show me how to add human-in-the-loop checkpoints to an agent: pause before consequential
actions and require approval; let a human provide input or correct course; surface what the agent is about
to do for oversight. Show an agent that asks "I'm about to "X" — approve?" before doing something
consequential.

PHASE 2 TINKER: Have me add an approval gate before a destructive/consequential tool; deny an action and
watch the agent adapt; provide mid-task human input and see it incorporate it. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what HITL is and WHY it matters (autonomy + consequential
actions + imperfect/manipulable model = need human checkpoints), and where to place gates. Connect to tool
safety (5.7). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me add appropriate HITL checkpoints to an agent that takes real actions — hints
free.

PHASE 5 EXERCISE 2: Have me design HITL for a different agent solo (deciding which actions need approval),
then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Human-in-the-loop (HITL)** designs the agent to pause for human approval or input at key points — especially before **consequential actions** — rather than running fully autonomously. An **approval gate/checkpoint** is where the agent stops and waits for a human "yes" (before sending an email, spending money, deleting data); **oversight** is monitoring what the agent does with the ability to intervene.

**Why it's essential (connects to tool safety 5.7 and autonomy 6.1):** an agent is autonomous and the model is imperfect and manipulable (it can be wrong or prompt-injected). Full autonomy over consequential actions is dangerous — a single bad decision could send a wrong email to thousands or delete real data. HITL keeps humans in control of the actions that matter, while letting the agent autonomously handle the low-stakes steps. The art is placing gates well: gate the consequential/irreversible actions, let the agent run freely on safe ones (too many gates and it's not useful; too few and it's dangerous).

**The maturity signal:** knowing that real agents aren't fully autonomous — that production agents keep humans in the loop for consequential decisions — is exactly the kind of grounded, senior understanding that separates someone who's built real agents from someone who's seen demos.

The interview answer: *"How do you keep an autonomous agent safe?" → "Human-in-the-loop: require human approval at checkpoints before consequential or irreversible actions, while letting the agent run autonomously on low-stakes steps, with oversight to intervene. Because the agent is autonomous and the model can be wrong or manipulated, gating the actions that matter keeps the blast radius controlled. The skill is placing gates well — enough for safety, not so many it's useless."*

The principle: **human-in-the-loop keeps humans approving consequential or irreversible agent actions while the agent autonomously handles low-stakes steps — necessary because an autonomous, imperfect, manipulable model shouldn't have unchecked power over actions that matter, and well-placed gates balance safety with usefulness.**
</details>

---

## Module 6.6 — Multi-Agent Systems: Orchestration & Specialization

**Difficulty:** ●●●●● · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Multi-agent system**: multiple specialized agents working together, each handling part of a task, coordinated somehow — rather than one agent doing everything.
- **Orchestrator / coordinator**: an agent (or controller) that delegates sub-tasks to specialist agents and combines their results.
- **Specialist agent**: an agent focused on one job (research, coding, reviewing) with tools/prompts tuned for it.
- **Critic / reflection**: an agent (or step) that reviews another agent's output and suggests improvements.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agents Fundamentals v2** → multi-agent / orchestration sections.
- **AI Agent: From Prototype to Production** → coordinating multiple agents.

**Read:**
- **Anthropic — Building effective agents (orchestrator-workers, evaluator-optimizer)** — https://www.anthropic.com/research/building-effective-agents — *Focus on: these multi-agent patterns and when they're worth it.*
- **Anthropic — Multi-agent research system** — search `"anthropic multi-agent research system"` — *Focus on: a real multi-agent architecture.*
- **"Multi-agent vs single agent when"** — search — *Focus on: added power vs added complexity.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 6, Module 6.6 — multi-agent systems. Powerful but complex; I
should understand WHEN it's worth it. Gentle rhythm; decode multi-agent, orchestrator, specialist, critic.

PHASE 1 STUDY: Show me a couple of multi-agent patterns with small examples: (a) orchestrator + specialists
(a coordinator delegates sub-tasks to focused agents and combines results); (b) critic/reflection (one
agent produces, another reviews and improves). Show a task where splitting into specialists beats one
do-everything agent — and be honest about the added complexity.

PHASE 2 TINKER: Have me build a simple orchestrator-and-specialists setup; add a critic step and see output
improve; then try the SAME task with a single well-prompted agent and compare (sometimes simpler wins!).
Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the main multi-agent patterns and WHEN they're worth the
complexity vs a single agent (genuinely separable sub-tasks, distinct specializations) — and when they're
over-engineering. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a small multi-agent system for a task that genuinely benefits — hints
free.

PHASE 5 EXERCISE 2: Have me build a different multi-agent system solo AND justify why multi-agent beats
single-agent for it (or honestly conclude it doesn't), then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Multi-agent systems** use multiple specialized agents together instead of one agent doing everything. Key patterns: **orchestrator + specialists** (a **coordinator** delegates sub-tasks to focused **specialist agents** — research, coding, reviewing — and combines their results); and **critic/reflection** (one agent produces output, another reviews and improves it). Each specialist has tools and prompts tuned for its job.

**The crucial honesty (and senior judgment):** multi-agent adds real **complexity** — more moving parts, more cost, more coordination, more ways to fail. It's worth it when a task has *genuinely separable sub-tasks* or needs *distinct specializations*, or when a critic meaningfully improves quality. But often a single well-prompted agent (or even a simple workflow) is better — Anthropic's own guidance stresses using the *simplest* thing that works. Reaching for multi-agent by default is a classic over-engineering mistake. Knowing *when not to* is the senior signal.

**The theory:** specialization can improve quality (each agent focused, with a smaller, clearer job and context) and decomposition can handle complexity — but coordination overhead and compounding errors across agents are real costs. The win has to exceed the complexity.

The interview answer: *"When do you use multiple agents vs one?" → "Multi-agent — an orchestrator delegating to specialists, or a critic reviewing output — helps when a task has genuinely separable sub-tasks or needs distinct specializations, or when reflection improves quality. But it adds cost, coordination, and failure modes, so often a single well-prompted agent or a simple workflow is better. I use the simplest architecture that meets the bar, and only go multi-agent when the separability genuinely justifies the complexity."*

The principle: **multi-agent systems (orchestrator-specialists, critic-reflection) add power through specialization and decomposition but cost real complexity — so they're worth it only for genuinely separable or specialized tasks, and defaulting to them is over-engineering; the senior skill is using the simplest architecture that works.**
</details>

---
## Module 6.7 — When NOT to Use an Agent (the most senior lesson)

**Difficulty:** ●●●●○ · **Format:** STUDY → investigation · **Stack:** concepts + Python

### New words in this module

- **Workflow vs agent**: a *workflow* is a fixed, predefined sequence of LLM calls you control (predictable, reliable, cheap); an *agent* decides its own steps (flexible, but less predictable, costlier). Most "agent" problems are better solved by a workflow.
- **Over-engineering**: using a more complex/autonomous solution than the problem needs.
- **Determinism vs flexibility tradeoff**: agents trade predictability for flexibility — often a bad trade.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agent: From Prototype to Production** → the "when is an agent appropriate" / reliability sections.
- **AI Agents Fundamentals v2** → workflows vs agents.

**Read:**
- **Anthropic — Building effective agents** — https://www.anthropic.com/research/building-effective-agents — *Re-read the "when (and when not) to use agents" and workflows-vs-agents sections. This is THE source for this module.*
- **"Don't build agents when a workflow works"** — search `"workflow vs agent when to use simpler"` — *Focus on: prefer the simplest reliable approach.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 6, Module 6.7 — when NOT to use an agent. This is the most
senior lesson in the track; make me really get it. Gentle rhythm; decode workflow vs agent, over-engineering.

PHASE 1 STUDY: Explain the workflow-vs-agent distinction with examples: a fixed workflow (predefined steps
I control) is more predictable, reliable, debuggable, and cheaper; an agent (decides its own steps) is
flexible but less predictable and costlier. Show a problem people would reach for an agent on that a simple
workflow solves better. Teach me the decision: only use an agent when the task genuinely needs dynamic,
unpredictable decision-making that a fixed workflow can't handle.

PHASE 2 TINKER: Have me take a few problems and decide workflow vs agent for each, justifying; build the
same simple task as both and compare reliability/cost/predictability. Predict, then check reasoning.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the workflow-vs-agent tradeoff and a clear rule for when an
agent is actually warranted. Name the interview question and answer (this one impresses interviewers a LOT).

PHASE 4 EXERCISE 1: Have me redesign an "agent" someone over-built as a simpler reliable workflow — hints
free.

PHASE 5 EXERCISE 2: Have me solo decide, for a real problem, workflow vs agent, justify rigorously, and
build the right one, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The most senior lesson in agents: **most problems people reach for agents on are better solved by a workflow.** A **workflow** is a fixed, predefined sequence of LLM calls *you* control — predictable, reliable, debuggable, cheaper. An **agent** decides its own steps — flexible, but less predictable, harder to debug, and costlier. You should only use an agent when the task *genuinely* needs dynamic, open-ended decision-making that a fixed sequence can't handle.

**Why this is the senior signal:** the industry hype pushes "agents for everything," and inexperienced builders over-engineer autonomous agents for tasks a simple chained workflow would handle more reliably. Anthropic's own guidance is explicit: find the simplest solution, and only increase complexity (toward agents) when it demonstrably improves outcomes. An engineer who says "this doesn't need an agent — a three-step workflow is more reliable and cheaper" sounds far more senior than one who builds an elaborate agent for a predictable task.

**The tradeoff:** agents trade *predictability for flexibility*. For most business tasks, predictability and reliability are worth more — so the flexibility of an agent is often a bad trade. Reserve agents for genuinely open-ended tasks (research, complex multi-step problems where the path can't be predetermined).

The interview answer: *"When should you NOT use an agent?" → "Most of the time. If the task can be solved by a fixed, predefined workflow of LLM calls, do that — it's more predictable, reliable, debuggable, and cheaper. Agents trade predictability for flexibility, which is only worth it for genuinely open-ended tasks where the steps can't be predetermined. Defaulting to agents is over-engineering; the skill is using the simplest approach that works and escalating to an agent only when the task truly needs dynamic decision-making."*

The principle: **prefer a predictable workflow over an autonomous agent unless the task genuinely needs dynamic, unpredictable decision-making — because agents trade reliability and cost for flexibility, a trade that's usually bad, and knowing when NOT to use an agent is the most senior judgment in the field.**
</details>

---

## Module 6.8 — Making Agents Reliable: Production Concerns

**Difficulty:** ●●●●● · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **Agent reliability**: making an autonomous, multi-step system succeed consistently — much harder than a single call, because errors compound across steps.
- **Error compounding**: a small mistake early in a long agent run cascades into a wrong final result.
- **Loop limits / guardrails**: caps on steps/cost and checks that stop an agent from running forever or going off the rails.
- **Observability**: being able to see what the agent did (its trajectory, tool calls, costs) to debug and improve it.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Agent: From Prototype to Production** → THE course for this module — reliability, observability, production-readiness.

**Read:**
- **Anthropic — Building effective agents (reliability, guardrails)** — https://www.anthropic.com/research/building-effective-agents — *Focus on: keeping agents on track.*
- **"Making LLM agents reliable in production"** — search `"llm agent reliability production error handling observability"` — *Focus on: limits, monitoring, error recovery.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 6, Module 6.8 — making agents reliable. Autonomous + multi-step
= errors compound; production agents need guardrails and observability. Gentle rhythm but push hard on
reliability.

PHASE 1 STUDY: Show me the reliability toolkit for agents: loop/step limits and cost caps (so it can't run
forever or rack up huge bills), error recovery within the loop (5.5 applied across steps), validation of
intermediate results, and observability (logging the trajectory, tool calls, and costs so I can see what
happened). Demonstrate error compounding (a small early mistake wrecking the final result) and how guards
catch it.

PHASE 2 TINKER: Have me add step limits and a cost cap; break a tool mid-run and watch recovery vs failure;
add trajectory logging and use it to debug a bad run. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: why agents are harder to make reliable than single calls
(autonomy + compounding errors + non-determinism), and the production toolkit (limits, recovery,
validation, observability). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me make an agent production-reliable (limits, recovery, observability) and prove
it across many runs — hints free.

PHASE 5 EXERCISE 2 — 🐛 BUG HUNT, LEVEL 3 (full strength — you say nothing): Hand me an agent that
"behaves badly" on certain tasks and say NOTHING about what, where, how many, or even whether the flaws
are in the tools, the loop, the prompt, or the limits. My first job is to CHARACTERIZE the failure (using
the observability from this module — trajectories, logs, repeated runs) before hunting causes: what
exactly goes wrong, on which inputs, how often? Sincere production plants only (e.g. compounding early
error, a missing step limit interacting with a flaky tool, intermediate results never validated). Full
Prime Directive; hint ladder is my only lifeline. Fixes proven across MANY runs with eval cases (📏) —
an agent bug "fixed" on one run is not fixed. Then the verdict. This is the primer's hardest hunt so far;
that's the point.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Agent reliability** is much harder than single-call reliability because agents are autonomous and multi-step, so errors **compound** — a small early mistake cascades into a wrong final result, and the model's non-determinism (Track 1.5) means each step can wobble. The production toolkit: **loop/step limits and cost caps** (so the agent can't run forever or rack up huge bills), **error recovery within the loop** (5.5's error-as-result, applied across many steps), **validation of intermediate results** (catch a bad step before it compounds), and **observability** (log the trajectory, tool calls, and costs so you can see what the agent actually did and debug it).

**Why it's the hard part:** a demo agent works once on a happy path; a production agent runs thousands of times on varied inputs, where compounding errors, runaway loops, and surprise failures *will* happen. Guardrails and observability are what make an agent dependable enough to deploy. This is exactly the "prototype to production" gap your FM course is named after.

The interview answer: *"Why are agents hard to make reliable, and how do you do it?" → "Because they're autonomous and multi-step, errors compound — a small early mistake cascades — and the model is non-deterministic. So I add step and cost limits to bound runs, recover from tool failures within the loop, validate intermediate results to stop compounding, and add observability — logging the trajectory, tool calls, and costs — to debug and improve. Reliability comes from guardrails and visibility, not from trusting the agent to behave."*

The principle: **agent reliability is hard because autonomy plus multi-step execution makes errors compound, so production agents need step/cost limits, in-loop error recovery, intermediate validation, and observability — the guardrails and visibility that bridge the prototype-to-production gap.**
</details>

---

## Module 6.8A — 🔬 AUTOPSY: Read a Real Open-Source Agent Loop

**Difficulty:** ●●●●● · **Format:** 🔬 Autopsy (read, predict, compare) · **Stack:** reading real code (Python)

### New words in this module

- **Agent framework**: a library packaging the loop/tools/memory patterns you hand-built (e.g. Hugging Face's smolagents). You built from scratch precisely so frameworks would read as mechanism, not magic — this is where that pays off.

### Prep — Watch & Read

**Read (live, during the autopsy):**
- **Hugging Face `smolagents`** — https://github.com/huggingface/smolagents — *a deliberately small, readable, real agent framework; the core agent loop file is the target.*
- *(Or the closest current equivalent: any small, maintained, real open-source agent whose core loop fits in one sitting. NOT a sprawling framework — the loop must be readable in ~an hour.)*

### 🔬 AUTOPSY (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md — 🔬 AUTOPSY, full protocol. Track 6, Module 6.8A. I've built ReAct and
plan-and-execute loops from scratch (6.1–6.8); now I read a real framework's loop and prove to myself it's
the same mechanism — plus scars.

SETUP: Clone smolagents (or closest small equivalent) and locate the core agent loop. ONE file/subsystem,
~60 minutes.

THE AUTOPSY: (1) PREDICT the loop's structure BEFORE reading — from my own 6.2 ReAct build, I sketch what
their loop MUST contain (think/act/observe, stop conditions, error paths, memory handling), then read and
verify. Every gap out loud and logged. (2) COMPARE to my loop: where did they put the step limits, the
output parsing, the recovery? What did they centralize that I scattered? At least one difference judged:
better, or just different, and what production constraint drove it? (3) INTERROGATE the parts a framework
must solve that my toy skipped — how do they parse a malformed model action? What happens on step 47 of a
50-step limit? (4) EXTRACT one production trick into NOTES.md for my 6.9 capstone agent.

End with the realization this module exists for: make me articulate that the framework IS my loop plus
scars — that I can now read ANY agent framework as mechanism. Verdict: engineer or tourist?
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

The payoff of building from scratch: **frameworks stop being magic.** When you read a real agent framework after hand-building the loop, you recognize everything — the think/act/observe cycle, the stop conditions, the parsing of model output into actions — and what's left over is the interesting part: the scars. The defensive parsing, the recovery paths, the limits placed exactly where production runs go wrong. You can now evaluate any agent framework in an afternoon by reading its loop, which is a genuinely senior capability ("we should/shouldn't adopt X, and here's why, from its source").

The principle: **having built the loop yourself, every agent framework reads as your own architecture plus production scars — and the scars, not the architecture, are what the autopsy is for.**
</details>

---

## Module 6.9 — Agent Capstone: Build a Real Autonomous Agent

**Difficulty:** ●●●●● · **Format:** Guided capstone · **Stack:** Python + anthropic SDK + tools + vector DB

### New words in this module

- *(None new — integrates the track and much of the primer.)*

### Prep — Watch & Read

**Watch (Frontend Masters):**
- Revisit whichever of your three agent courses covered the pattern you'll use — this capstone is what they all build toward.

**Read:**
- Revisit your `NOTES.md` from 6.1–6.8 and Anthropic's **Building effective agents** once more.

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 6, Module 6.9 — agent capstone. Completed 6.1–6.8. Build a real,
reliable agent. I'm strong now; calibrate (ZPD) toward independence, rescue if stuck.

PHASE 1 STUDY (light): With me, CHOOSE the right design first (applying 6.7 — is an agent even warranted,
or a workflow?). Assuming an agent is justified, map the pieces: the loop (ReAct or plan-execute), good
tools, memory if needed, human-in-the-loop for consequential actions, and reliability (limits, recovery,
observability).

PHASE 2 BUILD: Guide me to build a genuinely useful autonomous agent for a real task I care about — e.g. a
research agent (searches, reads, synthesizes, cites), a coding/repo assistant, or a task automation agent.
Apply the right pattern, real tools, memory if needed, HITL gates, and full reliability. Let me drive;
hint when stuck. Make me first justify that an agent (not a workflow) is the right call.

PHASE 3 REPORT: I report. Make me justify the architecture (including why an agent over a workflow),
demonstrate it accomplishing a real multi-step goal, show the trajectory, and show it's reliable and safe.

PHASE 4 EXERCISE 1: Have me extend the agent with a new capability applying the same rigor — hints available.

PHASE 5 EXERCISE 2: Have me build a DIFFERENT real agent solo (or correctly decide a workflow is better and
build that), prove it across many runs with trajectory/outcome evals (📏), then the big verdict: am I
ready for Track 7 (Evaluation & Safety), or which agent concept to revisit? Flagship portfolio material.

PHASE 6 — 🖼️ UI REP (expert mode, from scratch): a minimal Next.js UI that renders the agent's LIVE
TRAJECTORY — each think/act/observe step appearing as it happens, tool calls and results visible, final
answer distinguished from intermediate reasoning. This is the most impressive UI rep yet: watching an
agent think is exactly the demo that makes non-AI people gasp, and almost no agent builders can make it.
Full CLAUDE.md expert rules on all frontend work; seam gentleness only for streaming step events out of
the Python loop. Minimal, honest, one page.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The capstone integrates the whole track — and much of the primer — into a real, reliable agent. Crucially, it starts with the senior question (6.7): *is an agent even the right call, or would a workflow be better?* Assuming an agent is justified, it combines: the right **loop** (ReAct 6.2 or plan-and-execute 6.3), good **tools** (Track 5), **memory** if needed (6.4 — which is RAG over the agent's past), **human-in-the-loop** gates for consequential actions (6.5), and **reliability** (6.8 — limits, recovery, observability). It's the synthesis of everything.

This is the flagship "hunted by recruiters" portfolio project — a real autonomous agent that accomplishes a genuine multi-step goal, built with production reliability and the judgment to know when an agent is warranted. With your frontend skills wrapping it in a great interface, "I built a reliable research/automation agent" is exactly the kind of project that signals rare, senior-level applied-AI capability.

The interview answer (whole track): *"How would you build a production agent?" → "First decide if an agent is even warranted versus a workflow. If it is: choose ReAct or plan-and-execute, give it well-defined tools, add short- and long-term memory if needed, gate consequential actions with human-in-the-loop, and make it reliable with step/cost limits, in-loop error recovery, intermediate validation, and observability. The agent reasons-acts-observes in a loop; my job is the tools, the guardrails, and knowing when a simpler workflow would be better."*

The principle: **a real autonomous agent integrates the right reasoning loop, solid tools, appropriate memory, human-in-the-loop safety, and production reliability — but the senior move is first deciding an agent is genuinely warranted over a workflow; build that, with your frontend polish, and you have a flagship portfolio project demonstrating rare applied-AI judgment.**
</details>

---

## 🧊 COLD REBUILD (do this before starting the next track — the honest gate)

**The task:** From an empty folder, hand-build a minimal ReAct agent: the think/act/observe loop over two tools, a step limit, trajectory logging, and one full run on a real multi-step task — then state, in two sentences, when this agent would be the WRONG choice.

**The rules (per AI-CONTRACT.md):** empty folder, ~20 minutes, NO notes, NO old code open. Paste this into Claude Code:

```text
Follow CLAUDE.md and AI-CONTRACT.md — this is a 🧊 COLD REBUILD. I'm cold-rebuilding Track 6's core: a minimal ReAct loop with step limit and trajectory logging, run on a real task, plus the when-NOT-to-agent judgment. Time me (~20 min, soft limit).
Full Prime Directive: no worked examples, hint ladder only. When I finish (or the time is clearly gone),
grade the rebuild honestly — what came out fluently, what I fumbled, what I couldn't produce at all — log
it as evidence in LEARNING-RECORD, and give the gate verdict: proceed to the next track, or which modules
to re-run first. Recognizing is not retrieval; only what I PRODUCED counts.
```

*Why this exists: you can pattern-match through a capstone with your own notes open. You cannot fake a cold rebuild. This 20 minutes is the most honest signal in the whole track.*

---

*End of Track 6 — Agents. 10 modules (including one 🔬 autopsy). You can build autonomous agents from scratch — ReAct, plan-and-execute, with memory, human-in-the-loop, multi-agent coordination, and production reliability — AND you have the senior judgment to know when an agent is the wrong tool. You've reached the frontier of applied AI, and you understand it as mechanism, not magic.*

*Next: Track 7 — Evaluation, Safety & Reliability, where you learn how to KNOW your AI systems actually work — the discipline that separates someone who ships demos from someone who ships products.*
