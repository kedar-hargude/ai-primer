# TRACK 0 — PYTHON & THE TOOLS, FOR A JAVASCRIPT DEVELOPER

*The on-ramp. Before any AI, you need to be comfortable in Python and understand the tools and jargon everyone throws around (SDK, environment, package, API, endpoint, terminal). This track assumes you know JavaScript well and know nothing about Python — so it teaches Python BY analogy to what you already know, one tiny step at a time. Nothing here is hard; it's just new vocabulary for ideas you mostly already have. By the end you'll read Python comfortably, run scripts, manage a project, keep secrets safe, and know what every piece of jargon means. THEN the AI begins, and you'll be ready.*

*Prerequisites: you can use a computer and you know JavaScript. That's it. No Python, no terminal experience assumed.*

*This track maps directly to two Frontend Masters courses you own: **Python for Professional Developers** (the live workshop — your main reference for the Python language here) and the free **Claude Code** course (for getting comfortable in the tool you'll learn in). Watch alongside; this track tells you exactly which parts matter.*

*How each module runs (from AI-CONTRACT.md): **STUDY** (Claude writes a worked example, you interrogate every line) → **TINKER** (you modify and experiment) → **REPORT** (you explain back) → **EXERCISE 1** (you build, with generous hints) → **EXERCISE 2** (transfer, verified, with a verdict). You are never asked to build something cold. Maximum scaffolding here; it tapers later.*

---

## Module 0.1 — What Even Is the Terminal? (And Why Developers Live In It)

**Difficulty:** ●○○○○ · **Format:** Guided hands-on · **Stack:** Your computer's terminal

### New words in this module (plain definitions first)

- **Terminal** (a.k.a. command line, shell): a text window where you type commands to your computer instead of clicking buttons. Like the browser console, but for your whole computer instead of one webpage.
- **Command**: a typed instruction, e.g. `cd Documents` means "change directory into the Documents folder."
- **Directory**: just another word for "folder."
- **`cd`**: "change directory" — move into a folder. **`ls`** (Mac/Linux) / **`dir`** (Windows): "list" — show what's in the current folder. **`pwd`**: "print working directory" — show which folder I'm currently in.

### Prep — Watch & Read Before You Start

**Watch (Frontend Masters — you own these):**

- **Claude Code** (the free FrontendMasters course) → the early setup/terminal sections. Even though it's about Claude Code, it gets you comfortable opening and using a terminal, which is the whole point here.
- **Python for Professional Developers** → the environment-setup intro (where the instructor opens a terminal and runs things) — watch how they move around.

**Read (gentle, surrounding context):**

- **MDN — "Command line crash course"** — https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Environment_setup/Command_line — *Focus on: `cd`, `ls`/`dir`, `pwd`, and that the terminal is just typing instead of clicking. You already half-know this from npm.*
- **"Terminal for beginners" explainer** — search `"terminal command line beginner cd ls pwd"` — *Focus on: navigating folders.*

**The friendly framing:** You've actually used a terminal already — every time you ran `npm install` or `npm run dev`, that was the terminal. So this isn't new; we're just getting deliberate about it.

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.1 — the terminal. I'm a frontend dev who has
run `npm install` but never really understood the terminal. Treat me as a total beginner here and use
the gentle module rhythm (Study → Tinker → Report → Exercise 1 → Exercise 2). Decode every term.

PHASE 1 STUDY: Show me, with explanation, the handful of terminal commands I'll actually use:
navigating folders (cd, ls/dir, pwd), making a folder (mkdir), and where I "am" at any moment. Explain
each like I've never seen it, with a JS/npm analogy where one fits. Let me ask about anything confusing.

PHASE 2 TINKER: Give me a tiny guided tour to do myself — make a folder for this whole AI primer,
cd into it, make a subfolder, list contents, check where I am. Have me predict what each command will
do before I run it.

PHASE 3 REPORT: I'll tell you what happened. Check I understand "where am I in the folder tree" since
that confusion bit me before (which folder does my tool act on?).

PHASE 4 EXERCISE 1: Have me create the folder structure for Track 0 (a folder per module) myself,
using only the terminal, with hints freely available.

PHASE 5 EXERCISE 2: Have me navigate around and create one more module's structure mostly solo, then
give me the Solid/Shaky/Not-yet verdict on whether I'm comfortable moving around the terminal.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

The terminal is just typing commands instead of clicking. The only commands you need to start: `cd` (enter a folder), `ls`/`dir` (see what's in this folder), `pwd` (which folder am I in?), `mkdir` (make a folder). You already used the terminal for npm — this just makes you deliberate about it.

The one concept that matters most, and that bit you earlier with Claude Code: **at every moment, the terminal is "in" exactly one folder (your working directory), and commands act on THAT folder.** `cd` changes which folder you're in; `pwd` tells you. This is the same "which folder will my tool write to?" question you worried about before — and now you can always answer it with `pwd`. That clarity is the whole win of this module.

The principle: **the terminal is a text interface to your computer where you're always located in one working directory, and a few navigation commands (`cd`/`ls`/`pwd`/`mkdir`) are all you need to move around with confidence.**

</details>

---

## Module 0.2 — Installing Python & Running Your First Line of Code

**Difficulty:** ●○○○○ · **Format:** Guided hands-on · **Stack:** Python

### New words in this module

- **Python**: a programming language, like JavaScript but used heavily for AI, data, and scripting. Different syntax, same core ideas (variables, functions, loops).
- **Install**: download a program onto your computer so you can use it. Installing Python is like installing Node.js — it lets your computer run that language's code.
- **Interpreter / REPL**: a mode where you type one line of code and it runs immediately and shows the result. Exactly like typing JavaScript into the browser console. Python's is started by typing `python` in the terminal.
- **Script**: a file full of code (ending in `.py`) that you run all at once, versus typing line by line. Like a `.js` file you run with `node`.

### Prep — Watch & Read Before You Start

**Watch (Frontend Masters):**

- **Python for Professional Developers** → the installation and "first program" sections. This is your primary guide for installing Python correctly on your OS — follow along with the instructor.

**Read:**

- **Official Python — "Setup and Usage" / downloads** — https://www.python.org/downloads/ and https://docs.python.org/3/using/index.html — *Focus on: installing Python 3.11 or newer for your operating system.*
- **Real Python — "Python REPL"** — search `"real python interactive interpreter REPL"` — *Focus on: typing code interactively, like the browser console.*
- **"Python vs JavaScript for beginners"** — search `"python vs javascript syntax differences beginners"` — *Focus on: the surface differences (indentation, no semicolons, print vs console.log). Reassuring, not scary.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.2 — installing Python and running code. Total
beginner to Python; I know JS. Gentle rhythm, decode everything, JS analogies.

PHASE 1 STUDY: First, confirm with me how to check whether Python is installed (a terminal command),
and if not, walk me through installing Python 3.11+ for my OS (ask which OS I'm on). Then SHOW me two
ways to run Python: (a) the interactive interpreter/REPL (like the browser console) where I type
`print("hello")` and see it instantly, and (b) a `.py` script file I run from the terminal. Explain
`print()` as Python's `console.log()`. Explain what a `.py` file is (like a `.js` file).

PHASE 2 TINKER: Have me try the REPL — do some arithmetic, print a string, make a variable (note: no
`const`/`let` in Python, just `name = "value"`). Then have me put the same code in a `.py` file and run
it from the terminal. Make me predict outputs first.

PHASE 3 REPORT: I'll report what I saw. Check I understand the difference between REPL (line by line,
instant) and a script (a file run all at once), with the JS console vs `node file.js` analogy.

PHASE 4 EXERCISE 1: Have me write a tiny script that prints a few things and does some math, in a file,
run from the terminal — hints freely available.

PHASE 5 EXERCISE 2: Have me write a slightly different small script solo, run it, and give me the
verdict on whether I'm comfortable running Python.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

You now have Python installed (like having Node installed) and can run code two ways: the **REPL** (type a line, see it instantly — exactly like the browser console) and a **script** (a `.py` file run all at once — like `node file.js`). `print()` is Python's `console.log()`. Variables need no `const`/`let` — just `name = "value"`.

The principle: **Python is installed like Node, and you run its code either interactively (REPL, like the console) or as a script file — and the basics map closely onto JavaScript ideas you already have.** The differences are surface syntax, not new concepts.

</details>

---

## Module 0.3 — Python Basics I: Variables, Strings, Numbers, and print()

**Difficulty:** ●●○○○ · **Format:** STUDY-heavy · **Stack:** Python

### New words in this module

- **f-string**: Python's way of putting variables inside text, like JS template literals. JS: `` `Hello ${name}` `` → Python: `f"Hello {name}"`.
- **Type**: what kind of value something is — text (`str`), whole number (`int`), decimal (`float`), true/false (`bool`). Same idea as JS types.
- **Comment**: a note in code the computer ignores. JS uses `//`; Python uses `#`.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **Python for Professional Developers** → the language-basics sections (variables, strings, numbers, printing). This is your core reference for this and the next few modules.

**Read:**

- **Official Python Tutorial — "An Informal Introduction"** — https://docs.python.org/3/tutorial/introduction.html — *Focus on: numbers and strings. Skim; you know these concepts from JS.*
- **Real Python — "f-strings"** — https://realpython.com/python-f-strings/ — *Focus on: `f"...{variable}..."` — the template-literal equivalent.*
- **"Python for JavaScript developers"** — search `"python for javascript developers cheat sheet"` — *Focus on: the side-by-side syntax table. Keep this open as a reference.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.3 — Python basics: variables, strings, numbers,
print, f-strings. I know these concepts from JS; teach me the PYTHON SYNTAX for them. Gentle rhythm.

PHASE 1 STUDY: Write me a small, well-commented script that demonstrates: making variables (string,
int, float, bool), printing them, f-strings for inserting variables into text (compare to JS template
literals), basic math, and `#` comments. For each line, give the JS equivalent so I can map it.

PHASE 2 TINKER: Have me change values, make my own f-string greeting, try integer vs float division
(Python has a quirk here — let me discover it), and intentionally cause a small error to see what a
Python error message looks like.

PHASE 3 REPORT: I report what I changed and any surprises. Check I can read a basic Python error.

PHASE 4 EXERCISE 1: Have me write a script that takes a couple of variables and prints a formatted
sentence using f-strings and a calculation — hints free.

PHASE 5 EXERCISE 2: A slightly different formatted-output script solo, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

Python variables, strings, numbers, and printing — all concepts you already have from JS, with new syntax: no `const`/`let`, `#` for comments, `f"...{var}..."` f-strings instead of backtick template literals, and `print()` instead of `console.log()`. You also met your first Python error messages (which are actually quite readable).

The principle: **Python's basic building blocks map almost one-to-one onto JavaScript's — the work here is learning the new syntax for ideas you already understand, and getting comfortable reading Python's error messages.**

</details>

---

## Module 0.4 — Python Basics II: Lists, Dictionaries, and Indentation

**Difficulty:** ●●○○○ · **Format:** STUDY-heavy · **Stack:** Python

### New words in this module

- **List**: an ordered collection, like a JS array. JS: `[1, 2, 3]` → Python: `[1, 2, 3]` (same!).
- **Dictionary (dict)**: key-value pairs, like a JS object. JS: `{name: "Kedar"}` → Python: `{"name": "Kedar"}` (keys are usually quoted strings).
- **Indentation**: Python uses *indentation (spaces) to define code blocks* instead of `{ }` braces. This is the biggest visual difference from JS, and it matters — wrong indentation is an error.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **Python for Professional Developers** → the data-structures sections (lists, dictionaries) and the bit on indentation/blocks.

**Read:**

- **Official Python Tutorial — "Data Structures"** — https://docs.python.org/3/tutorial/datastructures.html — *Focus on: lists and dictionaries. These are arrays and objects with different names.*
- **Real Python — "Lists and Tuples"** and **"Dictionaries"** — https://realpython.com/python-lists-tuples/ and https://realpython.com/python-dicts/ — *Focus on: creating, accessing, looping.*
- **"Python indentation explained"** — search `"python indentation blocks vs curly braces"` — *Focus on: why indentation replaces `{ }`, and why consistency matters.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.4 — lists, dicts, indentation. Map these to JS
arrays and objects. The indentation thing is new and important — make sure I really get it. Gentle rhythm.

PHASE 1 STUDY: Write a commented script showing: a list (like a JS array) — create, access by index,
add an item, loop over it; a dictionary (like a JS object) — create, access by key, add a key. And
CRUCIALLY, show how Python uses INDENTATION instead of `{ }` for blocks (e.g. the body of a loop is
indented). Give JS equivalents throughout. Explain why indentation errors happen.

PHASE 2 TINKER: Have me add/remove items, loop and print, access a dict value, and DELIBERATELY mess up
the indentation to see the error — so the error becomes familiar, not scary.

PHASE 3 REPORT: I report what I found. Check I understand list vs dict (array vs object) and that
indentation defines blocks.

PHASE 4 EXERCISE 1: Have me build a small list of dictionaries (like an array of objects — e.g. a few
people with names and ages) and loop over it printing each — hints free.

PHASE 5 EXERCISE 2: A different list-of-dicts task solo, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

Lists are JS arrays; dictionaries are JS objects — same concepts, slightly different syntax. The genuinely new thing is **indentation defines code blocks** in Python instead of `{ }` braces: the indented lines after a `for` or `if` are that block's body. Get the indentation wrong and Python errors — so consistency (use the editor's auto-indent) matters. A "list of dictionaries" is an "array of objects," which is the shape most data comes in, including AI API responses.

The principle: **Python's lists and dictionaries are JavaScript's arrays and objects under new names, and the one real adjustment is that indentation — not braces — defines blocks.** Once indentation feels natural, Python reads easily.

</details>

---

## Module 0.5 — Python Basics III: Functions, Loops, and Conditionals

**Difficulty:** ●●○○○ · **Format:** STUDY-heavy · **Stack:** Python

### New words in this module

- **Function**: a reusable block of code. JS: `function greet(name) { ... }` → Python: `def greet(name):` then indented body.
- **`def`**: the keyword to define a function (like JS's `function`).
- **`for` loop**: repeat over items. Python's `for item in my_list:` is like JS's `for (const item of myList)`.
- **`if` / `elif` / `else`**: conditionals. `elif` is Python's `else if`.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **Python for Professional Developers** → the functions, loops, and conditionals sections.

**Read:**

- **Official Python Tutorial — "Control Flow"** — https://docs.python.org/3/tutorial/controlflow.html — *Focus on: `if`/`elif`/`else`, `for`, and `def` functions.*
- **Real Python — "Defining Functions"** — https://realpython.com/defining-your-own-python-function/ — *Focus on: `def`, parameters, `return`.*
- **"Python loops for JavaScript developers"** — search `"python for loop vs javascript for of"` — *Focus on: `for x in things:` vs JS `for...of`.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.5 — functions, loops, conditionals in Python.
I know these from JS; teach the Python syntax. Gentle rhythm, JS analogies throughout.

PHASE 1 STUDY: Write a commented script showing: a function with `def`, parameters, and `return`; a
`for` loop over a list; an `if`/`elif`/`else`. Show the JS equivalent of each. Emphasize the `:` and
indentation pattern that defines each block.

PHASE 2 TINKER: Have me write my own small function, call it with different inputs, add an `elif`
branch, and loop over a list calling my function on each item. Predict outputs first.

PHASE 3 REPORT: I report. Check I can write a basic function and loop, and that I understand `return`
vs `print` (compute-and-hand-back vs show-on-screen — a common beginner mix-up).

PHASE 4 EXERCISE 1: Have me write a function that takes a list and returns something computed from it
(e.g. filters or sums), then prints the result — hints free.

PHASE 5 EXERCISE 2: A different function-and-loop task solo, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

Functions (`def`), loops (`for x in things:`), and conditionals (`if`/`elif`/`else`) — the control flow you know from JS, in Python syntax. The pattern is consistent: a line ending in `:` starts a block, and the indented lines below are that block's body. One common beginner trap clarified: `return` hands a value back to whoever called the function (for further use), while `print` just displays text on screen — they're not interchangeable.

The principle: **Python's functions, loops, and conditionals are the same control-flow tools as JavaScript's, following the consistent `header-colon-then-indented-block` shape — and `return` (hand back a value) is distinct from `print` (show on screen).**

</details>

---

## Module 0.6 — Packages, pip, and What an "SDK" Actually Is

**Difficulty:** ●●○○○ · **Format:** Guided hands-on · **Stack:** Python + pip

### New words in this module

- **Package / library**: reusable code someone else wrote that you install and use, so you don't reinvent it. Exactly like an npm package. (Python people say "package" or "library"; same thing.)
- **pip**: Python's package installer — the `npm` of Python. `pip install requests` is like `npm install axios`.
- **PyPI**: the Python Package Index — the giant registry of packages, like npmjs.com.
- **import**: bring a package's code into your file so you can use it. Like JS `import` / `require`.
- **SDK (Software Development Kit)**: a package a company gives you to talk to their service easily. The `anthropic` SDK is a Python package that makes calling Claude's API simple — instead of hand-writing HTTP requests, you call `client.messages.create(...)`. An SDK is "an official library for using a specific service."

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **Python for Professional Developers** → the package management / pip section.
- **AI Engineering Fundamentals** → the early setup parts where SDKs are introduced (you'll use this course heavily soon; this is a gentle first look at what an SDK is).

**Read:**

- **Real Python — "What is pip?"** — https://realpython.com/what-is-pip/ — *Focus on: `pip install`, the npm analogy.*
- **PyPI** — https://pypi.org — *Look up the `anthropic` package and `requests`. Seeing the registry makes it concrete (it's npmjs.com for Python).*
- **"What is an SDK vs API"** — search `"sdk vs api difference simple explanation"` — *Focus on: an API is the service's "menu of things you can request"; an SDK is a convenient toolkit for ordering from that menu in your language.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.6 — packages, pip, and what an SDK is. I keep
hearing "SDK" and don't really know what it means. Decode it fully with npm analogies. Gentle rhythm.

PHASE 1 STUDY: Explain, with the npm analogy throughout: what a package/library is, what pip is
(`pip install X` ≈ `npm install X`), what PyPI is (≈ npmjs.com), and what `import` does. Then explain
what an SDK is in plain words — "an official library for talking to a company's service" — and preview
that the `anthropic` SDK is how we'll talk to Claude without hand-writing HTTP. Have me install one
simple, harmless package (e.g. `requests`) and import it in a script to prove the flow works.

PHASE 2 TINKER: Have me use the installed package for something tiny (e.g. `requests` to fetch a public
URL and print the status), and look it up on PyPI. Let me see that "install, import, use" is the same
loop as npm.

PHASE 3 REPORT: I report. Check I can explain, plainly, what pip / a package / an SDK are — using the
npm analogy. Make me say what an SDK is in one sentence.

PHASE 4 EXERCISE 1: Have me install and use a different simple package in a script — hints free.

PHASE 5 EXERCISE 2: Have me install and import another package and use one of its functions solo, then
the verdict — and confirm I could explain "what's an SDK?" in an interview.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

This demystifies three words that are everywhere in AI tutorials. A **package/library** is reusable code you install (like an npm package). **pip** is Python's installer (`pip install X` ≈ `npm install X`), pulling from **PyPI** (Python's npmjs.com). `import` brings a package into your file. And an **SDK** is just "an official library a company provides for using their service" — the `anthropic` SDK lets you call Claude with `client.messages.create(...)` instead of hand-writing raw HTTP requests.

The interview-grade distinction: an **API** is the service's set of operations you can request (its "menu"); an **SDK** is a convenient library in your language for making those requests (a tool for "ordering from the menu"). You can use an API without an SDK (by hand-writing HTTP), but the SDK makes it pleasant.

The principle: **packages are installable reusable code (pip is Python's npm), and an SDK is an official package for talking to a specific service — so "use the anthropic SDK" just means "install and import the official library for calling Claude."** No more mystery in that word.

</details>

---

## Module 0.7 — Virtual Environments: Why and How (the `.venv`)

**Difficulty:** ●●○○○ · **Format:** Guided hands-on · **Stack:** Python + venv

### New words in this module

- **Virtual environment (venv)**: a private, isolated copy of Python and its packages, just for one project — so different projects can't break each other's dependencies. The rough equivalent of how each JS project has its own `node_modules` folder.
- **Activate**: "turn on" a virtual environment in your terminal so that `python` and `pip` use that project's private copy.
- **`requirements.txt`**: a list of the packages a project needs, so anyone can recreate the environment. Like `package.json`'s dependencies list.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **Python for Professional Developers** → the virtual environments / project setup section. This is the canonical walkthrough — follow it for your OS.

**Read:**

- **Official Python — "venv"** — https://docs.python.org/3/library/venv.html — *Focus on: creating and activating a venv.*
- **Real Python — "Python Virtual Environments: A Primer"** — https://realpython.com/python-virtual-environments-a-primer/ — *Focus on: WHY isolation matters (no global dependency clashes), and the create/activate/install workflow.*
- **"venv vs node_modules"** — search `"python virtual environment vs node_modules"` — *Focus on: the analogy (per-project isolation), even though the mechanics differ.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.7 — virtual environments. This is new to me;
explain WHY it exists (the node_modules-style isolation analogy) and walk me through it carefully.
Gentle rhythm.

PHASE 1 STUDY: Explain why a virtual environment exists — so each project has its own isolated packages
and they don't clash globally (loosely like each JS project having its own node_modules). Then walk me,
step by step, through: creating a venv in my project folder, ACTIVATING it (and how to tell it's active),
installing a package INTO it, and freezing a requirements.txt. Show me how to tell whether I'm "inside"
the venv or not (a common confusion).

PHASE 2 TINKER: Have me create a venv, activate it, install a package, run `pip list` to see it's
isolated, deactivate, and notice the difference. Make the "am I in the venv?" check concrete.

PHASE 3 REPORT: I report. Check I understand why isolation matters and how to activate/know-I'm-active.

PHASE 4 EXERCISE 1: Have me set up a fresh venv for our AI primer project, activate it, install the
`anthropic` and `python-dotenv` packages (we'll use them next module), and freeze requirements.txt —
hints free.

PHASE 5 EXERCISE 2: Have me create a second throwaway project with its own venv and a different package
solo, proving I can do the workflow, then the verdict. Confirm I could explain "why use a virtual
environment?" in an interview.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

A **virtual environment** is a private, isolated Python setup for one project. Without it, every package you install is global, and two projects needing different versions of the same package clash. With it, each project has its own packages — loosely analogous to how each JS project has its own `node_modules`. The workflow: **create** the venv, **activate** it (so `python`/`pip` use the project's private copy — you'll see the venv's name in your terminal prompt when it's active), **install** packages into it, and **freeze** a `requirements.txt` so others (or future-you) can recreate it.

The interview answer: *"Why use a virtual environment?" → "To isolate each project's dependencies so they don't clash globally — different projects can use different package versions without interfering, and `requirements.txt` makes the environment reproducible."*

The principle: **a virtual environment gives each project its own isolated packages (conceptually like per-project node_modules), avoiding global dependency clashes — you create it, activate it, install into it, and freeze requirements.txt for reproducibility.** Always work inside an activated venv from now on.

</details>

---

## Module 0.8 — Secrets, API Keys, and the `.env` File

**Difficulty:** ●●○○○ · **Format:** Guided hands-on · **Stack:** Python + python-dotenv

### New words in this module

- **API key**: a secret password that identifies you to a service (like Anthropic) and lets you use it (and get billed). Anyone with your key can spend your money — so it's a real secret.
- **Environment variable**: a value stored *outside* your code, in the environment your program runs in, so secrets don't live in the code. Like a config value you don't hardcode.
- **`.env` file**: a simple text file holding your secrets as `KEY=value` lines, which your program reads at startup. It must NEVER be committed to git.
- **`.gitignore`**: a file listing things git should ignore (never save/commit) — you'll put `.env` in it. (You may know this from frontend already.)
- **python-dotenv**: a small package that loads your `.env` file's values into your program. (In Node you'd use the `dotenv` package — same idea.)

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **AI Engineering Fundamentals** → the setup section covering API keys and environment configuration (you'll lean on this course heavily next track; this is the safe-setup part).
- **Cursor & Claude Code Professional AI Setup** → the parts on configuring keys/secrets cleanly.

**Read:**

- **python-dotenv docs** — https://pypi.org/project/python-dotenv/ — *Focus on: a `.env` file + `load_dotenv()` to read it.*
- **Anthropic — API keys / setup** — https://docs.claude.com/en/api/getting-started — *Focus on: where your key comes from and that it's secret.*
- **"Why never commit API keys"** — search `"never commit api keys to git leaked secrets"` — *Focus on: real consequences of a leaked key (bills, abuse). This is important, not paranoia.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.8 — secrets and the .env file. This protects me
from a real, expensive mistake, so be careful and clear. Gentle rhythm. (I should already have an
Anthropic API key from my account; if not, point me to where to get one.)

PHASE 1 STUDY: Explain what an API key is (a secret that spends my money), why it must NEVER be in code
or committed to git, and the safe pattern: put it in a `.env` file, add `.env` to `.gitignore`, and load
it in Python with python-dotenv (`load_dotenv()` then read it via `os.environ`). Show me a worked
example of the safe pattern. Explain environment variables plainly.

PHASE 2 TINKER: Have me create a `.env` with a FAKE placeholder value first, add it to `.gitignore`,
write a script that loads and prints it (proving the mechanism) — using a fake value so nothing's at
risk. Then have me confirm `.env` is gitignored (so it won't be committed).

PHASE 3 REPORT: I report. Then have me put my REAL Anthropic key in `.env` and confirm (without printing
it publicly) that the script can read it. Double-check `.gitignore` covers it.

PHASE 4 EXERCISE 1: Have me set up the .env + dotenv pattern cleanly in our AI primer project so it's
ready for the first real API call next module — hints free. If I ever try to hardcode the key, stop me.

PHASE 5 EXERCISE 2: Have me explain back, and set up the same secret-loading pattern in a second small
project solo, then the verdict. Confirm I could explain "how do you keep API keys safe?" in an interview.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

An **API key** is a secret that authenticates you to a service and is tied to billing — anyone who gets it can spend your money, so it's a genuine password. The safe pattern, which you'll use forever: put secrets in a **`.env` file** (`ANTHROPIC_API_KEY=sk-...`), add `.env` to **`.gitignore`** so it's never committed, and load it in Python with **python-dotenv** (`load_dotenv()`, then read via `os.environ["ANTHROPIC_API_KEY"]`). The key lives in the environment, never in your code.

The interview answer: *"How do you handle API keys/secrets?" → "Never in code or version control. Store them in environment variables (e.g. a gitignored `.env` file loaded at runtime), so secrets stay out of the codebase and out of git history. A leaked key can be abused and billed, so it's treated like a password."*

The principle: **API keys are billable secrets, so they live in environment variables (a gitignored `.env` loaded by python-dotenv), never in code or git — this single habit prevents one of the most common and costly beginner mistakes.**

</details>

---

## Module 0.9 — What an API Is, What an "Endpoint" Is (the restaurant analogy)

**Difficulty:** ●●○○○ · **Format:** Investigation · **Stack:** concepts + a tiny request

### New words in this module

- **API (Application Programming Interface)**: a way for your program to ask another program/service to do something or give you data. The service publishes "here are the things you can ask me for, and how."
- **Endpoint**: one specific "thing you can ask for" at a URL. E.g. the Anthropic messages endpoint is the URL you send your prompt to. "Endpoint" = "a specific address/operation of an API."
- **Request / Response**: you send a request (your ask), you get a response (the answer). The same request/response you already know from fetch/axios on the frontend.
- **HTTP / JSON**: HTTP is the protocol of the web (how requests travel); JSON is the text format the data is usually in (you know JSON from frontend).

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **AI Engineering Fundamentals** → the section explaining how you talk to an LLM provider's API (request/response, endpoints).
- **AI for Software Engineers** → the parts framing APIs and how software talks to AI services (good conceptual grounding).

**Read:**

- **MDN — "Introduction to web APIs"** — https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction — *Focus on: the concept; you've used APIs from the frontend (fetch), so this is familiar framing.*
- **"What is an API endpoint"** — search `"what is an api endpoint simple explanation"` — *Focus on: endpoint = a specific URL/operation of an API.*
- **"API restaurant analogy"** — search `"api waiter restaurant analogy"` — *Focus on: you (the program) order from a menu (the API) via a waiter; the kitchen (service) makes it; you get your dish (response). An endpoint is a specific menu item.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.9 — APIs and endpoints. I've used fetch/axios on
the frontend, so anchor to that. Decode "endpoint" fully — I hear it constantly and it's fuzzy. Gentle
rhythm.

PHASE 1 STUDY: Explain, with the restaurant analogy AND the frontend-fetch analogy: what an API is, what
an ENDPOINT is (a specific URL/operation), and the request→response cycle. Then show me a tiny worked
example using `requests` to call a simple public API endpoint and read the JSON response — relating each
part (URL, request, response, JSON) to fetch from frontend.

PHASE 2 TINKER: Have me call a different simple public endpoint, inspect the JSON response, and pick out
a value from it. Make the request→response→parse-JSON loop concrete and familiar.

PHASE 3 REPORT: I report. Make me explain, plainly, what an API and an endpoint are, and map "endpoint"
to something concrete (a specific URL you send a request to).

PHASE 4 EXERCISE 1: Have me call another public API endpoint and extract and print specific fields from
the JSON — hints free.

PHASE 5 EXERCISE 2: A different public-API call solo, extracting data, then the verdict. Confirm I could
explain "what's an API endpoint?" cleanly in an interview.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

You already used APIs on the frontend (every `fetch`/`axios` call). An **API** is how your program asks another service for data or actions; an **endpoint** is one specific operation at a URL (the messages endpoint, the embeddings endpoint, etc.). You send a **request**, you get a **response**, usually as **JSON** (which you know from frontend). The restaurant analogy: the API is the menu, an endpoint is a specific dish you can order, the request is your order, the response is the dish.

This demystifies the sentence that overwhelmed you earlier — "use the token-counting endpoint" just means "send a request to the specific URL that counts tokens, and read the number back from the JSON response." Nothing more.

The principle: **an API is a service your program calls; an endpoint is one specific operation/URL of that API; you send a request and get a JSON response — exactly the fetch/request-response model you already know from frontend, just from Python instead of the browser.** "Endpoint" will never be fuzzy again.

</details>

---

## Module 0.10 — Your First Real Claude API Call (written WITH you)

**Difficulty:** ●●○○○ · **Format:** STUDY → build · **Stack:** Python + anthropic SDK

### New words in this module

- **`client`**: the object from the SDK that you call methods on to talk to Claude (e.g. `client.messages.create(...)`). Think of it as "the configured connection to Claude."
- **`messages`**: the list of the conversation so far that you send with each call (recall: the API is stateless, so you send the history each time — you'll feel this for real here).
- **`model`**: which Claude model to use (e.g. a Sonnet or Opus identifier).
- **`max_tokens`**: the cap on how long the reply can be (in tokens).

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **AI Engineering Fundamentals** → the first-API-call / "hello world with an LLM" section. This is the canonical walkthrough; follow it.
- **Practical Prompt Engineering** → the intro sections (you'll do prompting properly in the next track, but seeing a real call here helps).

**Read:**

- **Anthropic — Messages API / Quickstart** — https://docs.claude.com/en/docs/get-started and https://docs.claude.com/en/api/messages — *Focus on: `client.messages.create(model=..., max_tokens=..., messages=[...])` and the response shape.*
- **anthropic Python SDK README** — https://github.com/anthropics/anthropic-sdk-python — *Focus on: the minimal example.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.10 — my FIRST real Claude API call. This is a big
milestone for me; go slow and make it feel achievable. I've set up venv + .env + the anthropic SDK in
earlier modules. Gentle rhythm, and WRITE THE FIRST VERSION WITH ME.

PHASE 1 STUDY: Write me a complete, working, heavily-commented script that: loads my API key from .env,
creates the anthropic `client`, sends a single message to Claude, and prints the reply. Explain EVERY
line — what `client` is, what `messages=[{"role": "user", "content": "..."}]` means, what `model` and
`max_tokens` do, and how to dig the text out of the response object. Relate it to a fetch call returning
JSON. Let me ask about any line until it's all clear.

PHASE 2 TINKER: Have me change the prompt, change max_tokens (and see the reply get cut off), change the
model, and print the usage/token info from the response. Predict-then-run each time.

PHASE 3 REPORT: I report what I saw. Check I can trace the whole flow: key → client → messages →
response → text. Celebrate this — I just talked to an LLM from my own code.

PHASE 4 EXERCISE 1: Have me write (mostly myself, hints free) a small script that asks Claude something
and prints the answer — a clean version of my own.

PHASE 5 EXERCISE 2: Have me write a script that sends TWO turns of conversation — and discover that I
must include the previous messages in the `messages` list or Claude "forgets" (statelessness, felt for
real). Then the verdict. This sets up the whole "memory is engineered" idea for the AI tracks.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

You just talked to Claude from your own Python code — the milestone everything else builds on. The flow: load the key from `.env` → create the `client` (your configured connection) → call `client.messages.create(model=..., max_tokens=..., messages=[...])` → read the text out of the response. The `messages` list is the conversation; each item has a `role` (`user`/`assistant`) and `content`.

In Exercise 2 you felt **statelessness** for real: a second turn "forgets" the first unless you include the earlier messages in the `messages` list you send. That's not a bug — the API keeps no memory, so YOUR code supplies the history each call. This is the seed of a huge idea in the AI tracks: "memory" (chat history, agents, RAG) is all just *you deciding what to put in the messages list each time*.

The principle: **a Claude API call is load-key → client → `messages.create` → read response, and because the API is stateless you must resend the conversation each turn — which is why "memory" in AI systems is something you engineer, not something the model has.** You're now over the scariest hump: you can call an LLM from code.

</details>

---

## Module 0.11 — JSON: The Language APIs Speak

**Difficulty:** ●●○○○ · **Format:** STUDY → build · **Stack:** Python + json

### New words in this module

- **JSON (JavaScript Object Notation)**: a text format for structured data — you already know it from frontend. AI APIs send and receive JSON.
- **Parse**: turn JSON text into a usable Python object (dict/list). **Serialize/dump**: turn a Python object back into JSON text.
- **`json` module** (Python built-in): tools to parse (`json.loads`) and produce (`json.dumps`) JSON.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **AI Engineering Fundamentals** → any section handling structured/JSON responses from the model (you'll do this seriously in the prompting track; this is the data-format groundwork).

**Read:**

- **Python docs — `json` module** — https://docs.python.org/3/library/json.html — *Focus on: `json.loads` (parse) and `json.dumps` (produce).*
- **MDN — JSON** — https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/JSON — *Refresher; you know this from frontend.*
- **"JSON in Python for JavaScript developers"** — search `"python json loads dumps vs JSON.parse stringify"` — *Focus on: `json.loads` ≈ `JSON.parse`, `json.dumps` ≈ `JSON.stringify`.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.11 — JSON in Python. I know JSON from frontend;
teach me Python's tools for it and why it matters for AI. Gentle rhythm, JS analogies (loads≈parse,
dumps≈stringify).

PHASE 1 STUDY: Write a commented script showing: parsing a JSON string into a Python dict (`json.loads`,
like `JSON.parse`), accessing nested values, and producing JSON from a Python object (`json.dumps`, like
`JSON.stringify`). Connect it to API responses: the data that comes back is JSON, and I parse it to use
it. Show a realistic nested example like an API might return.

PHASE 2 TINKER: Have me parse a sample JSON response, pull out nested fields, modify the object, and dump
it back to a JSON string. Deliberately access a missing key to see the error.

PHASE 3 REPORT: I report. Check I can navigate nested JSON/dicts confidently — this matters a lot for
reading AI responses.

PHASE 4 EXERCISE 1: Have me take a realistic JSON blob (like an API response), parse it, and extract and
print several specific nested values — hints free.

PHASE 5 EXERCISE 2: A different JSON-parsing-and-extraction task solo, then the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

JSON is the structured-data text format you already know from frontend, and it's what AI APIs speak. In Python: `json.loads(text)` parses JSON text into a dict/list (like `JSON.parse`), and `json.dumps(obj)` turns a Python object back into JSON text (like `JSON.stringify`). Reading AI responses is largely navigating parsed JSON (dicts and lists), so comfort here pays off constantly — especially soon when you ask models to *return* JSON (structured outputs, Track 1).

The principle: **JSON is the lingua franca of APIs, and Python's `json.loads`/`json.dumps` are its `JSON.parse`/`JSON.stringify` — fluently navigating nested dicts/lists is how you read AI responses, and it's a skill you already have from frontend.**

</details>

---

## Module 0.12 — Reading Errors & Debugging Without Panic

**Difficulty:** ●●○○○ · **Format:** Investigation · **Stack:** Python

### New words in this module

- **Traceback**: Python's error report — it shows what went wrong and the chain of calls that led there. Read it bottom-up: the last line names the error.
- **Exception**: an error that stops your program (e.g. `KeyError`, `TypeError`). You can "catch" and handle these.
- **`try` / `except`**: Python's way to handle errors gracefully (JS's `try`/`catch`). Vital for AI, where calls can fail or return unexpected things.

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **Python for Professional Developers** → the error-handling / exceptions section.

**Read:**

- **Python Tutorial — "Errors and Exceptions"** — https://docs.python.org/3/tutorial/errors.html — *Focus on: reading tracebacks and `try`/`except`.*
- **Real Python — "Python Exceptions"** — https://realpython.com/python-exceptions/ — *Focus on: common exceptions and handling them.*
- **"How to read a Python traceback"** — search `"how to read python traceback beginner"` — *Focus on: read bottom-up, the last line is the actual error.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.12 — reading errors and try/except. Errors scare
me right now; make them un-scary and teach me to handle them. Gentle rhythm, JS try/catch analogy.

PHASE 1 STUDY: Show me how to READ a Python traceback (bottom-up; last line = the real error), with a few
common ones (KeyError, TypeError, etc.) deliberately triggered so I see them. Then show `try`/`except`
(like JS try/catch) to handle an error gracefully instead of crashing. Explain why this matters
especially for AI calls (the network can fail, the response can be unexpected).

PHASE 2 TINKER: Have me deliberately cause a few different errors, read each traceback and say what it
means, then wrap risky code in try/except and handle it. Make errors feel like information, not failure.

PHASE 3 REPORT: I report. Check I can read a traceback and write a basic try/except.

PHASE 4 EXERCISE 1: Have me write code that might fail (e.g. accessing data that might be missing) and
handle it gracefully with try/except, printing a friendly message — hints free.

PHASE 5 EXERCISE 2: A different error-handling task solo, then the verdict. This skill matters a lot once
AI calls (which fail in messy ways) start.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

Errors are information, not failure. A Python **traceback** is the error report — read it bottom-up; the last line names the actual error (e.g. `KeyError: 'name'` means you asked for a dict key that isn't there). **`try`/`except`** (Python's `try`/`catch`) lets you handle an error gracefully instead of crashing. This matters enormously for AI: network calls fail, responses come back in unexpected shapes, and a real system handles those cases instead of crashing — exactly the "handle the failure path" discipline the AI tracks will demand.

The principle: **a traceback is readable (bottom-up, last line is the error) and `try`/`except` handles failures gracefully — and since AI calls fail in messy ways (network, unexpected output), comfort with reading errors and handling them is foundational for building reliable AI systems.** Errors stop being scary once you can read them.

</details>

---

## Module 0.13 — A Tiny Taste of Async (just enough to not be scared later)

**Difficulty:** ●●●○○ · **Format:** STUDY → light build · **Stack:** Python + asyncio

### New words in this module

- **Synchronous**: code runs one line at a time, each finishing before the next starts. (Most code you've written.)
- **Asynchronous (async)**: code that can "wait" for slow things (like a network/API call) without blocking everything else — letting other work happen meanwhile. You know this from JS Promises / `async`/`await`.
- **`async` / `await`**: Python has these too, very similar to JS. You'll see them in some AI libraries.

*Note: you don't need deep async to start AI — this module is a gentle "don't panic when you see `await`" primer, so later code isn't confusing. Keep it light.*

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **Python for Professional Developers** → the async section if it has one (otherwise rely on the reading below — async is optional-depth at this stage).

**Read:**

- **Real Python — "Async IO in Python"** — https://realpython.com/async-io-python/ — *Focus on: the concept and `async`/`await` syntax; don't go deep yet.*
- **"Python async vs JavaScript async await"** — search `"python asyncio vs javascript async await comparison"` — *Focus on: how similar they are; you already understand the concept from JS.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.13 — a GENTLE taste of async. I know async/await
from JS. I just want to not be confused when I see `await` in AI library code. Keep this light — don't
overwhelm me with the deep async machinery. Gentle rhythm.

PHASE 1 STUDY: Explain sync vs async with the JS analogy (Promises/async-await), then show a tiny Python
example using `async`/`await` so the syntax is familiar. Emphasize: I don't need to master this now; I
just need to recognize it. Show how it maps to JS.

PHASE 2 TINKER: Have me run the example, tweak it, and observe that async lets waiting happen without
blocking. Keep it small.

PHASE 3 REPORT: I report. Check I can recognize `async`/`await` and explain the basic idea (don't block
while waiting on slow things) — that's enough for now.

PHASE 4 EXERCISE 1: Have me write a tiny async example mostly myself — hints free. Low stakes.

PHASE 5 EXERCISE 2: A slightly different tiny async snippet solo, then a gentle verdict. It's fine if
this one's just "recognize it, revisit later" — note that in the verdict.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

This is deliberately light. **Async** lets your program wait for slow things (like API calls) without freezing everything else — exactly the concept you know from JavaScript Promises and `async`/`await`. Python's `async`/`await` syntax is very similar. You don't need to master async to start building AI apps (many examples are synchronous), but you'll occasionally see `await` in AI library code, and now it won't confuse you. You can revisit async in depth later (the production track touches it for handling many calls efficiently).

The principle: **async lets code wait on slow operations without blocking, via `async`/`await` that closely mirrors JavaScript's — and at this stage you only need to recognize it, not master it.** This was a "don't panic later" module, not a deep dive.

</details>

---

## Module 0.14 — Putting the Tools Together: A Clean Project From Scratch

**Difficulty:** ●●●○○ · **Format:** Synthesis build · **Stack:** Everything from Track 0

### New words in this module

- *(None new — this synthesizes everything.)*

### Prep — Watch & Read

**Watch (Frontend Masters):**

- **Cursor & Claude Code Professional AI Setup** → the project-structure / clean-setup sections — good model for organizing an AI project well.
- **AI Engineering Fundamentals** → revisit the setup section now that the pieces make sense.

**Read:**

- Revisit your own `NOTES.md` files from 0.1–0.13. This module is synthesis.
- **"Python project structure best practices"** — search `"python project structure venv requirements .env beginner"` — *Focus on: a clean, simple layout.*

### STUDY → TINKER → REPORT → EXERCISE (paste into Claude Code)

```text
Follow CLAUDE.md and AI-CONTRACT.md. Track 0, Module 0.14 — synthesis: build a clean AI project from
nothing, using everything in Track 0. This proves I have the foundation before AI begins. Gentle rhythm,
but I should be able to do more of this myself now — calibrate to how I'm doing (Zone of Proximal
Development).

PHASE 1 STUDY: Briefly recap the full workflow as a checklist (folder + terminal, venv, activate,
install packages, requirements.txt, .env + .gitignore for the key, a script that calls Claude, reads the
JSON response, handles errors with try/except). Don't re-teach each — just assemble the map.

PHASE 2 TINKER / BUILD: Guide me to build, from an empty folder, a clean little project that: sets up a
venv, installs anthropic + python-dotenv, safely loads my key, calls Claude with a question, parses and
prints the answer, and gracefully handles a failure (e.g. wrap the call in try/except). Let me do as much
as I can; hint where I stall.

PHASE 3 REPORT: I report. Check I can now stand up an AI project end-to-end without hand-holding on the
plumbing — because the AI tracks assume this is comfortable.

PHASE 4 EXERCISE 1: Have me add a small feature to the project (e.g. ask Claude two things, or loop over
a few questions) — hints available but encourage independence.

PHASE 5 EXERCISE 2: Have me create a BRAND NEW clean AI-ready project from scratch, entirely solo (venv,
deps, .env, a working Claude call with error handling), then give me the big verdict: am I ready to leave
Track 0 and start the actual AI foundations (Track 1), or which tool/Python module should I revisit?
Confirm I could set up an AI project in an interview/take-home without panic.
```

### 🔒 SPOILER — What This Is Teaching

<details>
<summary>Click after you finish</summary>

This welds Track 0 into one fluent workflow: from an empty folder you can now make a project, set up and activate a **venv**, install packages with **pip**, track them in **requirements.txt**, keep your key safe in a gitignored **`.env`** loaded by **python-dotenv**, write a Python **script** that creates the **client** and calls Claude's **API**, parse the **JSON** response, and handle failures with **try/except**. Every word that was jargon two weeks ago is now a thing you've done.

This is the real graduation: the AI tracks can now assume you're comfortable with the plumbing, so they can focus on the *AI ideas* instead of stopping to explain what an SDK is. You climbed the on-ramp.

The principle: **standing up a clean, safe, working AI project from an empty folder — venv, deps, secrets, a real API call, error handling — is the foundation every AI system is built on, and you can now do it without panic.** You're ready for the AI itself.

</details>

---

*End of Track 0 — Python & The Tools, for a JavaScript Developer. 14 modules. You went from "what's an SDK?" to standing up a clean AI project from scratch — venv, packages, secrets, a real Claude API call, JSON parsing, and error handling — with every piece of jargon decoded along the way. The scary part is behind you.*

*One heads-up before you go: Track 0 was deliberately exempt from the 🧊 cold-open discipline — pure on-ramp, zero pressure. From Track 1 onward, every module opens with a tiny no-notes quiz on earlier material, and every track ends with a timed cold rebuild. It will feel like a step up. It is — a gentle one, and it's the mechanism that makes everything you learn here permanent instead of leaky.*

*Next: Track 1 — Foundations: How LLMs Actually Work, where you learn what the thing you just called actually IS (next-token prediction, tokens, context, temperature) — the theory your theory-loving brain has been waiting for, now that you can comfortably run the code to explore it.*
