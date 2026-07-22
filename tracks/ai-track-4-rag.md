# TRACK 4 — RETRIEVAL-AUGMENTED GENERATION (RAG)

*This is the big one — the single most in-demand applied-AI skill, and the thing the blog post said most engineers (even senior ones) have never built. RAG wraps an LLM around the semantic search you built in Track 3, so the model answers questions using YOUR documents — accurately, with sources, without hallucinating. By the end of this track you'll have built a real "chat with your documents" system from scratch, understand every piece deeply enough to explain it in an interview, and know exactly why RAG retrieval goes wrong and how to fix it (which is what separates you from people who just glue a library together).*

*Prerequisites: Tracks 0–3 — especially Track 3 (you built semantic search; RAG is that plus generation). Python + the anthropic SDK + a vector DB.*

*Spine courses (you own them): **AI Engineering Fundamentals** (RAG is a core module — your main video reference) and **AI for Software Engineers** (practical RAG framing). Anthropic's free **Building with the Claude API** course also covers retrieval patterns well.*

*Gentle rhythm: STUDY → TINKER → REPORT → EXERCISE 1 → EXERCISE 2. Jargon decoded inline. You've got real momentum now, so the scaffolding tapers — but hints stay generous.*

*🧊 Cold opens + track-end cold rebuild. 📏 Evals thread: every RAG build here carries retrieval-eval cases ("this question must retrieve that chunk") — they're how you'll PROVE your chunking/hybrid/re-ranking improvements instead of vibing them. 🐛 THE BUG HUNTS BEGIN in this track (Module 4.4, Level 1 — training wheels: you'll be told how many failures are planted). 🔬 One autopsy module (4.7A): you'll read a real, shipped RAG implementation. 🖼️ The capstone gets your first UI REP — a from-scratch Next.js frontend, where the rules flip to expert mode.*

---

## Module 4.1 — What RAG Is and Why It Exists

**Difficulty:** ●●○○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK + vector DB

### New words in this module

- **RAG (Retrieval-Augmented Generation)**: a pattern where you *retrieve* relevant documents (semantic search) and put them into the prompt so the model *generates* an answer grounded in those documents, instead of from its training memory alone.
- **Grounding**: making the model's answer based on provided source text rather than its (possibly wrong) internal knowledge.
- **Context injection**: putting the retrieved documents into the prompt as context for the model to use.

### Prep — Watch & Read

**Watch (Frontend Masters — you own these):**
- **AI Engineering Fundamentals** → the RAG sections. Your main reference for this track.
- **AI for Software Engineers** → how RAG fits into real applications.

**Read (gentle → deeper):**
- **"What is RAG, explained simply"** — search `"what is retrieval augmented generation explained simply"` — *Focus on: retrieve relevant text → put it in the prompt → model answers from it.*
- **Anthropic — Contextual retrieval / RAG patterns** — https://www.anthropic.com/news/contextual-retrieval — *Focus on: the retrieve-then-generate pattern and why grounding reduces hallucination.*
- **"Why RAG instead of fine-tuning"** — search `"rag vs fine tuning when to use"` — *Focus on: RAG adds knowledge without retraining; cheaper and updatable.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 4, Module 4.1 — what RAG is and why. I built semantic search in
Track 3; RAG adds the LLM on top. Gentle rhythm; decode RAG, grounding, context injection.

PHASE 1 STUDY: Write me a minimal but COMPLETE RAG script that reuses semantic search (from Track 3): given
a question, retrieve the most relevant documents from a vector DB, inject them into the prompt as context,
and have Claude answer USING that context. Demonstrate the payoff: ask a question the base model would get
wrong or refuse (about my specific documents), and watch RAG answer it correctly from the retrieved text.
Explain each stage: retrieve → inject → generate.

PHASE 2 TINKER: Have me ask questions that ARE in the documents (answered well) and questions that are NOT
(watch it should say "I don't know" if prompted well, or hallucinate if not — connecting to Track 1.1).
Have me remove the retrieval step and watch the answer quality collapse. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: (1) plain — what is RAG? (2) technical — WHY it works: it
changes what the model is CONDITIONED on (real source tokens), so it answers from provided truth rather
than fuzzy training memory — directly fighting hallucination (connect to Track 1.1). Name the interview
question and answer.

PHASE 4 EXERCISE 1: Have me build a basic RAG system over a small document set I care about and ask it
questions — hints free.

PHASE 5 EXERCISE 2: Have me build basic RAG over a DIFFERENT document set solo, demonstrate grounded
answers, then the verdict. This is the core loop everything else in the track refines.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**RAG (Retrieval-Augmented Generation)** is the pattern: **retrieve** relevant documents (the semantic search you built in Track 3), **inject** them into the prompt as context, and have the model **generate** an answer grounded in those documents. The three stages: retrieve → inject → generate.

**Why it works (the theory, connecting to Track 1):** recall hallucination is intrinsic — the model predicts *plausible* tokens from fuzzy training memory, with no ground truth. RAG fixes this by **changing what the model is conditioned on**: instead of relying on training memory, it conditions the answer on *real, retrieved source text* you put in the context. The model's job shifts from "recall and predict" to "read these sources and answer from them" — which is far more accurate and lets you cite sources. This is the single most important application of the "memory/knowledge is engineered via context" idea.

**Why RAG instead of fine-tuning:** RAG adds knowledge without retraining the model — it's cheaper, instantly updatable (change the documents, not the model), and works with private/changing data. That's why it's the dominant pattern for "answer questions about my data."

The interview answer: *"What is RAG and why does it reduce hallucination?" → "Retrieve relevant documents via semantic search, inject them into the prompt, and have the model generate an answer grounded in them. It reduces hallucination because it conditions the model on real retrieved source text instead of its fuzzy training memory — so the model answers from provided truth and can cite sources. It adds knowledge without fine-tuning, so it's cheap and updatable."*

The principle: **RAG retrieves relevant documents and injects them into the prompt so the model answers grounded in real sources rather than training memory — directly fighting hallucination by changing what the model is conditioned on, and adding knowledge without retraining.** You just built the most in-demand applied-AI system in its simplest form.
</details>

---

## Module 4.2 — Chunking: Splitting Documents the Right Way

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + vector DB

### New words in this module

- **Chunking**: splitting long documents into smaller pieces ("chunks") before embedding, because you can't embed a whole book as one vector and you want to retrieve just the relevant part.
- **Chunk size**: how big each piece is (in tokens/characters). Too big = imprecise retrieval + wasted context; too small = lost meaning/context.
- **Overlap**: letting chunks share some text at their edges so meaning isn't cut off at boundaries.
- **Semantic chunking**: splitting at meaningful boundaries (paragraphs, sections) rather than blindly every N characters.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → the document-processing / chunking sections.

**Read:**
- **"Chunking strategies for RAG"** — search `"rag chunking strategies chunk size overlap"` — *Focus on: size, overlap, and semantic boundaries.*
- **Anthropic — Contextual retrieval** — https://www.anthropic.com/news/contextual-retrieval — *Focus on: why naive chunking loses context and how to preserve it.*
- **"Why chunking matters for retrieval quality"** — search — *Focus on: chunking is a top cause of bad RAG.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 4, Module 4.2 — chunking. This is a top cause of good-or-bad
RAG and most people get it wrong. Gentle rhythm; decode chunking, chunk size, overlap, semantic chunking.

PHASE 1 STUDY: Explain why we chunk (can't embed a whole document well; want to retrieve the relevant
part). Write a script that takes a longer document, chunks it a few different ways (fixed-size; fixed-size
with overlap; by paragraph/semantic boundary), embeds each, and shows how retrieval quality differs for
the same questions. Make the tradeoffs VISIBLE.

PHASE 2 TINKER: Have me make chunks too big (imprecise, retrieves irrelevant bulk) and too small (loses
context, retrieves fragments), and find a sweet spot; add overlap and see a boundary-spanning answer
improve. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: how chunk size and overlap affect retrieval, why semantic
boundaries beat blind splitting, and the size tradeoff (precision vs context). Name the interview question
and answer.

PHASE 4 EXERCISE 1: Have me chunk a real document well (chosen size, overlap, sensible boundaries), embed
it, and show good retrieval — hints free.

PHASE 5 EXERCISE 2: Have me solo diagnose and fix a RAG system whose answers are bad BECAUSE of poor
chunking, on a new document, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Chunking** splits long documents into smaller pieces before embedding, because you can't usefully embed a whole document as one vector and you want to retrieve just the relevant passage. It's one of the biggest determinants of RAG quality, and a top cause of bad RAG.

**The tradeoffs:** **chunk size** too big → retrieval is imprecise (you pull in lots of irrelevant text, wasting context and diluting the answer) and the embedding is a fuzzy average of many ideas; too small → chunks lose the surrounding context that gives them meaning, and you retrieve fragments. **Overlap** (sharing some text between adjacent chunks) prevents meaning from being cut off at boundaries. **Semantic chunking** (splitting at paragraphs/sections rather than blindly every N characters) keeps coherent ideas together, which embeds and retrieves better.

**Why it impresses:** when someone's RAG gives bad answers, the cause is very often chunking — and most builders never suspect it. You'll be able to say "retrieval is poor because our chunks are too large and semantically mixed, so the embeddings average unrelated ideas" — a precise diagnosis.

The interview answer: *"How do you chunk documents for RAG, and why does it matter?" → "Split into pieces sized to balance precision and context — too big dilutes retrieval and averages multiple ideas into one fuzzy embedding; too small loses context. Add overlap so meaning isn't cut at boundaries, and prefer semantic boundaries (paragraphs/sections) over blind fixed splits. Chunking is one of the biggest drivers of retrieval quality and a common hidden cause of bad RAG."*

The principle: **chunking splits documents into retrievable pieces, and getting size/overlap/boundaries right is one of the largest levers on RAG quality — too big dilutes and averages meaning, too small loses context, and semantic boundaries with overlap preserve coherent, retrievable ideas.**
</details>

---

## Module 4.3 — The Full RAG Pipeline & Citing Sources

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK + vector DB

### New words in this module

- **Pipeline**: the full sequence — ingest documents → chunk → embed → store → (at query time) retrieve → inject → generate → return answer with sources.
- **Citations / source attribution**: returning *which* chunks the answer came from, so users can verify it. A hallmark of trustworthy RAG.
- **Top-k**: how many chunks you retrieve and inject (e.g. top 5 most similar).

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → the end-to-end RAG pipeline sections.

**Read:**
- **"Building a RAG pipeline end to end"** — search `"rag pipeline ingest chunk embed retrieve generate"` — *Focus on: the full flow.*
- **"RAG with citations"** — search `"rag citations source attribution trustworthy"` — *Focus on: returning sources with answers.*
- **Anthropic — Citations** — https://docs.claude.com/en/docs/build-with-claude/citations — *Focus on: getting Claude to cite the provided sources.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 4, Module 4.3 — the full RAG pipeline with citations. We
assemble everything into a real, trustworthy pipeline. Gentle rhythm; more independence (I've built the
pieces).

PHASE 1 STUDY: Write a complete RAG pipeline: ingest documents → chunk → embed → store in vector DB →
(query) retrieve top-k → inject with a good prompt → generate an answer THAT CITES which chunks it used.
Explain top-k and why citations make RAG trustworthy. Show the answer plus its sources.

PHASE 2 TINKER: Have me vary top-k (too few = missing info; too many = noise/cost), check that citations
actually match the source text, and test a question whose answer spans multiple chunks. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the full pipeline stages and why citations matter (trust +
verifiability + catching hallucination). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me build a full cited-RAG pipeline over a real document set and verify the
citations are accurate — hints free.

PHASE 5 EXERCISE 2: Have me build a full cited-RAG pipeline over a DIFFERENT corpus solo, verify
trustworthiness, then the verdict. This is a strong portfolio piece — note it.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The full **RAG pipeline**: ingest documents → **chunk** (4.2) → embed → store in a vector DB → at query time, retrieve **top-k** most similar chunks → inject them into a well-crafted prompt → generate an answer → return it **with citations** to the source chunks. Citations are what make RAG *trustworthy*: users can verify the answer against the cited source, and you can catch hallucination (if the answer isn't supported by the cited chunks, something's wrong).

**The top-k tradeoff:** too few chunks and you miss information the answer needs; too many and you add noise (irrelevant chunks dilute the answer and cost more tokens, and risk "lost in the middle" from Track 1.4). You tune k to retrieve enough relevant context without drowning it.

**Why citations matter so much:** a RAG system that answers without sources asks users to trust it blindly — and given hallucination, that's dangerous. Citations turn it into a verifiable system, which is the difference between a toy and something a business can rely on.

The interview answer: *"Walk me through a RAG pipeline." → "Ingest → chunk → embed → store in a vector DB. At query time: embed the query, retrieve the top-k most similar chunks, inject them into the prompt, generate an answer, and return it with citations to the source chunks. Top-k balances completeness against noise and cost, and citations make the answer verifiable — which is what makes RAG trustworthy rather than a confident guesser."*

The principle: **a full RAG pipeline is ingest→chunk→embed→store→retrieve-top-k→inject→generate-with-citations, where top-k is tuned to balance completeness against noise, and citations make the system trustworthy by letting users verify answers against real sources.** You've now built a genuinely portfolio-worthy system.
</details>

---

## Module 4.4 — Why Retrieval Fails: Diagnosing Bad RAG

**Difficulty:** ●●●●○ · **Format:** STUDY → investigation · **Stack:** Python + vector DB

### New words in this module

- **Retrieval failure**: the right chunk wasn't retrieved, so the model can't answer (or hallucinates). The #1 cause of bad RAG — and usually retrieval's fault, not the model's.
- **Query-document mismatch**: the user's question is phrased very differently from the source text, so embeddings don't match well.
- **Retrieval evaluation**: measuring whether retrieval actually fetched the right chunks (separately from whether the final answer was good).

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → any RAG debugging / evaluation sections.

**Read:**
- **"Why is my RAG not working"** — search `"why is my rag not working retrieval debugging"` — *Focus on: it's usually retrieval, not the LLM.*
- **"Evaluating RAG retrieval"** — search `"rag retrieval evaluation recall precision"` — *Focus on: measuring whether the right chunk was retrieved.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 4, Module 4.4 — diagnosing bad RAG. This is the skill that makes
me the person who can FIX RAG, not just build it. Gentle rhythm but push my diagnostic thinking.

PHASE 1 STUDY: Build me a RAG system that gives BAD answers, and walk me through diagnosing WHY — crucially,
separating "did retrieval fetch the right chunk?" from "did the model answer well given the chunks?" Show
me how to inspect what was actually retrieved. Most bad RAG is retrieval failure (wrong chunks fetched),
not the LLM. Demonstrate a query-document mismatch case.

PHASE 2 TINKER: Have me inspect retrieved chunks for failing questions and classify each failure: was the
right chunk even in the corpus? was it retrieved? was it chunked badly? was the query phrased too
differently? Predict, then verify by looking.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: the categories of RAG failure and how to systematically
diagnose them (inspect retrieval FIRST, then generation). Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me diagnose and fix a broken RAG system, identifying whether the fault is
retrieval or generation — hints free.

PHASE 5 EXERCISE 2 — 🐛 BUG HUNT, LEVEL 1 (my first real hunt — training wheels on): Scaffold a fresh
RAG system over a new small corpus with EXACTLY THREE planted, realistic retrieval failures (e.g. bad
chunking fragmenting an idea, a query-document mismatch, a top-k drowning problem — your choice, sincere
bugs only). Tell me the count is three, then SAY NOTHING ELSE — full Prime Directive: I observe symptoms,
form hypotheses, inspect retrieved chunks, and hunt. Hint ladder available and generous at this level.
For each bug I must classify the failure category (4.4's taxonomy), fix it, and PROVE the fix with my
retrieval evals (📏 the eval cases are how I demonstrate before/after). Then the verdict — and log this
in LEARNING-RECORD as my first hunt.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The most valuable RAG skill: **diagnosing why it gives bad answers.** The critical move is **separating retrieval from generation** — ask "did retrieval fetch the right chunks?" *before* "did the model answer well?" Because most bad RAG is a **retrieval failure** (the right chunk wasn't fetched), not the model's fault — if the relevant text never made it into the context, even a perfect model can't answer.

The failure categories you learn to classify: the answer isn't in the corpus at all; it's there but wasn't retrieved (embedding/similarity issue, or **query-document mismatch** where the question is phrased too differently from the source); it was chunked badly (4.2) so the relevant idea is fragmented or buried; too many irrelevant chunks drowned it (top-k too high). **Retrieval evaluation** means measuring whether retrieval fetched the right chunks, *separately* from final-answer quality.

**Why this is your differentiator:** the blog post's point exactly — most people glue a RAG library together, and when it gives bad answers they shrug or blame "the AI." You'll inspect the retrieved chunks, identify it's a retrieval failure from query-document mismatch, and fix it. That diagnostic ability is rare and exactly what makes you hireable.

The interview answer: *"Your RAG gives bad answers — how do you debug it?" → "First separate retrieval from generation: inspect what chunks were actually retrieved. Usually it's a retrieval failure — the right chunk wasn't fetched due to query-document mismatch, bad chunking, or wrong top-k — not the LLM. I evaluate retrieval quality independently of answer quality, fix the retrieval (chunking, query rewriting, re-ranking), and only then look at the generation prompt."*

The principle: **most bad RAG is a retrieval failure, not a model failure, so you diagnose by inspecting the retrieved chunks FIRST and evaluating retrieval separately from generation — which is the skill that lets you fix RAG instead of just building it, and the exact differentiator the market rewards.**
</details>

---

## Module 4.5 — Hybrid Search & Re-ranking: Better Retrieval

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + vector DB

### New words in this module

- **Hybrid search**: combining semantic search (embeddings, by meaning) with keyword search (exact terms, like BM25) — because each catches what the other misses (semantic gets paraphrase; keyword gets exact names/codes/jargon).
- **BM25 / keyword search**: classic term-matching search; great for exact identifiers semantic search can fumble.
- **Re-ranking**: after retrieving a candidate set, using a more powerful (slower) model to re-order them by true relevance, then keeping the best few. Improves precision.
- **Re-ranker model**: a model specialized for scoring query-document relevance more accurately than raw embedding similarity.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → advanced retrieval sections (hybrid, re-ranking).

**Read:**
- **"Hybrid search explained"** — search `"hybrid search semantic keyword bm25 rag"` — *Focus on: why combining beats either alone.*
- **"Re-ranking in RAG"** — search `"reranking rag cross encoder improve retrieval"` — *Focus on: retrieve broadly, then re-rank precisely.*
- **Anthropic — Contextual retrieval (rerank section)** — https://www.anthropic.com/news/contextual-retrieval — *Focus on: combining methods + reranking for big gains.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 4, Module 4.5 — hybrid search & re-ranking. These are the
"make retrieval genuinely good" upgrades that separate amateur from professional RAG. Gentle rhythm.

PHASE 1 STUDY: Show me, with worked examples, (a) hybrid search — combine semantic + keyword and show a
case where semantic alone misses an exact term (a product code, a name) but hybrid catches it; (b)
re-ranking — retrieve a broad candidate set, then re-rank with a more powerful relevance scorer and keep
the top few, improving precision. Explain BM25, hybrid, re-ranking, re-ranker model.

PHASE 2 TINKER: Have me find queries where semantic-only fails and hybrid fixes; where re-ranking promotes
the truly-best chunk above a superficially-similar one. Measure retrieval quality before/after.
Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: why hybrid beats either method alone, and why retrieve-broad-
then-rerank improves precision (cheap broad recall + expensive precise ranking). Name the interview
question and answer.

PHASE 4 EXERCISE 1: Have me add hybrid search and/or re-ranking to my RAG and prove the retrieval-quality
improvement — hints free.

PHASE 5 EXERCISE 2: Have me solo improve a weak-retrieval RAG using hybrid + re-ranking on a new corpus,
measure the gain, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

Two upgrades that make retrieval genuinely good. **Hybrid search** combines **semantic search** (embeddings — catches paraphrase and meaning) with **keyword search** (BM25 — catches exact terms: product codes, names, jargon, identifiers that embeddings can blur). Each catches what the other misses, so combining them beats either alone — semantic-only notoriously fumbles exact identifiers, and keyword-only misses paraphrase.

**Re-ranking** uses a two-stage approach: retrieve a *broad* candidate set cheaply (embeddings/hybrid, good recall), then **re-rank** those candidates with a more powerful, slower **re-ranker model** that scores true query-document relevance more accurately, and keep the top few. This improves *precision* — the best chunks rise to the top — without the cost of running the expensive scorer over the whole corpus.

**The theory:** embedding similarity is fast but approximate; it's good for casting a wide net (recall) but not always for fine-grained ranking (precision). Re-ranking adds a precise second pass only where it's cheap to do so (on the small candidate set). Hybrid covers the semantic-vs-exact blind spots. Together they're how professional RAG hits high retrieval quality.

The interview answer: *"How do you improve RAG retrieval quality?" → "Hybrid search — combine semantic (embeddings) with keyword (BM25) so you catch both paraphrase and exact terms like codes and names. And re-ranking — retrieve a broad candidate set cheaply for recall, then re-rank with a more powerful relevance model for precision and keep the top few. Together they fix the blind spots of embedding-only retrieval."*

The principle: **hybrid search combines semantic and keyword retrieval to cover each other's blind spots, and re-ranking adds a precise second-pass relevance scoring over a cheaply-retrieved broad candidate set — together delivering the high retrieval quality that distinguishes professional RAG from a naive embedding lookup.**
</details>

---

## Module 4.6 — Query Transformation: Fixing the Question Before You Search

**Difficulty:** ●●●●○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK + vector DB

### New words in this module

- **Query rewriting / transformation**: using an LLM to reshape the user's raw question into one (or several) better-for-retrieval queries before searching — fixing vague, ambiguous, or poorly-phrased questions.
- **Query expansion**: generating multiple variations of the query to retrieve more broadly, then merging results.
- **HyDE (Hypothetical Document Embeddings)**: a clever trick — have the LLM write a hypothetical *answer*, embed THAT, and search with it (since an answer is more similar to source documents than a question is).

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → query-processing / advanced RAG sections.

**Read:**
- **"Query transformation for RAG"** — search `"query rewriting expansion rag retrieval"` — *Focus on: improving the query before retrieval.*
- **"HyDE hypothetical document embeddings"** — search `"hyde hypothetical document embeddings explained"` — *Focus on: search with a hypothetical answer.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 4, Module 4.6 — query transformation. Fix the QUESTION before
searching — a powerful, underused upgrade. Gentle rhythm.

PHASE 1 STUDY: Show me, with worked examples: (a) query rewriting — an LLM reshapes a vague/ambiguous user
question into a cleaner retrieval query, improving results; (b) query expansion — generate several
variations and merge results; (c) HyDE — have the LLM write a hypothetical answer, embed THAT, and search
with it (an answer matches source docs better than a question does). Show each fixing a case raw retrieval
failed.

PHASE 2 TINKER: Have me find vague/awkward queries that retrieve poorly, then improve them via rewriting/
expansion/HyDE and watch retrieval improve. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: why transforming the query helps (it fixes query-document
mismatch from 4.4 — connecting back), and when each technique fits. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me add query rewriting (and/or HyDE) to my RAG and prove improved retrieval on
hard queries — hints free.

PHASE 5 EXERCISE 2: Have me solo apply a query-transformation technique to fix a poor-retrieval case on a
new corpus, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

**Query transformation** improves retrieval by fixing the *question* before you search. Techniques: **query rewriting** (an LLM reshapes a vague, ambiguous, or awkwardly-phrased question into a cleaner retrieval query); **query expansion** (generate several variations and merge their results for broader recall); and **HyDE** (have the LLM write a *hypothetical answer* to the question, embed that, and search with it — because a hypothetical answer is textually more similar to the actual source documents than the question is, so it retrieves better).

**The theory (connects to 4.4):** a major retrieval failure is **query-document mismatch** — users ask in ways that don't match how source text is written. Query transformation attacks this directly by reshaping the query to better match the document space. HyDE is especially elegant: questions and answers live in different "regions" of embedding space, so searching with a hypothetical answer lands closer to real answer-bearing documents.

**Why it's underused and impressive:** most RAG builders search with the raw user query and never think to improve it first. Knowing query transformation — and *why* it works (fixing query-document mismatch) — is another mark of someone who understands retrieval deeply.

The interview answer: *"How can you improve retrieval when users phrase questions poorly?" → "Transform the query before searching: rewrite it into a cleaner retrieval query, expand it into variations for broader recall, or use HyDE — generate a hypothetical answer and search with its embedding, since answers match source documents better than questions do. These fix query-document mismatch, a common retrieval failure."*

The principle: **query transformation (rewriting, expansion, HyDE) reshapes the user's question to better match the document space before retrieval — directly fixing query-document mismatch, one of the main retrieval failures — and it's a powerful, underused upgrade that signals deep retrieval understanding.**
</details>

---
## Module 4.7 — Advanced RAG Patterns: Multi-hop, Agentic & Contextual Retrieval

**Difficulty:** ●●●●● · **Format:** STUDY → build · **Stack:** Python + anthropic SDK + vector DB

### New words in this module

- **Multi-hop retrieval**: questions that need information from *multiple* documents combined in sequence — retrieve, reason, retrieve again based on what you found.
- **Agentic RAG**: letting the model *decide* what to retrieve, when to retrieve again, and when it has enough — instead of a fixed retrieve-once pipeline. (A bridge to Tracks 5–6.)
- **Contextual retrieval**: Anthropic's technique of prepending a short context blurb to each chunk before embedding, so chunks aren't stranded without their document context.
- **Metadata filtering**: narrowing retrieval by attributes (date, author, source) before/with the similarity search.

### Prep — Watch & Read

**Watch (Frontend Masters):**
- **AI Engineering Fundamentals** → advanced RAG sections.
- **AI for Software Engineers** → real-world RAG architectures.

**Read:**
- **Anthropic — Contextual Retrieval** — https://www.anthropic.com/news/contextual-retrieval — *Read fully. The contextual-chunk technique + reranking; a big quality win.*
- **"Multi-hop / agentic RAG"** — search `"multi-hop rag agentic retrieval iterative"` — *Focus on: retrieve→reason→retrieve loops for complex questions.*
- **"Metadata filtering in vector search"** — search `"vector database metadata filtering rag"` — *Focus on: combining filters with similarity.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 4, Module 4.7 — advanced RAG patterns. The professional-grade
upgrades. Gentle rhythm but I can handle more independence now.

PHASE 1 STUDY: Show me, with worked examples: (a) multi-hop — a question needing info combined from several
docs, handled by retrieve→reason→retrieve-again; (b) contextual retrieval — prepend a short context blurb
to each chunk before embedding so chunks keep their document context (Anthropic's technique); (c) metadata
filtering — narrow retrieval by attributes. Briefly preview agentic RAG (the model decides when/what to
retrieve — full agents in Tracks 5–6).

PHASE 2 TINKER: Have me build a multi-hop question that single-shot retrieval fails and the loop solves;
add contextual blurbs and see retrieval improve; add a metadata filter. Predict-then-run.

PHASE 3 REPORT: I report. Explain TWO-LEVEL: when single-shot RAG is insufficient and how multi-hop/
contextual/metadata each help. Name the interview question and answer.

PHASE 4 EXERCISE 1: Have me add one advanced pattern (contextual retrieval or multi-hop) to my RAG and
prove the improvement — hints free.

PHASE 5 EXERCISE 2: Have me solo apply an advanced pattern to a harder retrieval problem on a new corpus,
then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The patterns that take RAG from "works" to "professional." **Multi-hop retrieval** handles questions needing information combined across documents: retrieve → reason about what you found → retrieve again based on that → answer. **Contextual retrieval** (Anthropic's technique) prepends a short context blurb to each chunk *before embedding* (e.g. "This chunk is from the 2023 financial report, discussing Q3 revenue…"), so a chunk isn't stranded without its document context — a significant retrieval-quality win, especially combined with reranking (4.5). **Metadata filtering** narrows retrieval by attributes (date, author, source) alongside similarity. **Agentic RAG** (preview of Tracks 5–6) lets the *model* decide what and when to retrieve, rather than a fixed pipeline.

**The theory:** naive RAG is single-shot (retrieve once, answer). Real questions are often more complex — they span documents (multi-hop), depend on chunks that lost their context (contextual retrieval fixes this), or need scoping (metadata). These patterns relax the "retrieve once" constraint to match real question complexity.

The interview answer: *"When is basic retrieve-once RAG insufficient, and what do you do?" → "For questions spanning multiple documents, use multi-hop (retrieve, reason, retrieve again). When chunks lose their context, use contextual retrieval — prepend a context blurb before embedding. Use metadata filtering to scope retrieval, and agentic RAG when the model should decide what and when to retrieve. These match retrieval to the real complexity of the question instead of assuming one retrieval suffices."*

The principle: **advanced RAG relaxes the single-shot assumption — multi-hop for cross-document questions, contextual retrieval to keep chunks grounded in their document, metadata filtering for scope, and agentic retrieval to let the model drive — matching retrieval to real question complexity.**
</details>

---

## Module 4.7A — 🔬 AUTOPSY: Read a Real, Shipped RAG Implementation

**Difficulty:** ●●●●○ · **Format:** 🔬 Autopsy (read, predict, compare) · **Stack:** reading real code (Python)

### New words in this module

- **Autopsy**: dissecting real, shipped, open-source code with predict-before-you-read discipline — the reading half of engineering that toy builds can't teach.
- **Reference implementation**: code published by the people who designed a technique, showing how THEY build it.

### Prep — Watch & Read

**Read (this IS the module — the reading happens live, in the autopsy):**
- **Anthropic Cookbook — the RAG / retrieval recipes** — https://github.com/anthropics/anthropic-cookbook — *the primary corpse: real, maintained, shipped reference code for the exact patterns you just built. The contextual-retrieval/RAG notebooks are the target.*
- *(If the repo has reorganized, per AI-CONTRACT.md: find the current closest equivalent — a small, real, maintained open-source RAG implementation. The protocol matters, not the exact repo.)*

### 🔬 AUTOPSY (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md — this is a 🔬 AUTOPSY module; run the full autopsy protocol. Track 4,
Module 4.7A. I've built RAG from scratch across 4.1–4.7; now I calibrate my toy against shipped code.

SETUP: Help me clone the Anthropic Cookbook and locate its RAG/retrieval implementation (or the closest
current equivalent). Choose ONE focused implementation — ingestion + retrieval + generation — sized for
~45–60 minutes of reading.

THE AUTOPSY, per the protocol:
1. PREDICT BEFORE READING — for each function/section, before I open it, ask me to predict from its name
   and call site what it does and how. Then I read and verify. Make me say every prediction-vs-reality gap
   OUT LOUD; those gaps are the yield. Log notable ones in LEARNING-RECORD.
2. COMPARE TO MY BUILD — for each significant design difference from my 4.1–4.7 code, make me answer: is
   theirs better or just different? What constraint were they handling that my toy ignored (scale? cost?
   messy real documents? caching?)? At least one difference must be named and defended.
3. INTERROGATE THE CHOICES — Socratically probe: why THIS chunking? why THIS prompt structure for
   injection? what does their error handling assume that mine didn't?
4. EXTRACT ONE PRODUCTION TRICK — I end by stating ONE concrete technique this code uses that my build
   didn't, and where in my capstone I'll apply it. It goes in NOTES.md verbatim.

Close with a short verdict: did I read this code like an engineer (predicting, comparing, extracting) or
like a tourist (nodding along)? Be honest.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

Building teaches you the pattern; **reading shipped code calibrates it.** Your toy RAG made simplifying assumptions you didn't know you were making — real implementations reveal them: they handle messy documents, cache embeddings, batch API calls, structure injection prompts defensively, and fail gracefully in places your toy simply crashed. The **predict-before-reading** discipline is what turns reading into learning: every gap between what you predicted and what the code actually does is a precise, personal lesson no tutorial could target.

This is also a direct interview and job skill: engineers spend as much time reading AI code as writing it, and "I've read how the reference implementations do X, and here's a choice they made that surprised me" is production-scar currency.

The principle: **toy builds teach the mechanism, autopsies of shipped code teach the constraints — and predict-before-reading turns someone else's code into a personalized curriculum of exactly your blind spots.**
</details>

---

## Module 4.8 — RAG Capstone: "Chat With Your Documents"

**Difficulty:** ●●●●● · **Format:** Guided capstone · **Stack:** Python + anthropic SDK + vector DB

### New words in this module

- *(None new — integrates the whole track.)*

### Prep — Watch & Read

**Watch (Frontend Masters):**
- Revisit the **AI Engineering Fundamentals** RAG arc as a checklist.

**Read:**
- Revisit your `NOTES.md` from 4.1–4.7.
- **"Production RAG checklist"** — search `"production rag best practices checklist"` — *Focus on: the end-to-end quality levers.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 4, Module 4.8 — RAG capstone. Completed 4.1–4.7. Integrate the
whole track into a real product. I'm strong here now; calibrate (ZPD) toward independence, rescue if stuck.

PHASE 1 STUDY (light): Map the full quality-grade RAG pipeline as a checklist: ingest → smart chunking →
(contextual) embed → vector DB → hybrid retrieve → re-rank → query transform → inject → generate with
citations → handle "not in docs" gracefully → evaluate retrieval + answer quality.

PHASE 2 BUILD: Guide me to build a real "chat with your documents" app over a corpus I care about (my
notes, a set of PDFs, docs — my choice): the full pipeline with good chunking, citations, graceful "I
don't know," and at least one quality upgrade (hybrid/re-rank/contextual/query-transform). Multi-turn chat
that remembers context (Track 1.2). Let me drive; hint where I stall.

PHASE 3 REPORT: I report. Make me justify each design choice, demonstrate grounded cited answers, show it
gracefully handles out-of-scope questions, and HONESTLY evaluate where retrieval/answers are weak and why.

PHASE 4 EXERCISE 1: Have me improve the system's weakest area (diagnosed via Track 4.4 method) and prove
the gain — hints available.

PHASE 5 EXERCISE 2: Have me build a "chat with your documents" system over a DIFFERENT corpus solo, end to
end, run my retrieval + answer evals (📏) and report the numbers, then the big verdict: am I ready for
Track 5 (Tool Use), or which RAG concept to revisit? This is a flagship portfolio project — note it.

PHASE 6 — 🖼️ UI REP (my first — and THE RULES FLIP): wrap the capstone system in a minimal Next.js
frontend: one page, an ask box, the streamed-or-plain answer, and clickable citations that reveal the
source chunk. CRITICAL MODE SWITCH per AI-CONTRACT.md: frontend is my EXPERT domain — I set up the
Next.js project from scratch MYSELF (do not scaffold it for me, do not hand me components; this from-
scratch setup rep is half the point), and you review my frontend code like a rigorous senior with the
full Prime Directive. The ONE seam where normal AI-primer gentleness still applies: connecting the
frontend to my Python pipeline (the API route, the request/response shape) — that seam is new; the React
is not. Keep it minimal and honest — polish lives in Track 10.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish or give up</summary>

The capstone integrates everything into a real "chat with your documents" product — the flagship applied-AI portfolio piece. The quality-grade pipeline: ingest → smart **chunking** (4.2) → **contextual** embedding (4.7) → vector DB (3.4) → **hybrid** retrieval + **re-ranking** (4.5) → **query transformation** (4.6) → inject → generate **with citations** (4.3) → gracefully handle out-of-scope ("I don't know" rather than hallucinate) → **evaluate** retrieval and answer quality (4.4) → multi-turn chat that remembers context (1.2).

This is the system the blog post said most engineers can't build — and you can now build it *and diagnose and improve it*, which is even rarer. With a nice frontend (your edge), "I built a cited, evaluated RAG system you can chat with over [domain]" is exactly the project that gets recruiter attention.

The interview answer (whole track): *"Build me a document Q&A system." → "Full RAG pipeline: chunk well, embed (contextually) into a vector DB, retrieve with hybrid search + re-ranking, transform the query when needed, inject top-k, generate with citations, refuse gracefully when the answer isn't in the docs, and evaluate retrieval separately from generation. The hard parts are retrieval quality — chunking, hybrid+rerank, query transformation — and trustworthiness via citations and graceful refusal."*

The principle: **a real 'chat with your documents' system is the full quality-grade RAG pipeline — smart chunking, contextual embedding, hybrid+reranked retrieval, query transformation, cited generation, graceful refusal, and separate retrieval/answer evaluation — and building AND diagnosing it is the single most marketable applied-AI skill you can demonstrate.** Add your frontend polish and it's a flagship portfolio piece.
</details>

---

## 🧊 COLD REBUILD (do this before starting the next track — the honest gate)

**The task:** From an empty folder, build a minimal but complete RAG pipeline over a few documents: chunk → embed → store → retrieve → inject → generate a grounded answer with a citation, handling one "not in the docs" question gracefully — with retrieval eval cases run and reported.

**The rules (per AI-CONTRACT.md):** empty folder, ~20 minutes, NO notes, NO old code open. Paste this into Claude Code:

```text
Follow CLAUDE.md and AI-CONTRACT.md — this is a 🧊 COLD REBUILD. I'm cold-rebuilding Track 4's core: a minimal complete RAG pipeline (chunk, embed, retrieve, inject, generate with citation, graceful 'not in docs'), evals reported. Time me (~20 min, soft limit).
Full Prime Directive: no worked examples, hint ladder only. When I finish (or the time is clearly gone),
grade the rebuild honestly — what came out fluently, what I fumbled, what I couldn't produce at all — log
it as evidence in LEARNING-RECORD, and give the gate verdict: proceed to the next track, or which modules
to re-run first. Recognizing is not retrieval; only what I PRODUCED counts.
```

*Why this exists: you can pattern-match through a capstone with your own notes open. You cannot fake a cold rebuild. This 20 minutes is the most honest signal in the whole track.*

---

*End of Track 4 — Retrieval-Augmented Generation. 9 modules (including one 🔬 autopsy). You can build a real, trustworthy, cited RAG system from scratch, diagnose why retrieval fails, and apply professional upgrades (hybrid search, re-ranking, query transformation, contextual retrieval). You've built the most in-demand applied-AI system — and you understand every piece deeply enough to fix it and explain it.*

*Next: Track 5 — Tool Use & Function Calling, where the model stops just talking and starts DOING — calling your code, fetching live data, taking actions. The bridge from chatbot to agent.*
