# SRE / Infrastructure Engineer — Interview Prep Plan

> **Target role:** Backend Software Engineer – Infrastructure (London)
> **Approach:** Option A (interview-ready sprint) + Option C (long-game capability build)
> **Anchor project:** [dockersamples/example-voting-app](https://github.com/dockersamples/example-voting-app)

---

## Contents

- [The interview process](#the-interview-process)
- [Learning rules — how to use Claude](#learning-rules--how-to-use-claude)
- [Claude usage playbook](#claude-usage-playbook)
- [Tech stack map](#tech-stack-map)
- [Project phases](#project-phases)
  - [Phase 0 — Orient and understand](#phase-0--weeks-1-orient--understand)
  - [Phase 1 — Modify and build](#phase-1--weeks-23-modify--build)
  - [Phase 2 — Add observability](#phase-2--weeks-46-add-observability)
  - [Phase 3 — Swap Redis for Kafka](#phase-3--weeks-79-swap-redis-for-kafka)
  - [Phase 4 — Harden and ship](#phase-4--weeks-1012-harden--ship)
- [Python practice track](#python-practice-track)
- [Why \[company\] framework](#why-company-framework)
- [Interview story bank reference](#interview-story-bank-reference)

---

## The interview process

| Stage | Format | What they're testing |
|---|---|---|
| Recruiter screen | 30–45 min call | Mission alignment, Foundry vs Gotham knowledge, "Why [company]?" |
| Online assessment | 60–90 min HackerRank | Coding (multi-part mini-projects), SQL, sometimes API integration |
| Onsite: Decomposition | 60 min CodePair | Break a vague problem into subproblems, data models, API contracts |
| Onsite: Re-engineering | 60 min CodePair | Find and fix a bug in 200–500 lines of unfamiliar code |
| Onsite: Learning | 60 min CodePair | Use an unfamiliar API/language with minimal docs — learn on the spot |
| Onsite: System design | 60 min whiteboard | Distributed systems at scale, fault tolerance, trade-off reasoning |
| Hiring manager final | 60 min | Deep dive on past projects, mission fit, revisit gaps from earlier rounds |

**Key insight:** The decomposition and learning rounds are uniquely theirs. They are not testing what you know — they are testing how you think when you don't know. Your ability to use Claude to rapidly learn unfamiliar stacks is *directly* what these rounds reward, provided you have genuinely internalised what you learned.

---

## Learning rules — how to use Claude

These rules keep you on the right side of "used AI to learn fast" vs "used AI as a crutch."

**Rule 1 — Concept before code**
Never ask Claude to generate code for a technology you don't yet understand conceptually. First ask for an explanation and a restaurant analogy. Then write the code yourself.

**Rule 2 — Always explain what you got**
After Claude generates any code: read every line and add a comment explaining what it does in your own words. If you can't comment a line, you don't understand it. Ask Claude to explain it. Never ship code you can't explain.

**Rule 3 — Active recall, not passive reading**
After every Claude explanation session, close the chat and write a 3-sentence summary of what you learned from memory. Then compare to the conversation. The gap is your learning gap.

**Rule 4 — Break things intentionally**
For every service you add, ask Claude for its 3 most likely failure modes. Then reproduce them. You learn more from a controlled failure you understand than from code that mysteriously works.

**Rule 5 — Interview framing throughout**
At the end of every phase, ask Claude to conduct a 10-minute technical interview about what you just built. This tells you exactly what you know confidently vs what you'd struggle with in a real interview.

**Rule 6 — Use LLM usage as a signal, not a crutch**
In interviews, you can say: *"I used Claude to learn Kafka quickly and built a producer-consumer system from scratch."* That's a strength. But you must be able to explain every decision independently. Claude accelerates the journey — it doesn't replace it.

---

## Claude usage playbook

### Basic features — use daily

| Mode | How to use it |
|---|---|
| **Explain + analogy** | Paste any unfamiliar code and ask: *"Explain this using a restaurant analogy."* Forces translation into your mental model. |
| **Code review** | After writing code: *"Review this like a senior engineer in a PR. What's not production-quality?"* |
| **Rubber duck** | Before writing code: *"I'm going to implement X. Here's my plan. What's wrong with my approach?"* |

### Advanced features — use for deep learning

| Mode | How to use it |
|---|---|
| **Socratic mode** | After building something: *"Quiz me on what I just built. Ask 3 questions an interviewer would ask. Wait for my answers."* |
| **Failure mode analysis** | *"Suggest 3 ways this code could fail in production."* Then reproduce each failure. |
| **Deep research** | Enable the deep research toggle for distributed systems concepts — synthesised multi-source, not just one perspective. |
| **Architecture diagrams** | Ask Claude to generate interactive visual diagrams of your system at each phase. Use these in your README. |
| **Interview simulation** | *"Conduct a 20-minute technical interview about this project. Wait for my answers before continuing."* |
| **SRE framing** | After each phase: *"How would an SRE describe this system in terms of SLIs, SLOs, error budget, and failure modes?"* |
| **Comprehension check** | When reading unfamiliar code: *"What questions should I be able to answer about this file before moving on?"* |

### Ready-made prompts — copy and paste

```
Explain how the voting app worker service reads from Redis and writes to Postgres, using a restaurant kitchen analogy.
```

```
I just wrote this Python function. Review it like a senior engineer in a code review. Be specific about what is not production-quality.
```

```
Quiz me on what I just built in the voting app. Ask me 3 questions an interviewer would ask about my architectural decisions. Wait for each answer before continuing.
```

```
Suggest 3 ways my current voting app code could fail in production — be specific about the failure mode, not just vague warnings.
```

```
Teach me Kafka from first principles using the restaurant kitchen analogy. Cover: topics, partitions, consumer groups, and offsets. Then quiz me.
```

```
What SLIs would you define for the voting app? What SLO would be reasonable? Help me think through this like an SRE interviewer would.
```

```
Conduct a 20-minute technical deep-dive interview about my voting app project. Ask me about every major architectural decision. Wait for my answers.
```

```
I am reading this code for the first time. What questions should I be able to answer about it before moving on?
```

---

## Tech stack map

### Frontend + API layer

<details>
<summary><strong>Flask</strong> — Python web framework for the vote UI</summary>

**What it does:** Handles HTTP routes, renders HTML templates, reads form submissions from the browser.

**First-principles prompt:**
```
Teach me Flask from first principles. I have never used it.
Use the restaurant analogy — what is a route, a request, a response context?
```

**Key concepts to understand:**
- Route decorators (`@app.route`)
- Request object (`request.form`, `request.json`)
- Response and status codes
- Jinja2 templates
- Application context vs request context

</details>

<details>
<summary><strong>HTTP + Gunicorn</strong> — Request handling, WSGI server</summary>

**What it does:** Gunicorn is the production-grade server that sits in front of Flask and handles concurrent requests. Flask's built-in server is for development only.

**First-principles prompt:**
```
Teach me how HTTP request/response works in the context of a Python web app.
What happens between a browser click and a Flask function returning a response?
What is WSGI and why does Gunicorn exist?
```

</details>

<details>
<summary><strong>Node.js + Express</strong> — Result service backend</summary>

**What it does:** The result service is written in Node.js. It reads vote totals from Postgres and pushes live updates to the browser via Socket.IO.

**First-principles prompt:**
```
Teach me Node.js and Express from first principles.
What is the event loop? How does it handle concurrent requests differently from Python?
What is Socket.IO and why is it used for live updates?
```

</details>

---

### Message queue layer

<details>
<summary><strong>Redis</strong> — In-memory queue (Phase 0–2)</summary>

**What it does:** Acts as a fast message queue. The vote service pushes votes into a Redis list. The worker service pops them off and writes to Postgres.

**First-principles prompt:**
```
Teach me Redis from first principles using a restaurant analogy.
What is it, why is it in-memory, what happens on restart,
and why is it used as a queue here rather than a database?
```

**Key concepts:**
- In-memory data structure store
- `LPUSH` / `BRPOP` for queue operations
- TTL and expiry
- What happens when Redis restarts (data loss unless persistence configured)
- Why it's fast (everything in RAM, single-threaded event loop)

</details>

<details>
<summary><strong>Kafka</strong> — Distributed queue (Phase 3+)</summary>

**What it does:** A fault-tolerant, horizontally scalable message streaming platform. Replaces Redis when you need durability, replay, and high throughput.

**First-principles prompt:**
```
Teach me Kafka from first principles using a restaurant kitchen analogy.
Cover topics, partitions, consumer groups, offsets, and why you would use it instead of Redis.
```

**Key concepts:**
- Topics vs queues (Kafka retains messages; Redis destroys them on pop)
- Partitions — parallelism unit
- Consumer groups — multiple workers sharing load
- Offsets — position tracking, replay capability
- Why Kafka beats Redis at scale: durability, replay, backpressure handling

**Interview question to be ready for:**
> "Why did you replace Redis with Kafka? What specific problem were you solving?"

</details>

---

### Data persistence layer

<details>
<summary><strong>PostgreSQL</strong> — Persistent vote storage</summary>

**What it does:** Stores final vote counts durably. Unlike Redis, data survives restarts.

**First-principles prompt:**
```
Teach me PostgreSQL from first principles.
What are ACID guarantees, how does connection pooling work,
and why does this app use it for vote results instead of Redis?
```

**Key concepts:**
- ACID (Atomicity, Consistency, Isolation, Durability)
- Why not just use Redis for everything (Redis is volatile by default)
- Connection pooling — why opening a new connection per request is expensive
- Transactions and when to use them

</details>

<details>
<summary><strong>psycopg2</strong> — Python Postgres client</summary>

**What it does:** The Python library that connects to PostgreSQL, executes queries, and handles transactions.

**First-principles prompt:**
```
Explain psycopg2, the Python PostgreSQL client library.
How do I open a connection, execute a query, handle a transaction, and close safely?
What is connection pooling and should I use it here?
```

</details>

---

### Observability layer

<details>
<summary><strong>Prometheus</strong> — Metrics collection</summary>

**What it does:** Scrapes metrics endpoints from your services on a schedule and stores them as time-series data.

**First-principles prompt:**
```
Teach me Prometheus from first principles.
What is a pull model, what is a metric type (counter, gauge, histogram),
and how do I add it to my Python service?
```

**Key metric types:**
- `Counter` — only goes up (total votes processed)
- `Gauge` — can go up or down (current queue depth)
- `Histogram` — distribution of values (request latency buckets)

**SRE framing for interviews:**
- Counter → use for SLI calculations (error rate = errors / total requests)
- Gauge → use for capacity signals
- Histogram → use for latency SLOs (p50, p95, p99)

</details>

<details>
<summary><strong>Grafana</strong> — Metrics visualisation</summary>

**What it does:** Connects to Prometheus as a data source and renders dashboards.

**First-principles prompt:**
```
Teach me Grafana from first principles.
How does it connect to Prometheus? What is a panel, what is PromQL,
and how do I build a useful dashboard for a distributed system?
```

**Panels to build for this project:**
1. Vote rate (votes per second) — Counter rate
2. Kafka consumer lag — Gauge
3. Worker error rate — Counter rate
4. Worker processing latency — Histogram p95

</details>

---

### Infrastructure + orchestration

<details>
<summary><strong>Docker + Docker Compose</strong> — Container runtime and wiring</summary>

**What it does:** Docker packages each service into a container. Compose wires them together with networking and shared volumes.

**First-principles prompt:**
```
Teach me Docker from first principles.
What is a container vs a VM, what is a Dockerfile, what is a layer,
and how does docker compose wire services together with networking?
```

**Key concepts:**
- Image vs container (image is the recipe, container is the running dish)
- Layers — each RUN/COPY instruction adds a layer (cached if unchanged)
- Networks — services in the same compose network can reach each other by service name
- Volumes — persistent storage that survives container restarts

</details>

<details>
<summary><strong>Kubernetes</strong> — Orchestration (Phase 4)</summary>

**What it does:** Manages containers at scale — scheduling, health checks, scaling, service discovery. The voting app repo includes K8s manifests.

**First-principles prompt:**
```
Teach me Kubernetes from first principles — I already use EKS at work
but want to understand the fundamentals behind it.
Cover pods, services, deployments, and why K8s exists vs just using Docker.
```

</details>

<details>
<summary><strong>GitHub Actions</strong> — CI/CD pipeline</summary>

**What it does:** Runs automated checks (lint, test, build) on every push.

**First-principles prompt:**
```
Teach me GitHub Actions from first principles.
What is a workflow, a job, a step, and an action?
How do I write a CI pipeline that lints, tests, and builds Docker images?
```

**Pipeline to build:**
```yaml
# .github/workflows/ci.yml outline
on: [push, pull_request]
jobs:
  lint:    # ruff check .
  test:    # pytest with coverage
  build:   # docker build for each service
```

</details>

---

## Project phases

### Phase 0 — Week 1: Orient & understand

**Goal:** You can explain every data hop from browser click to result page update, without referring to any docs. No code written yet.

<details>
<summary>Tasks</summary>

- [ ] Clone the repo: `git clone https://github.com/dockersamples/example-voting-app`
- [ ] Run `docker compose up` — get it working before reading a single line of code
- [ ] Vote in the UI. Watch the result page update. Ask: what just happened between those two clicks?
- [ ] Open each service folder in order: `vote/`, `worker/`, `result/`. Don't touch anything — just read.
- [ ] Draw the full architecture from memory on paper. Then compare to the actual architecture diagram. Note every gap.
- [ ] For every file you don't understand: paste it to Claude and ask for the restaurant analogy explanation.

**Claude usage this phase:**
- Paste any file and ask: *"Explain this file as if I've never used Flask/Redis/Postgres before, using a restaurant analogy"*
- Ask: *"What question should I be able to answer about this file before moving on?"*
- Ask Claude to generate a visual architecture diagram of the app with clickable components

</details>

---

### Phase 1 — Weeks 2–3: Modify & build

**Goal:** You understand Flask routing, Redis client libraries, and JSON logging well enough to explain every line you wrote.

<details>
<summary>Part A — Add features to the Python vote service</summary>

- [ ] Add a `/history` endpoint — returns the last 10 votes stored in Redis
- [ ] Add a `/health` endpoint — checks Redis connectivity and returns JSON status
- [ ] Add structured JSON logging to every route: timestamp, route, status code, latency
- [ ] Test each endpoint manually with `curl` before moving on

**Claude usage:**
- Rubber duck design before writing: *"I'm going to implement /history. Here's my plan. What's wrong with my approach before I write it?"*
- Code review after writing: *"Review this like a senior engineer would in a PR"*
- Failure modes: *"Suggest 3 ways this /health endpoint could fail in production"*

</details>

<details>
<summary>Part B — Rewrite the .NET worker in Python</summary>

- [ ] Read the existing `.NET` worker line by line — understand what it does before writing a character of Python
- [ ] Write the Python equivalent from scratch — translate the logic yourself, don't copy/paste
- [ ] Add retry logic with exponential backoff when Postgres is unavailable
- [ ] Write 5 unit tests with `pytest` — test the processing logic, not just "does it run"

**Claude usage:**
- Socratic mode after each function: *"Quiz me on what I just wrote — ask me 3 questions an interviewer would ask about these design decisions"*
- Deep research: *"What are the failure modes of polling Redis in a tight loop? How do real systems handle this?"*
- Test generation: *"Write pytest tests for this worker function — include edge cases I probably haven't thought of"*

**Key things to understand before moving on:**
- How `redis-py` client connects and handles reconnection
- What `psycopg2` connection pooling means and why it matters
- What exponential backoff is and when to use it
- How to write a test that doesn't depend on a live Redis or Postgres instance (mocking)

</details>

---

### Phase 2 — Weeks 4–6: Add observability

**Goal:** You can discuss SLIs, SLOs, and observability architecture in an interview using concrete examples from your own project.

<details>
<summary>Tasks</summary>

- [ ] Add `prometheus-client` to your Python worker
- [ ] Expose these metrics: `votes_processed_total` (Counter), `queue_depth` (Gauge), `worker_errors_total` (Counter)
- [ ] Add a Prometheus scrape target in `docker-compose.yml`
- [ ] Stand up Grafana — connect it to Prometheus
- [ ] Build a Grafana dashboard with 3 panels minimum: vote rate, queue depth, worker error rate
- [ ] Write a Python alerting script that polls the metrics endpoint and prints an alert if error rate > 5%
- [ ] Define an SLI and SLO for the voting system (write it in a `SLO.md` file)

**Claude usage:**
- Concept first: *"Explain Prometheus's pull model vs a push model like Datadog — in restaurant terms, which is the waiter and which is the kitchen bell?"*
- Generate Grafana dashboard JSON, then study it before importing — understand every panel
- SRE framing: *"What SLIs would you define for this voting system? What SLO would be reasonable?"*

**Interview question to be ready for:**
> "How did you approach observability for your project? What metrics did you expose and why those specific ones?"

</details>

---

### Phase 3 — Weeks 7–9: Swap Redis for Kafka

**Goal:** You can explain Kafka partitions, consumer groups, and offset management in an interview. You have metrics proving your system is observable.

<details>
<summary>Tasks</summary>

- [ ] Before touching code: understand *why* you'd swap Redis for Kafka — write a one-paragraph answer
- [ ] Add Kafka to `docker-compose.yml` (use `confluentinc/cp-kafka` image)
- [ ] Modify the vote service to produce to a Kafka topic instead of pushing to Redis
- [ ] Modify the worker to consume from Kafka using `confluent-kafka-python`
- [ ] Add Kafka consumer lag as a Gauge metric in Prometheus
- [ ] Add a Grafana panel for consumer lag
- [ ] Write a runbook: *"What to do when the vote worker falls behind Kafka lag"*

**Claude usage:**
- Conceptual deep dive before any code: *"Teach me Kafka from first principles. Use the restaurant kitchen analogy. What is a topic, partition, consumer group, offset?"*
- Architecture diagram: *"Generate a visual diagram of my new Kafka-based voting app architecture"*
- Interview simulation: *"Ask me 5 system design questions about this Kafka migration. Wait for my answers and grade them."*

**Key things to understand before moving on:**
- Why Kafka retains messages (unlike Redis which destroys on pop) — what does this enable?
- What a consumer group is and how multiple workers share load across partitions
- What consumer lag is, how to measure it, and what high lag means operationally
- When you'd use Redis vs Kafka — be able to give a concrete trade-off answer

</details>

---

### Phase 4 — Weeks 10–12: Harden and ship

**Goal:** Public GitHub repo with CI pipeline, deployed system, and a README you can walk an interviewer through for 20 minutes confidently.

<details>
<summary>Tasks</summary>

- [ ] GitHub Actions CI pipeline: lint (`ruff`), test (`pytest`), Docker build on every push
- [ ] Deploy to AWS free tier or GCP — not just Docker on localhost
- [ ] Write a `RUNBOOK.md`: step-by-step response to "vote worker falling behind Kafka lag"
- [ ] Write a `README.md` with: architecture diagram, design decisions, known trade-offs, what you'd do differently
- [ ] Make the repository public — this is your GitHub portfolio piece
- [ ] Start Tour of Go (official tutorial) — 30 min/day, no pressure

**Claude usage:**
- GitHub Actions: *"Generate a CI workflow for this Python project — then explain each step so I can maintain it independently"*
- README review: *"Review my project README like a hiring manager. What's missing? What signals real understanding vs copy-paste?"*
- Final interview simulation: *"Conduct a 20-minute technical interview about this project. Ask me about every architectural decision."*

</details>

---

## Python practice track

### Mamun Mashid-style questions (warm-up)

Work through these question categories in order. Mirror every solution live in your editor.

- String manipulation: palindrome, reverse, count characters
- List operations: deduplicate, sort, find second largest
- Dictionary operations: word count, group by value
- File I/O: read line by line, find matching lines, count words
- Error handling: try/except, write defensive code

### Claude-generated SRE scenarios (main track)

Use these prompts to generate realistic infrastructure scripting questions:

```
Give me a log parsing question. Provide a realistic sample access.log to work from.
I should parse it, find all 5xx errors, and return count by endpoint.
```

```
Give me a file manipulation question. I should update every occurrence of an IP
address in a config file without touching other lines.
```

```
Give me an API automation question using the Star Wars open API (swapi.dev).
I should fetch all starships, filter by crew size, and write results to CSV.
```

```
Give me an incident metrics question. I have a CSV of incidents with timestamps
and severity. I should calculate MTTR per severity level.
```

```
Give me an alerting script question. I should poll a URL every 30 seconds,
log to a file, and print an alert if response time exceeds 500ms or returns non-200.
```

### Production-quality habits (enforce from Week 2)

Every Python script you write should have:

```python
# Type hints on every function signature
def parse_log(filepath: str) -> dict[str, int]:

# Docstring on every function
    """Parse an access log and return error counts by endpoint."""

# Explicit error handling
    try:
        with open(filepath) as f:
            ...
    except FileNotFoundError as e:
        raise ValueError(f"Log file not found: {filepath}") from e

# At least one test in pytest
def test_parse_log_returns_counts():
    result = parse_log("fixtures/sample.log")
    assert result["/api/vote"] == 3
```

---

## Why [company] framework

Use this for any company. Each step has a purpose — don't skip.

**Step 1 — Know the product, not just the name**
What does this company actually build? Who uses it? What problem does it solve at scale? If you can't explain it in two sentences without jargon, do more research.
- Research sources: company blog, product pages, case studies, named customers
- For this company specifically: understand Foundry vs Gotham — they are distinct platforms, not synonyms. Mixing them up ends the recruiter call.

**Step 2 — Find your genuine connection**
What about the mission actually resonates with you? This must be real — recruiters spot rehearsed enthusiasm immediately.
- Ask yourself: do I care about the domains this company operates in? What draws me to that specifically?

**Step 3 — Link it to your background**
Why you specifically, not just anyone? What in your experience makes you the right person?
- Your Nationwide work: you've operated infrastructure where failures have real consumer consequences (payments, CoP service). That's closer to mission-critical than most junior candidates can claim.

**Step 4 — State what you want to build next**
What capability do you want to develop that this role specifically enables?
- Be concrete: "I want to build systems that operate at 10x the scale I've encountered at Nationwide — distributed systems, not just infrastructure operations."

**Step 5 — Show you've thought about the hard parts**
This company operates in sensitive domains. Have you thought about that? You don't need a political position — you need to show you're not naive.
- Acknowledge the dual-use nature of the technology. You don't have to resolve the tension — but you must show you've sat with it.

### Research checklist before any recruiter call

- [ ] Read the company's engineering blog — at least 3 posts
- [ ] Know 2–3 named customers and what they use the product for
- [ ] Understand the specific team you're interviewing for (infrastructure, not just "software")
- [ ] Know one recent company announcement or product release
- [ ] Have a genuine view on why their approach to the problem is interesting or unusual

---

## Interview story bank reference

> Full story bank is in your CV Story Bank markdown file. Below is a quick-reference mapping of which story to use for which question type.

| Interview question | Lead story | Backup story |
|---|---|---|
| Describe a complex incident you diagnosed | P2 EKS/Apigee certificate rebuild | Calico-apiserver crashloop |
| Tell me about a time you reduced toil | F5 load balancer migration (2hr saved/delivery) | Dynatrace extension automation |
| Tell me about cross-team coordination | P2 incident (Google Apigee, F5, release manager) | Dynatrace operator imagepullbackoff |
| What's your proudest accomplishment? | F5 traffic switching redesign | Dynatrace 10-month automation initiative |
| Walk me through your Linux experience | P2 log hunting (Nginx access logs) | Integrated channel 401/500 diagnosis |
| What is your Kubernetes experience? | Traefik ingress migration | Dynatrace operator imagepullbackoff |
| How do you handle on-call you can't immediately solve? | Calico-apiserver: zero business impact, documented + closed | Missing bastion node escalation |
| Tell me about your CI/CD experience | CloudBees Jenkins credential fix | Dynatrace extension pipeline automation |

---

*Last updated: June 2026*
*Roles targeted: SRE, Infrastructure Engineer, Backend Software Engineer – Infrastructure*
