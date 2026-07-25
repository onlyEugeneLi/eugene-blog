# Agentic AI Programming — Skill Development Syllabus

📊 **[Visual syllabus map](https://claude.ai/code/artifact/411734f3-694f-4cae-9d28-bb50683562ed)** — diagrams and at-a-glance view of everything below.

Future-proofing plan for mastering agentic AI software engineering, organized into three parts:

1. **Course Recommendations & Sequence** — theory, in watch order
2. **Mini Project Plan** — "FixerBot", a 4-week hands-on build
3. **Reference Doc** — Spotify "Honk" production case study

---

# Part 1: Course Recommendations & Sequence

Prioritized lectures, workshops, and deep dives, organized sequentially from basic LLM mechanics to advanced agent loop design patterns.

## Phase 1: The Token Stream & LLM Mechanics
Before writing loops, understand how language models process instructions, manage state, and maintain a context window.

- **[Deep Dive into LLMs like ChatGPT](https://www.youtube.com/watch?v=7xTGNNLPyMI)** — Andrej Karpathy (2025)
  - Why watch: The gold standard for understanding how data is filtered, how pre-training transforms the internet into a "probabilistic zip file," and how post-training instills an assistant persona.
  - Key theory: Tokenization mechanics, context window realities, and why LLMs lack true "working memory" outside their active chat history.
- **[How I use LLMs](https://www.youtube.com/watch?v=EWvNQjAaOHw)** — Andrej Karpathy (2025)
  - Why watch: A practical lecture visualizing the conversation as a one-dimensional stream of tokens where user and assistant take turns writing.
  - Key theory: How clicking "new chat" wipes the token buffer, and how the model strictly uses probability to guess the next text block.

## Phase 2: Agent Design Patterns & Workflows
Once you understand the single API call, learn the architectural frameworks used to link multiple calls together autonomously.

- **[What's next for AI agentic workflows](https://www.youtube.com/watch?v=sal78ACtGTc)** — ft. Andrew Ng (DeepLearning.AI)
  - Why watch: The foundational lecture that popularized the industry-standard agentic design taxonomy.
  - Key theory: The 4 Key Agentic Design Patterns — Reflection (self-correction), Tool Use (APIs/Bash), Planning (subtask decomposition), Multi-Agent Collaboration.
- **[Learn to build effective Agentic AI systems](https://www.youtube.com/watch?v=w7vqXL4PWEE)** — Andrew Ng (2025)
  - Why watch: Focuses on vendor-neutral system design using raw programming languages rather than abstract libraries.
  - Key theory: Disciplined "error analysis" when an agent fails a task, and how to programmatically structure iterative correction loops.

## Phase 3: Technical Implementation Workshops
These long-form technical builds map directly onto the Part 2 "FixerBot" project plan below.

- **[Agentic AI Engineering: Complete 4-Hour Workshop](https://www.youtube.com/watch?v=LSk5KaEGVk4)** (2025)
  - Why watch: A massive, code-first architectural deep dive explaining how frameworks abstract direct LLM interactions.
  - Key theory: Deep coverage of Anthropic's Model Context Protocol (MCP) — a "universal USB-C for AI" tool connection standard.
- **[Agentic AI Tutorial for Beginners | LangGraph Tutorial](https://www.youtube.com/watch?v=CnXdddeZ4tQ)** (2025)
  - Why watch: Transitions you from linear chains into graph-based state machines.
  - Key theory: State orchestration — how an agent manages complex loops, handles retries, maintains persistent memory across state changes, and decides when to branch into a sub-agent.

## Phase 4: Production Scale & Human Control
The mental shift required to manage software execution at enterprise scale.

- **[Andrej Karpathy's AI Agent Blueprint! 10 Principles!](https://www.youtube.com/watch?v=QOndW72vrYo)** (2026)
  - Why watch: Explores the paradigm shift from writing code manually to orchestrating autonomous agents.
  - Key theory: Moving to macro-actions, using markdown files as functional code configurations for agents, and treating persistent looping setups as standalone production infrastructure.
- **The Right Way To Use Coding Agents** — Andrej Karpathy (2026)
  - Why watch: Explains operational workflows needed to safely deploy code agents.
  - Key theory: Defining deterministic verification methods before implementation, isolating the execution space, and forcing agents to commit changes in tiny, test-backed checkpoints.

### Self-assessment: which phase to start from
- **Phase 1** — I need to master LLM core token mechanics first
- **Phase 2 & 3** — I understand LLMs; show me how to build the loops
- **Phase 4** — I want to focus on production architectures and safety

---

# Part 2: Mini Project Plan — "FixerBot"

To learn agentic programming fundamentals, build the core infrastructure yourself using raw LLM APIs before relying on high-level frameworks like LangChain or CrewAI. This ensures you understand how state management, tool calling, and correction loops actually work under the hood.

**The Project:** Build a local CLI agent that takes a broken Python file, reads the error, runs a local test suite (pytest), reads the failure logs, rewrites the code, and loops until the tests pass.

## Week 1: Core LLM Fundamentals & State Management
**Goal:** Learn token mechanics, structured outputs, and how to maintain conversation history.

Concepts to learn:
- System prompts vs. user prompts — setting agent persona, rules, and behavioral boundaries
- Context window management — tracking token usage and pruning chat history so the agent doesn't run out of memory
- JSON mode / structured output — forcing the LLM to reply in a strict schema (e.g., separating "Thoughts" from "Code execution")

Action items:
1. Set up an account and API key with an LLM provider (Anthropic Claude API or OpenAI API).
2. Write a Python script that maintains an active list of messages (`[{"role": "user", "content": "..."}]`) to simulate a chat history.
3. Experiment with temperature settings (use 0.0 for coding/deterministic tasks).

## Week 2: Tool Calling & The ReAct Pattern
**Goal:** Learn how an LLM interacts with the real world using the Reason-Action (ReAct) loop.

Concepts to learn:
- Function calling API — defining Python functions as JSON schemas so the LLM knows when and how to invoke them
- The execution loop — pausing the LLM response when a tool is requested, running the local Python code, feeding the result back into the chat history, and resuming the LLM

Action items:
1. Write two local Python functions: `read_file(path)` and `write_file(path, content)`.
2. Define these functions as tools in your API call payload.
3. Prompt your agent to look at a file and append a specific comment to it, forcing it to successfully use `read_file` then `write_file`.

## Week 3: Sandbox Isolation & The Verification Loop
**Goal:** Run external processes safely and parse terminal errors back into the LLM context.

Concepts to learn:
- Subprocess execution — using Python's `subprocess` module to programmatically execute bash/terminal commands
- Sandboxing basics — isolating file execution so the LLM doesn't accidentally run malicious or destructive code on your host machine
- Error extraction — catching stderr streams to pass compilation or runtime errors back to the AI

Action items:
1. Write a third tool: `run_tests(file_path)` which triggers pytest or `python -m unittest` on a target file via subprocess.
2. Create a deliberately broken Python file (e.g., a syntax error or a failing math function) alongside a simple test file.
3. Build logic that catches the string output of the failed test terminal log.

## Week 4: The Self-Correction Loop & Orchestration
**Goal:** Stitch everything together into an autonomous, self-healing loop with a hard token/financial budget.

Concepts to learn:
- Infinite loop protection — setting max iteration counters (`max_loops = 5`) and token spend ceilings so the agent doesn't burn money if stuck
- Self-correction prompting — constructing the loop context so the LLM evaluates why its previous attempt failed and modifies its next approach

Action items:
1. Build the master loop: prompt the agent to "Make the tests pass for calculation.py."
2. Loop 1: agent reads file → runs tests → tests fail.
3. Loop 2: agent receives the failure logs → reasons about the fix → edits the file → runs tests again.
4. Exit condition: the loop terminates immediately when `run_tests` returns an exit code of 0 (success) or hits your `max_loops` limit.

### Getting started on Week 1
To get moving, decide:
- What programming language for your agent harness? (Python is highly recommended)
- Which LLM provider API do you currently have access to or plan to use?

Then get minimal boilerplate code for the first script talking to the API.

---

# Part 3: Reference Doc — Spotify "Honk" Production Case Study

Spotify's "Honk" AI agent succeeded in merging over 1,500 production Pull Requests (PRs) because it was built as a thin AI wrapper on top of 15 years of strict internal platform infrastructure. It is not a standalone magical AI — it's a sandboxed automation pipeline that couples [Anthropic's Claude Code](https://code.claude.com/) with Spotify's massive distributed systems.

## How Spotify Built Honk
Spotify did not just give an AI model access to their repositories. Instead, they placed Claude inside a strict Observe-Reason-Act loop integrated into their existing DevOps ecosystem.

```
[Developer Prompt via Slack]
       │
       ▼
[Kubernetes Pod / Honk Harness] ──(Runs Claude Agent SDK)
       │
       ├─► [Backstage Catalog] ──── (Fetches Repo Context & Metadata)
       ├─► [Fleetshift/Fleet Mgmt] ─ (Orchestrates Changes Across Thousands of Repos)
       │
       ▼
[Verification Loop (MCP Tools)] ─── (Runs Format, Lint, Compile, Test)
       │
       ├─► [Success] ──► Opens Verified Production PR
       └─► [Failure] ──► Feeds Errors Back to Claude (Up to 10 Turns/3 Retries)
```

### 1. The Core AI Agent Loop
Honk runs inside sandboxed, ephemeral Kubernetes Jobs. It leverages the Claude Agent SDK to prompt the LLM. Rather than granting sweeping system access, Honk provides Claude with exactly three primary tools via a strict Model Context Protocol (MCP) connection:

- **Git** — to create branches and commit changes
- **Bash allowlist** — highly restricted terminal commands for file manipulation
- **Verify** — a custom tool to execute testing pipelines

### 2. The Verification Loop (Self-Correction)
The agent operates on a strict budget (typically a maximum of 10 loops and 3 compilation retries per task). If Claude edits code and the verify command fails (e.g., a linter error or a broken unit test), the exact compiler or test error is fed directly back into Claude's prompt context. The agent reads its own error, refactors the code, and tests it again. A PR is generated only if the verification loop completely passes with a "green build."

### 3. Leveraging Pre-Existing Infrastructure
Honk's massive scale (scaling from 1,500 PRs to nearly 1,000 merged PRs every 10 days) relies on internal platforms Spotify built long before the LLM boom:

- **Backstage** — Spotify's open-source developer portal. Catalogs ownership, dependencies, and metadata across thousands of microservices, feeding structured code context to the agent.
- **Fleet Management ("Fleetshift")** — a tool built to orchestrate deterministic code transformations across thousands of codebases simultaneously. Honk replaces rigid scripts with LLM flexibility to execute large-scale framework migrations, configuration upgrades, and language updates across the entire company fleet.

## How to Learn to Do This Yourself
You do not need Spotify's multi-million dollar budget to build or learn agentic software engineering. Four progressive stages, mirroring the FixerBot project plan in Part 2:

### Step 1: Master Code Isolation & Sandbox Environments
An AI agent can easily corrupt a local environment if left unchecked. Learn to containerize workloads.
- What to do: Learn [Docker](https://www.docker.com/) and containerization fundamentals.
- Practice project: Write a local Python script that can launch an isolated Docker container, execute a simple command line task, and pull the logs back out to your main machine.

### Step 2: Build an Observe-Reason-Act (ReAct) Loop
Instead of asking an LLM to generate code in a single response, learn to program an agentic loop.
- What to do: Learn [Anthropic's TypeScript/Python SDKs](https://github.com/anthropics) or open-source frameworks like [LangGraph](https://www.langchain.com/langgraph) or [CrewAI](https://www.crewai.com/).
- Practice project: Create a script where an LLM is given access to a local file reader and file writer tool. Prompt it to fix a broken function in a local file.

### Step 3: Implement the Verification Feedback Loop
This is the secret sauce that allowed Spotify to scale safely.
- What to do: Build an automatic execution pipeline using Python's `subprocess` library.
- Practice project: Point your agent at a file with an active test harness (like pytest or Jest). Write logic that automatically intercepts terminal errors from failed tests and appends them to a new prompt: "The test failed with this error: [X]. Please fix your previous code change and try again." Set a hard loop limit (e.g., maximum 5 attempts).

### Step 4: Explore Enterprise Architecture Foundations
To handle massive scale across multiple repositories, study distributed state management and standardized software catalogs.
- What to do: Deploy an open-source instance of [Backstage](https://backstage.io/) to understand how modern engineering organizations index codebases.
- Next steps: Familiarize yourself with the Model Context Protocol (MCP) specification to see how modern agents securely plug into external development tools, databases, and enterprise file structures.

### Options for a mini-version of Honk
- A Python boilerplate script for an autonomous testing agent loop
- An explanation of how to configure an MCP server for local file edits
- A breakdown of how to structure a prompt for code self-correction

## Sources
- [ninetwothree.co — Honk Spotify AI Infrastructure](https://www.ninetwothree.co/blog/honk-spotify-ai-infrastructure)
- [medium.com — How Spotify built an AI coding agent that merged 1,500 PRs](https://medium.com/codetodeploy/how-spotify-built-an-ai-coding-agent-that-merged-1-500-prs-6e913b9b4ca5)
- [contextstudios.ai — Spotify built an AI coding agent that replaced writing code](https://www.contextstudios.ai/blog/spotify-built-an-ai-coding-agent-that-replaced-writing-code-heres-what-developers-need-to-know)
- [rywalker.com — Spotify Honk research](https://rywalker.com/research/spotify-honk)
- [backstage.spotify.com — How Spotify built Honk](https://backstage.spotify.com/how-spotify-built-honk)
- [backstage.spotify.com — Fleetshift](https://backstage.spotify.com/fleetshift)
- [engineering.atspotify.com — Spotify's background coding agent, Part 1](https://engineering.atspotify.com/2025/11/spotifys-background-coding-agent-part-1)
- [infoq.com — Spotify Honk rewrite](https://www.infoq.com/news/2026/03/spotify-honk-rewrite/)
- [blog.stackademic.com — Loop engineering](https://blog.stackademic.com/loop-engineering-is-the-hottest-skill-in-ai-right-now-heres-what-it-actually-means-de5bb85636f3)
- [medium.com — Spotify's senior devs haven't written code since December](https://medium.com/@krupeshraut/spotifys-senior-devs-haven-t-written-code-since-december-9a6dba74b988)
