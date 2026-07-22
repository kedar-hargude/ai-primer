# THE OPUS APPLIED AI PRIMER

*A problems-first path from "I use Claude because work gave me a subscription" to "I can architect, build, and ship any applied-AI system myself — and explain the theory underneath it well enough to impress any interviewer." Built for a frontend engineer who knows nothing about AI yet, who loves theory, and who wants to become the rare profile the market is starving for: a builder who ships AI products AND understands how they actually work.*

---

## WHAT THIS IS (AND ISN'T)

This teaches **applied AI engineering** — using LLMs (large language models) as components to build real systems: prompting reliably, embeddings, retrieval (RAG), tool use, agents, evaluation, and shipping to production. This is the skill that gets you hired for AI product work and makes you hard to replace.

This is **not** machine-learning research. It does not teach you to train models, derive backpropagation, or do the PhD mathematics. That's a different (and optional) mountain — see the note at the very end about a future "AI Internals" primer, if you fall in love and want the academic depth. Everything here lives in the band you asked for: **deep theory OF applied AI**, interview-grade "why," stopping short of training-mathematics.

**The deal that makes you exceptional:** most people who build a RAG app can't explain why their retrieval is bad. Most who use agents can't explain why the loop works or where it fails. You will be able to — because every module here goes one layer past "make it work" into "here's the mechanism, here's why it behaves this way, here's exactly what an interviewer would probe and how you'd answer." That depth is your differentiator, the same way explaining virtual-DOM-vs-real-DOM signalled that you actually understand React.

---

## HOW THIS WORKS (the format)

For each concept you will:

1. **Read the Prep** — primary sources (official docs, the foundational blog posts and papers, explained simply) that give you the mental model AND the theory underneath. Each module names what understanding it gives you.
2. **Do Exercise 1 (guided)** — paste the prompt into Claude Code. It scaffolds a real, broken or unbuilt system, refuses to hand you the answer, makes you build/fix and *understand* it, then runs a Socratic exam demanding the two-level explanation: the plain-language version AND the technical-mechanism version (the way you like to learn, and the way you win interviews).
3. **Do Exercise 2 (transfer)** — a *different* problem on the *same* concept, also in Claude Code, also fully verified by Claude — same build, same exam — but with hints withheld harder (you attempt solo and report first) and a mandatory end **verdict**: did this land (proceed), or redo Exercise 1? That verdict is the signal you can't get on your own.

The teaching behaviour (never hand over the answer, the hint ladder, `I give up`, `exam me`, proof-of-understanding) lives in the root `CLAUDE.md`. The AI-specific rules (always understand the mechanism, always handle the non-determinism, the two-level interview explanation) live in `AI-CONTRACT.md`. Both auto-load in Claude Code; you don't paste them.

**On top of the module rhythm, five standing disciplines run through the whole primer** (full rules in `AI-CONTRACT.md`):

1. **🧊 Cold opens & cold rebuilds** — from Track 1 on, every module opens with a ≤3-minute no-notes quiz on earlier material, and every track ends with a timed from-memory rebuild of its core mechanism. Knowledge you can't retrieve cold is knowledge you've lost; this is how the primer refuses to let that happen silently.
2. **📏 The evals thread** — born in Module 2.8 and never optional again: every artifact you build carries a tiny `evals/` folder (test cases + a runner reporting a success rate), growing as you do. No Solid verdict without a number. Track 7 formalizes what you'll already be living.
3. **🐛 Bug hunts** — from Track 4 on, some Exercise 2s are planted-failure hunts where the full never-reveal Prime Directive returns. Debugging a non-deterministic system nobody annotated for you is THE production skill; the hunts ramp from training wheels (failure count disclosed) to full "it's just behaving badly."
4. **🖼️ UI reps** — track capstones from Track 4 on get wrapped in a minimal from-scratch Next.js frontend. This compounds your frontend+AI edge continuously AND gives you the from-scratch project-setup reps you need. Crucially, the rules flip there: frontend is your expert domain, so no hand-holding.
5. **🔬 Autopsies** — dedicated modules where you dissect real open-source AI code (a real RAG implementation, a real agent loop, a real MCP server) with predict-before-you-read discipline. Toy builds teach the pattern; autopsies calibrate it against how it looks shipped.

These are what make the primer hard AND worthy: the module rhythm keeps the ramp gentle, and the five disciplines keep the outcome honest.

---

## SETUP (do this once)

- **Python** — all exercises are in Python (the industry default for AI, and what Arpit Bhayani's Applied AI Masterclass uses, so this prepares you for it). You know JS; the Python you need is small and you'll pick it up in Track 0. Install Python 3.11+ and learn `venv` (Track 0.1 walks you through it).
- **An API key** — you already have **Anthropic API access**, so the examples default to Claude via the `anthropic` Python SDK. The *concepts* are framed model-agnostically — every module notes "here's the Claude way, here's why it's the same pattern with OpenAI/Gemini" — so nothing locks you in and you'll recognise everything when Arpit's course uses Gemini.
- **One git repo** for the whole primer. At the root: `CLAUDE.md`, `AI-CONTRACT.md`, a `.gitignore` (ignore `.venv`, `__pycache__`, `.env`, and your API keys — NEVER commit a key). Each module gets its own folder with `exercise-1/` and `exercise-2/` subfolders.
- **A `NOTES.md` per module** — at the end of each, ask Claude to summarise what you built, what tripped you up, and the verdict. This is your durable learning log and your interview-prep cheat sheet.
- **An `evals/` folder per artifact (from Module 2.8 onward)** — a plain file of test cases plus a small runner script that prints a success rate. This is the evals thread; the convention is simply `evals/cases.json` + `evals/run.py` inside the module folder. Keep it tiny; keep it honest.
- **Node.js (you have it)** — the 🖼️ UI reps from Track 4 onward use from-scratch Next.js apps. Nothing to prepare; just know your existing frontend toolchain is part of this primer, not a separate world.

**Cost note:** running these exercises costs a few dollars of API usage total — trivial. Never put a real API key in code; use a `.env` file and `python-dotenv` (Track 0 shows you). Treat your key like a password.

---

## MODEL ADVICE (Opus vs Sonnet, as you asked for the perf primer)

- **Default to Sonnet 4.6** for scaffolding, early tracks, and single-concept work — faster, capable, cheaper.
- **Switch to Opus 4.8** (use `/model` in Claude Code) for the hard integration: the RAG and agent capstones, the evaluation track's judgment calls, anything where the tutor must hold many interacting parts together or reason deeply about a mechanism, and any Exercise 2 verdict you really care about.
- Rule of thumb: **Sonnet to learn a concept, Opus to integrate many or go deep on theory.**

---

## THE TRACK MAP

Do these in order — each builds on the last, and the capstones require everything. The ramp is deliberately gentle: a full Python-and-tools on-ramp comes FIRST, before any AI, so you're never thrown in cold.

- **Track 0 — Python & The Tools, for a JavaScript Developer** — the on-ramp. Python by analogy to JS, and what every piece of jargon actually means: terminal, SDK, package, pip, virtual environment, API, endpoint, `.env`, JSON, errors. Ends with you standing up a clean AI project from scratch. *(14 modules. Start here even though you'll want to rush to the AI — this is what makes the AI feel easy instead of terrifying.)*
- **Track 1 — Foundations: How LLMs Actually Work** — what the thing you're calling actually IS: next-token prediction, statelessness, tokens, context window, temperature/sampling, cost. The theory your theory-loving brain wants, now that you can run code to explore it.
- **Track 2 — Prompting as Engineering** — structured outputs, few-shot, chain-of-thought, system prompts, failure modes, and making a stochastic model behave reliably. Where your daily Claude use becomes engineering.
- **Track 3 — Embeddings & Vector Search** — the theory of meaning-as-vectors, why similar meanings sit near each other, similarity metrics, and building semantic search. The foundation of RAG.
- **Track 4 — Retrieval-Augmented Generation (RAG)** — chunking, retrieval, hybrid search, re-ranking, query rewriting, reducing hallucination, and *evaluating* retrieval. The single most in-demand applied-AI skill.
- **Track 5 — Tool Use & Function Calling** — letting the model call your code: schemas, the call loop, parallel calls, partial failures. The bridge from "chatbot" to "does things."
- **Track 6 — Agents** — the reasoning loops (ReAct, Plan-and-Execute, Observe-Think-Act), memory architecture, human-in-the-loop, multi-agent patterns, and *when an agent is the wrong tool*. The frontier and the best portfolio material.
- **Track 7 — Evaluation, Safety & Reliability** — how you *know* it works: eval sets, LLM-as-judge, adversarial testing, prompt injection, guardrails. What separates someone who demos from someone who ships.
- **Track 8 — Production: Cost, Caching, Streaming & Observability** — prompt/semantic caching, streaming, cost attribution, latency, monitoring. Making it real, cheap, and reliable.
- **Track 9 — MCP & Modern AI Tooling** — the Model Context Protocol (connecting AI to tools/data the standard way), plus working effectively in Cursor and Claude Code as a professional. Directly maps to your FM courses on these.
- **Track 10 — Capstones: Ship Real AI Products** — 2–3 deployed, public, portfolio-grade projects combining everything, ideally with the beautiful frontends that are your edge. This is what gets you recruited.

## HOW EACH MODULE RUNS (the gentle rhythm)

Because you're a beginner here (even though you're an experienced frontend dev), every module uses a scaffolded ramp — you're never asked to build something cold:

1. **STUDY** — Claude writes a complete, working, commented solution; you interrogate it line by line until it makes sense.
2. **TINKER** — you make small, guided modifications and observe what changes.
3. **REPORT** — you explain back what you learned, two-level (plain + technical).
4. **EXERCISE 1** — you build something similar, mostly yourself, with hints freely available.
5. **EXERCISE 2** — a different problem on the same concept, attempted more independently, verified by Claude with a Solid/Shaky/Not-yet verdict.

As you get stronger, the scaffolding tapers automatically. Early modules hold your hand; later ones let go. This is the Zone of Proximal Development — challenged but never overwhelmed.

## YOUR FRONTEND MASTERS COURSES (you own all of these — referenced throughout)

This primer cites your specific FM courses where they fit, so you always have a video reference:

- **Python for Professional Developers** → Track 0 (the Python language).
- **Claude Code** (free) & **Cursor & Claude Code Professional AI Setup** → Track 0 tooling + Track 9.
- **AI Engineering Fundamentals** → Tracks 1–2 (the practical backbone) and throughout.
- **Practical Prompt Engineering** → Track 2.
- **AI for Software Engineers** → framing throughout, esp. Tracks 1–2.
- **The Hard Parts of AI: Neural Networks** → optional deep-theory dips (Tracks 1, 3) for your theory appetite.
- **AI Agents Fundamentals v2**, **Build an AI Agent from Scratch v2**, **AI Agent: From Prototype to Production** → Tracks 5–6 (tool use & agents).
- **Complete Intro to MCP** → Track 9.
- **Maximilian Schwarzmüller — Claude Code: The Practical Guide** → Track 0/9 tooling depth.

---

## YOUR EDGE (read this when you feel behind)

You are a frontend engineer learning to build AI products. That intersection is rare: most frontend people fear the AI layer, most AI people can't build a good product, and within applied AI most builders can't explain the mechanism. You're aiming at all three at once. The blog posts warning that "most engineers, even senior ones, haven't built a RAG pipeline" are not telling you you're behind — they're telling you the bar is on the floor and almost nobody has started. You have a roadmap to the scarce skill. The work now is small and repeatable: one module at a time.

Start at Track 0, Module 0.1 — even though you'll be tempted to rush to the impressive agent stuff. The foundations are what let you *understand* the impressive stuff instead of cargo-culting it.

---

*This file is the orientation. Each track is its own file (`ai-track-0-foundations.md`, etc.), built in the same problems-first, deep-theory, two-exercise format.*
