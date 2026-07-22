# TRACK 9 — MCP & MODERN AI TOOLING

*This track makes you fluent in the modern AI tooling ecosystem — the standard way to connect AI to the world (Model Context Protocol), and how to work as a genuinely effective professional in AI-powered dev tools (Cursor, Claude Code). MCP is rapidly becoming the universal connector between AI models and external tools/data, so understanding it deeply is increasingly expected. And mastering AI dev tools isn't just convenience — it's a multiplier on everything else you do. This track is where you go from "uses AI tools casually" (the baseline the blog post mentioned) to "uses them at a level most engineers don't."*

*Prerequisites: Tracks 0–8, especially Track 5 (tool use — MCP is a standard protocol for exactly that). Python + the anthropic SDK.*

*Spine courses (you own them): **Complete Intro to MCP** (the dedicated FM course — your main reference), **Cursor & Claude Code Professional AI Setup**, and **Maximilian Schwarzmüller — Claude Code: The Practical Guide**. PLUS Anthropic's excellent free **Introduction to Model Context Protocol** course (https://anthropic.skilljar.com/introduction-to-model-context-protocol) — build MCP servers/clients in Python from scratch — which pairs perfectly with this track.*

*Gentle rhythm: STUDY → TINKER → REPORT → EXERCISE 1 → EXERCISE 2.*

*🧊 Cold opens + track-end cold rebuild. 🔬 Autopsy in 9.4A: an official open-source MCP reference server — how the people who wrote the protocol write servers.*

---

## Module 9.1 — What MCP Is and Why It Exists

**Difficulty:** ●●●○○ · **Format:** STUDY → investigation · **Stack:** concepts + Python

### New words in this module

- **MCP (Model Context Protocol)**: an open standard for connecting AI models to external tools, data sources, and services in a consistent way — so any MCP-compatible model can use any MCP-compatible tool.
- **Protocol**: an agreed-upon standard for how two things communicate (like HTTP for the web). MCP is a protocol for model↔tool communication.
- **MCP server**: a program that exposes tools/data/prompts via MCP (e.g. a server that lets a model access GitHub, or a database, or your files).
- **MCP client**: the side (often the AI app) that connects to MCP servers and uses what they expose.

### Prep — Watch & Read

**Watch (Frontend Masters — you own this):**
- **Complete Intro to MCP** → the "what is MCP and why" foundation. Main reference for this whole track.

**Watch/Do (Anthropic free course):**
- **Introduction to Model Context Protocol** (https://anthropic.skilljar.com/introduction-to-model-context-protocol) → the intro sections — build MCP from scratch in Python. Excellent free companion.

**Read (gentle → deeper):**
- **MCP — Introduction** — https://modelcontextprotocol.io/introduction — *Focus on: what problem MCP solves (standardizing model↔tool connections).*
- **"What is MCP, explained"** — search `"model context protocol explained simply why"` — *Focus on: the "USB-C for AI tools" analogy — one standard connector.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 9, Module 9.1 — what MCP is and why. I understand tool use
(Track 5); MCP standardizes it. Gentle rhythm; decode MCP, protocol, server, client.

PHASE 1 STUDY: Explain MCP plainly: it's an open STANDARD (protocol) for connecting AI models to external
tools/data, so any MCP model can use any MCP tool — instead of every app reinventing custom tool
integrations. Use the "USB-C for AI" analogy (one standard connector). Connect to Track 5: I built custom
tool use; MCP is the standardized, reusable version. Explain MCP server (exposes tools/data) vs client
(the AI app that uses them). Show me the shape of a simple MCP interaction conceptually.

PHASE 2 TINKER: Have me explore an existing MCP server (many exist for GitHub, files, etc.) conceptually,
and map MCP concepts onto the custom tool use I built in Track 5 (what's the same, what MCP standardizes).
Predict-then-check.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what MCP is, the problem it solves (standardizing model↔tool
connections so tools are reusable across apps), and server vs client. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me articulate how a real integration I'd want (e.g. connect a model to a data
source) would work via MCP vs custom tool use — hints free.

PHASE 5 EXERCISE 2: Have me solo explain MCP's value for a different scenario and sketch the server/client
split, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**MCP (Model Context Protocol)** is an open **standard (protocol)** for connecting AI models to external tools, data sources, and services consistently — so any MCP-compatible model can use any MCP-compatible tool, instead of every app building custom one-off integrations. The common analogy: **"USB-C for AI"** — one standard connector instead of a different cable for everything. An **MCP server** exposes tools/data/prompts (e.g. a GitHub server, a filesystem server, a database server); an **MCP client** (usually the AI app, like Claude Desktop or Cursor) connects to servers and uses what they expose.

**The connection to Track 5:** you built *custom* tool use — defining tools and wiring them yourself. MCP is the **standardized, reusable** version of that: instead of every app reinventing a GitHub integration, someone builds one MCP GitHub server and *any* MCP client can use it. It's tool use as an interoperable ecosystem rather than bespoke per-app code.

**Why it matters now:** MCP is rapidly becoming the universal way to connect AI to the world, so understanding it is increasingly expected — and being able to build MCP servers/clients puts you ahead.

The interview answer: *"What is MCP and why does it matter?" → "An open protocol standardizing how AI models connect to external tools and data — 'USB-C for AI.' Instead of every app building custom tool integrations, an MCP server exposes tools/data once and any MCP client can use them. It's the standardized, interoperable version of function calling, so tools become reusable across apps, which is why it's becoming the universal connector between models and the world."*

The principle: **MCP is an open protocol that standardizes model↔tool/data connections (servers expose capabilities, clients consume them) — the interoperable, reusable evolution of the custom tool use you built in Track 5, and increasingly the universal way to connect AI to the world.**
</details>

---

## Module 9.2 — MCP's Core Primitives: Tools, Resources & Prompts

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + MCP SDK

### New words in this module

- **MCP primitives**: the three things an MCP server can expose — **tools** (functions the model can call), **resources** (data the model can read, like files or records), and **prompts** (reusable prompt templates the server provides).
- **Tools vs resources**: tools *do* things (actions); resources *provide* data (read-only context). The distinction mirrors Track 5's tool-use-vs-RAG idea.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **Complete Intro to MCP** → the core primitives sections (tools, resources, prompts).

**Do (Anthropic free):**
- **Introduction to Model Context Protocol** → the hands-on building of tools/resources/prompts in Python.

**Read:**
- **MCP — Core concepts (tools, resources, prompts)** — https://modelcontextprotocol.io/docs/concepts/architecture — *Focus on: the three primitives and what each is for.*
- **"MCP tools vs resources vs prompts"** — search `"mcp tools resources prompts difference"` — *Focus on: when to use each.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 9, Module 9.2 — MCP's three primitives. Gentle rhythm; decode
tools, resources, prompts (in the MCP sense).

PHASE 1 STUDY: Explain MCP's three primitives with examples: TOOLS (actions the model can invoke — like
Track 5 tools), RESOURCES (read-only data the model can access — like files/records, conceptually like
RAG context), and PROMPTS (reusable prompt templates the server provides). Show a tiny MCP server exposing
one of each, and explain when to use which. Map tools-vs-resources onto the actions-vs-knowledge idea from
Track 5.6.

PHASE 2 TINKER: Have me reason about a real integration and classify what should be a tool vs resource vs
prompt; tweak a simple server's primitives and see the effect. Predict-then-check.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the three primitives, the tools-vs-resources distinction
(actions vs data), and why MCP separates them. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me design (and start building) an MCP server exposing appropriate tools/resources/
prompts for a real use case — hints free.

PHASE 5 EXERCISE 2: Have me design the primitives for a different integration solo, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

MCP servers expose three **primitives**: **tools** (functions the model can call to *do* things — exactly like Track 5 tool use, now standardized), **resources** (read-only *data* the model can access — files, records, documents; conceptually like RAG context), and **prompts** (reusable prompt templates the server provides to clients). The **tools-vs-resources distinction** mirrors Track 5.6's actions-vs-knowledge idea: tools take *actions*, resources provide *data/context*.

**Why the separation matters:** it cleanly maps the two fundamental things a model needs from the outside world — the ability to *act* (tools) and the ability to *read* (resources) — plus reusable prompting (prompts). This structure is why MCP is a clean abstraction: it standardizes the three ways a model interacts with external systems. Understanding which primitive fits a given integration (an action → tool; read-only data → resource; a reusable template → prompt) is core MCP design skill.

The interview answer: *"What are MCP's core primitives?" → "Tools, resources, and prompts. Tools are functions the model can call to take actions — standardized function calling. Resources are read-only data the model can access, like files or records — context, conceptually like RAG. Prompts are reusable templates the server provides. The tools-vs-resources split mirrors actions-vs-knowledge: tools do, resources provide. Designing an MCP integration is mapping each need to the right primitive."*

The principle: **MCP exposes three primitives — tools (actions), resources (read-only data/context), and prompts (reusable templates) — cleanly standardizing the ways a model interacts with external systems, where tools-vs-resources mirrors the actions-vs-knowledge distinction from tool use and RAG.**
</details>

---

## Module 9.3 — Building an MCP Server From Scratch

**Difficulty:** ●●●●● · **Format:** STUDY → build · **Stack:** Python + MCP SDK

### New words in this module

- **MCP server implementation**: writing a server (in Python) that exposes your own tools/resources/prompts so any MCP client (Claude Desktop, Cursor, your app) can use them.
- **Transport**: how the client and server communicate (e.g. stdio for local, HTTP for remote).
- **Capability**: something your server offers (a specific tool, resource, or prompt).

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **Complete Intro to MCP** → the build-a-server sections. Follow along.

**Do (Anthropic free — perfect for this module):**
- **Introduction to Model Context Protocol** → the build-an-MCP-server-from-scratch-in-Python sections. This is the hands-on core.

**Read:**
- **MCP — Build a server (Python)** — https://modelcontextprotocol.io/quickstart/server — *Focus on: the minimal Python server, exposing a tool.*
- **MCP Python SDK** — search `"mcp python sdk github"` — *Focus on: the SDK for building servers.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 9, Module 9.3 — building an MCP server from scratch. Hands-on
and exciting. Gentle rhythm; decode server implementation, transport, capability.

PHASE 1 STUDY: Walk me through building a minimal MCP server in Python that exposes one useful tool (e.g.
something that queries data or performs an action). Explain the server structure, how it declares its
capabilities, and transport (stdio for local use). Show how a client (e.g. Claude Desktop or a test client)
would connect and use it. Build it WITH me.

PHASE 2 TINKER: Have me add a second tool and a resource to the server, test them, and connect the server
to a real MCP client to use my tools from there. Break something and read the error. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: how an MCP server works, what it exposes, and how a client uses
it — connecting to the tool-use mechanics from Track 5. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a useful MCP server for a real need of mine (exposing a tool/resource I'd
actually use) and connect it to a client — hints free.

PHASE 5 EXERCISE 2: Have me build a different MCP server solo, connect and use it, then the verdict. A
custom MCP server is impressive, current portfolio material — note it.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

You built an **MCP server** from scratch in Python — a program that exposes your own tools/resources/prompts so *any* MCP client (Claude Desktop, Cursor, your own app) can use them. You learned the server structure, how it declares its **capabilities** (the specific tools/resources/prompts it offers), and **transport** (stdio for local, HTTP for remote — how client and server talk).

**Why this is genuinely impressive:** MCP is current and rapidly growing, and most engineers haven't built an MCP server. Being able to say "I built a custom MCP server that exposes [useful capability] and connected it to Claude/Cursor" is impressive, modern portfolio material that signals you're at the frontier of the tooling ecosystem — not just using AI tools, but *extending* them. It also connects everything: an MCP server is the standardized, reusable version of the tool use you built in Track 5, now usable by any MCP client.

The interview answer: *"Have you built an MCP server?" → "Yes — a Python MCP server exposing custom tools and resources, connected to MCP clients like Claude Desktop. The server declares its capabilities and communicates over a transport like stdio or HTTP. It's the standardized, reusable form of function calling: instead of wiring tools into one app, the server exposes them once and any MCP client can use them, which is why MCP is becoming the universal connector."*

The principle: **building an MCP server means exposing your own tools/resources/prompts via the standard protocol so any MCP client can use them — the reusable, interoperable form of tool use — and a custom MCP server is impressive, current portfolio material that shows you extend AI tools, not just use them.**
</details>

---

## Module 9.4 — Using MCP: Connecting Claude to Real Tools & Data

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + MCP + clients

### New words in this module

- **MCP ecosystem**: the growing collection of ready-made MCP servers (for GitHub, Google Drive, databases, Slack, web search, etc.) you can plug into an MCP client.
- **Composability**: combining multiple MCP servers so a model can use many capabilities at once (read your files AND query a database AND search the web).

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **Complete Intro to MCP** → using/connecting servers, the ecosystem.
- **Cursor & Claude Code Professional AI Setup** → connecting MCP servers in real dev tools.

**Read:**
- **MCP — Example servers / connecting** — https://modelcontextprotocol.io/examples — *Focus on: the catalog of existing servers.*
- **"How to use MCP servers with Claude"** — search `"connect mcp server claude desktop cursor"` — *Focus on: plugging servers into clients.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 9, Module 9.4 — using MCP: connecting Claude to real tools/data.
Gentle rhythm; decode MCP ecosystem, composability.

PHASE 1 STUDY: Show me the ecosystem of ready-made MCP servers (GitHub, files, databases, search, etc.) and
how to connect one to an MCP client (e.g. Claude Desktop or Cursor) so the model can use real capabilities.
Show composing multiple servers so the model can do several things at once. Walk me through connecting one
real server and using it.

PHASE 2 TINKER: Have me connect a couple of MCP servers and use the model with real tools/data; combine two
servers in one session; notice how this extends what the AI can do without me writing integration code.
Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the value of the MCP ecosystem (reusable servers + composability)
and how it changes what's possible. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me connect real MCP servers to build a useful setup for my own work — hints free.

PHASE 5 EXERCISE 2: Have me compose a different MCP setup solo for a real task, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

There's a growing **MCP ecosystem** of ready-made servers (GitHub, Google Drive, databases, Slack, web search, filesystems, and more) you can plug into an MCP client without writing integration code. **Composability** means combining multiple servers so the model can use many capabilities at once — read your files AND query a database AND search the web in one session.

**Why this is powerful:** MCP turns "what can the AI do?" into "what servers can I connect?" — and because servers are reusable and composable, you can assemble powerful AI setups quickly. Connecting your dev tools (Cursor, Claude Code) to the right MCP servers is a genuine productivity multiplier, and understanding the ecosystem lets you reach for an existing server instead of building integrations from scratch. This is the practical payoff of MCP's standardization.

The interview answer: *"How do you extend what an AI assistant can do with MCP?" → "Connect MCP servers — ready-made ones for GitHub, databases, files, search, etc. — to an MCP client like Claude Desktop or Cursor, with no custom integration code. And compose multiple servers so the model can use many capabilities at once. MCP's standardization means capabilities are reusable and composable, so you assemble powerful setups by connecting servers rather than building each integration."*

The principle: **the MCP ecosystem provides reusable, composable servers (GitHub, databases, files, search) you connect to clients without writing integration code — turning 'what can the AI do?' into 'what servers can I connect?', a real productivity multiplier and the practical payoff of MCP's standardization.**
</details>

---

## Module 9.4A — 🔬 AUTOPSY: Read an Official MCP Reference Server

**Difficulty:** ●●●●○ · **Format:** 🔬 Autopsy (read, predict, compare) · **Stack:** reading real code (Python)

### New words in this module

- *(None new — the autopsy protocol applies.)*

### Prep — Watch & Read

**Read (live, during the autopsy):**
- **Official MCP reference servers** — https://github.com/modelcontextprotocol/servers — *servers written by the protocol's own authors. Target one small Python server (e.g. the fetch or git server) — how the people who designed MCP write MCP.*
- *(Closest-equivalent rule applies: any small, official-or-widely-used, real MCP server.)*

### 🔬 AUTOPSY (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md — 🔬 AUTOPSY, full protocol. Track 9, Module 9.4A. I built an MCP
server from scratch in 9.3; now I read one written by the protocol's authors.

SETUP: Clone the official MCP servers repo (or closest equivalent) and pick ONE small Python server
(fetch or git are good targets). ~45–60 minutes.

THE AUTOPSY: (1) PREDICT before reading: from my 9.3 build, sketch what their server MUST contain —
registration of tools/resources, the request handling, the error paths — then read and verify, gaps out
loud and logged. (2) COMPARE to my 9.3 server: their tool descriptions vs mine (descriptions steer the
model — whose steer better and why?), their input validation and safety boundaries vs mine, their error
responses vs mine. At least one difference judged: better or just different, and what constraint drove it.
(3) INTERROGATE the safety choices especially: what does an official server refuse to do, and where is
that refusal implemented? (4) EXTRACT one production trick into NOTES.md for my 9.6 capstone server.

Verdict: engineer or tourist?
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

Reference servers written by a protocol's authors are the closest thing to reading the designers' intent: how they name and describe tools (descriptions are prompts — they steer the model), where they draw safety boundaries, how they shape errors so a model can recover. Comparing your 9.3 server against theirs upgrades your capstone server from "works" to "idiomatic" — and "my MCP server follows the reference servers' conventions for X and Y" is a strong, current, checkable claim for a portfolio README.

The principle: **the authors' own servers encode the protocol's intended idioms — especially in tool descriptions and safety boundaries — and autopsying one turns your working server into an idiomatic one.**
</details>

---

## Module 9.5 — Professional AI-Assisted Development: Cursor & Claude Code Mastery

**Difficulty:** ●●●○○ · **Format:** STUDY → practice · **Stack:** Cursor / Claude Code

### New words in this module

- **AI-assisted development**: using AI dev tools (Cursor, Claude Code) as a serious productivity multiplier — not just autocomplete, but delegating real work effectively.
- **Context management (in dev tools)**: giving the AI the right files/context to work well (the context-window idea, Track 1.4, applied to coding).
- **Effective delegation**: knowing what to ask the AI to do, how to specify it, and how to review its output critically.

### Prep — Watch & Read

**Watch (Frontend Masters — you own these):**
- **Cursor & Claude Code Professional AI Setup** → the professional workflow sections. Main reference.
- **Maximilian Schwarzmüller — Claude Code: The Practical Guide** → practical Claude Code mastery.
- **Claude Code** (free FM course) → for the fundamentals if needed.

**Read:**
- **Anthropic — Claude Code best practices** — search `"claude code best practices anthropic"` — *Focus on: effective workflows.*
- **"Getting the most out of AI coding tools"** — search `"cursor claude code professional workflow tips"` — *Focus on: context, delegation, review.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 9, Module 9.5 — professional AI-assisted development. The blog
post said using these tools is baseline; I want to use them at a HIGH level. Gentle rhythm; this is
practice-oriented.

PHASE 1 STUDY: Teach me to use AI dev tools (Cursor/Claude Code) professionally, beyond autocomplete:
managing context (giving the AI the right files — Track 1.4 applied to code), delegating real tasks
effectively (clear specs), reviewing AI output critically (never trust blindly), and combining with MCP
servers (9.4). Show me the difference between casual and expert usage with examples.

PHASE 2 TINKER: Have me practice: delegate a real coding task with good context and a clear spec, critically
review the result, iterate; connect an MCP server to the tool and use it; notice what makes the AI more vs
less effective. Reflect on my own usage habits.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: what separates professional from casual AI-tool usage (context,
delegation, critical review) and why this is a multiplier. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me complete a real task using AI dev tools at a professional level and reflect on
what worked — hints free.

PHASE 5 EXERCISE 2: Have me tackle a different real task with professional AI-tool usage solo, then the
verdict on whether my workflow is genuinely effective.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**AI-assisted development** done well is a serious productivity multiplier, not just autocomplete. The skills that separate professional from casual usage: **context management** (giving the AI the right files and information — the context-window idea from Track 1.4 applied to coding; the AI works far better with the right context), **effective delegation** (clear specs for what you want, broken into reviewable pieces), **critical review** (never trusting AI output blindly — you're responsible for it, so you review like a senior reviewing a junior's PR), and **combining with MCP** (9.4) to give the tools more capabilities.

**Why it matters (the blog post's exact point):** using Cursor/Copilot is now *baseline*. What differentiates you is using them at a *high* level — and going one level deeper into how they work (MCP, context, the LLM mechanics you learned in Tracks 0-8). You now understand *why* these tools behave as they do (they're LLMs with context windows, doing next-token prediction over your code), which makes you far better at using them than someone who treats them as magic. This is also meta-relevant: you're using exactly these tools to work through this whole primer.

The interview answer: *"What separates professional from casual use of AI coding tools?" → "Context management — giving the tool the right files and information, since it's an LLM bounded by a context window. Effective delegation — clear specs broken into reviewable pieces. Critical review — never trusting output blindly, reviewing like a senior on a junior's PR. And extending the tools with MCP. Using these tools is baseline now; using them well — informed by how LLMs actually work — is the differentiator."*

The principle: **professional AI-assisted development is context management, clear delegation, critical review, and MCP extension — and because using AI coding tools is now baseline, the differentiator is using them at a high level, informed by understanding the LLM mechanics underneath, which turns them into a genuine multiplier.**
</details>

---

## Module 9.6 — MCP & Tooling Capstone: Build & Integrate Your Own MCP Server

**Difficulty:** ●●●●● · **Format:** Guided capstone · **Stack:** Python + MCP + clients

### New words in this module

- *(None new — integrates the track.)*

### Prep — Watch & Read

**Watch (Frontend Masters):**
- Revisit **Complete Intro to MCP** as the checklist; revisit **Cursor & Claude Code Professional AI Setup** for the integration.

**Do (Anthropic free):**
- Revisit **Introduction to Model Context Protocol** for any server-building details you need.

**Read:**
- Revisit your `NOTES.md` from 9.1–9.5.

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 9, Module 9.6 — MCP & tooling capstone. Completed 9.1–9.5. Build
something real and integrate it. Calibrate toward independence (ZPD).

PHASE 1 STUDY (light): Map the goal: build a custom MCP server exposing a genuinely useful capability (tool
+ resource), make it robust (apply Track 5 error handling + 7 safety basics), and integrate it into a real
client (Claude Desktop / Cursor / your own app) so you actually use it.

PHASE 2 BUILD: Guide me to build a useful custom MCP server for a real need of mine — exposing a tool and a
resource — with proper error handling and basic safety, then connect it to a client and use it in a real
workflow. Let me drive; hint when stuck.

PHASE 3 REPORT: I report. Make me explain the server's design, demonstrate it working through a real client,
and justify the tool/resource choices and safety measures.

PHASE 4 EXERCISE 1: Have me add a capability to the server with the same rigor — hints available.

PHASE 5 EXERCISE 2: Have me build a DIFFERENT useful MCP server solo and integrate it, then the big verdict:
am I ready for Track 10 (Capstones), or which MCP concept to revisit? A polished custom MCP server is
current, impressive portfolio material — note it.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The capstone integrates the track: build a real, useful custom **MCP server** (exposing a tool and a resource — 9.2, 9.3) with proper **error handling** (Track 5.5) and basic **safety** (Track 7), then **integrate** it into a real client (9.4) and use it in an actual workflow. You've gone from understanding MCP to *extending the AI tooling ecosystem with your own server* — and using it professionally (9.5).

This is current, impressive portfolio material. MCP is new and growing fast; a polished custom MCP server that does something genuinely useful, integrated into Claude Desktop or Cursor, demonstrates you're at the frontier of AI tooling. Combined with your frontend skills, you could even build a server exposing capabilities for a product you make. It signals exactly the "one level deeper than baseline tool usage" the blog post said differentiates candidates.

The interview answer (whole track): *"What's your experience with MCP?" → "I've built custom MCP servers in Python exposing tools and resources, with error handling and safety, and integrated them into clients like Claude Desktop and Cursor. I understand MCP as the standardized, reusable form of tool use — servers expose capabilities, clients consume them, composably — and I use AI dev tools professionally with good context management and critical review. It's the modern connective tissue between models and the world, and I can both use and extend it."*

The principle: **the capstone is building and integrating a real, robust, safe custom MCP server — extending the AI tooling ecosystem rather than just using it — which is current, frontier portfolio material that demonstrates the 'one level deeper than baseline' tooling fluency that differentiates candidates.**
</details>

---

## 🧊 COLD REBUILD (do this before starting the next track — the honest gate)

**The task:** From an empty folder, build a minimal MCP server exposing one tool and one resource, connect it to a real client, and demonstrate the tool being invoked through the protocol — narrating what MCP standardizes as you go.

**The rules (per AI-CONTRACT.md):** empty folder, ~20 minutes, NO notes, NO old code open. Paste this into Claude Code:

```text
Follow CLAUDE.md and AI-CONTRACT.md — this is a 🧊 COLD REBUILD. I'm cold-rebuilding Track 9's core: a minimal MCP server (one tool, one resource) connected to a real client and invoked through the protocol. Time me (~20 min, soft limit).
Full Prime Directive: no worked examples, hint ladder only. When I finish (or the time is clearly gone),
grade the rebuild honestly — what came out fluently, what I fumbled, what I couldn't produce at all — log
it as evidence in LEARNING-RECORD, and give the gate verdict: proceed to the next track, or which modules
to re-run first. Recognizing is not retrieval; only what I PRODUCED counts.
```

*Why this exists: you can pattern-match through a capstone with your own notes open. You cannot fake a cold rebuild. This 20 minutes is the most honest signal in the whole track.*

---

*End of Track 9 — MCP & Modern AI Tooling. 7 modules (including one 🔬 autopsy). You understand the Model Context Protocol deeply — its primitives, how to build servers from scratch, how to use the ecosystem — and you use AI dev tools (Cursor, Claude Code) at a professional level. You've gone from 'uses AI tools casually' to 'extends and masters them,' which is exactly the differentiator the market rewards.*

*Next: Track 10 — Capstones: Ship Real AI Products, where everything converges into 2–3 deployed, public, portfolio-grade projects that get you recruited — ideally with the beautiful frontends that are your unique edge.*
