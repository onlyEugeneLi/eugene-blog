Use this workbook as a step-by-step starter plan. It is written for a **graduate SRE** who wants to deploy a first internal LLM MVP and grow toward **AI engineer** capability.

# Workbook: First Internal LLM MVP for a Graduate SRE

## Mission
Build a **small-scale internal AI assistant** for:
- new joiner induction
- team/system/component/architecture Q&A from Confluence docs
- internal policy Q&A with citations

Target approach:
- **self-hosted**
- **small model**: `Qwen2.5 7B`
- **RAG-first**, not fine-tuning-first
- **one cloud GPU**
- **FastAPI + vLLM + pgvector**
- **simple UI or Slack/Teams bot**

---

# 1. What you are building

## Product goal
A user asks:
- “How does the payments platform connect to the risk service?”
- “What is the onboarding exception process?”
- “What approvals are required for wire escalation?”
- “Summarize this policy update.”

System:
1. retrieves relevant approved internal documents
2. sends the retrieved context to the model
3. generates an answer
4. includes citations/snippets
5. logs the interaction for review

## Success criteria
Your MVP is successful if:
- users get useful answers from internal docs
- answers cite source material
- system is safe for internal pilot use
- latency is acceptable
- cost is understandable
- you can operate it reliably

---

# 2. What you need to know at a practical level

## Role map: simple summary

### A. AI/ML engineer or strong backend engineer
What they do:
- choose the model
- set up model serving
- build retrieval and prompt flow
- evaluate response quality
- improve prompt templates
- optimize inference/cost

### B. App/backend engineer
What they do:
- build API endpoints
- connect UI/bot to backend
- implement auth and session logic
- handle document ingestion jobs
- integrate storage and DB

### C. Part-time security/compliance reviewer
What they do:
- define what data is allowed
- review logging/redaction rules
- review access control and retention
- approve pilot boundaries

### D. Business owner
What they do:
- define the use case
- provide source documents
- define what “good” answers look like
- recruit pilot users
- validate business value

## As a graduate SRE, your practical ownership
You can lead the MVP if you focus on:
- infrastructure setup
- deployment
- observability
- secure access
- API/runtime reliability
- basic RAG integration
- iterative testing

---

# 3. The MVP architecture

## Minimal architecture
- **Cloud GPU VM**: run the model
- **vLLM**: serve Qwen2.5 7B
- **FastAPI**: backend orchestration
- **PostgreSQL + pgvector**: document chunk embeddings and retrieval
- **Object storage**: raw docs and processed artifacts
- **Simple UI**: web chat or Slack/Teams bot
- **SSO / company auth proxy**
- **Basic monitoring/logging**

## Simple flow
1. docs exported from Confluence
2. docs stored in object storage
3. ingestion job cleans/chunks docs
4. embeddings generated and stored in pgvector
5. user asks question
6. backend retrieves top matching chunks
7. backend builds prompt
8. vLLM serves model response
9. answer returned with citations
10. prompt/response metadata logged

---

# 4. Why this stack

## Model: Qwen2.5 7B
Why:
- strong general-purpose instruction performance
- manageable for first deployment
- good enough for internal Q&A MVP
- cheaper and easier than large models

## vLLM
Why:
- good inference serving
- widely used
- efficient for open model serving
- easier than building raw inference yourself

## FastAPI
Why:
- easy Python backend
- fast to prototype
- simple REST integration
- popular ecosystem

## PostgreSQL + pgvector
Why:
- one familiar database
- easy to operate
- enough for MVP retrieval
- avoids introducing too many moving parts

## Object storage
Why:
- simple document staging
- scalable enough for pilot
- supports raw + processed document lifecycle

---

# 5. Skills checklist for the graduate SRE

Rate yourself 1–5 on each.

## Infrastructure
- Linux administration
- Docker basics
- cloud VM provisioning
- networking/security groups
- storage mounting and usage
- DNS / reverse proxy basics

## App/platform
- Python basics
- REST APIs
- JSON handling
- environment variables / secrets
- Git workflows
- debugging services

## Data/LLM
- what an embedding is
- what chunking is
- what RAG is
- prompt construction basics
- model context window basics
- hallucination and retrieval failure modes

## Ops
- logs
- metrics
- alerting
- uptime basics
- incident handling
- rollback thinking

If you are weak in some of these, that is fine. This project is how you learn.

---

# 6. Scope control: what NOT to do first

Do not start with:
- fine-tuning
- agents doing many autonomous actions
- multi-model routing
- Kubernetes
- huge enterprise UI
- multi-region setup
- complex document permissions inheritance
- broad company-wide rollout
- any production claims

First goal:
**one working internal pilot for one approved document set**

---

# 7. Project phases

## Phase 0: define the pilot
Duration: 2–3 days

### Output
- one use case
- one document corpus
- one user group
- one model
- one success definition

### Your checklist
- [ ] define pilot users
- [ ] define allowed data class
- [ ] define approved document source
- [ ] define top 10 example questions
- [ ] define “good answer” criteria
- [ ] define who signs off on pilot

### Example
Use case:
- new joiner induction + internal policy Q&A

Users:
- 10–20 internal pilot users

Docs:
- approved Confluence exports only

Model:
- Qwen2.5 7B

Success:
- 70%+ of pilot queries are rated useful by users

---

## Phase 1: deploy the model
Duration: 3–5 days

### Goal
Run `Qwen2.5 7B` on one GPU VM behind an internal API.

### Deliverables
- GPU VM provisioned
- model served via vLLM
- health endpoint works
- basic inference test works

### What you learn
- model serving
- GPU runtime basics
- service management
- latency observation

### Checklist
- [ ] provision cloud GPU VM
- [ ] secure inbound access
- [ ] install Python/Docker as needed
- [ ] install vLLM
- [ ] download model
- [ ] expose internal inference endpoint
- [ ] test sample prompt
- [ ] log latency and errors

### Output evidence
- screenshot of successful inference call
- measured latency for 5 test prompts
- note GPU memory usage

---

## Phase 2: build document ingestion
Duration: 4–7 days

### Goal
Convert Confluence content into retrievable chunks.

### Deliverables
- ingestion script
- cleaned text extraction
- chunking pipeline
- document metadata schema
- embeddings stored in pgvector

### What you learn
- document preprocessing
- text chunking
- embeddings
- vector retrieval

### Checklist
- [ ] export approved Confluence pages
- [ ] define metadata fields
- [ ] write parser/cleaner
- [ ] split into chunks
- [ ] generate embeddings
- [ ] store chunks + embeddings in pgvector
- [ ] preserve source URL/title/page ID
- [ ] test retrieval quality

### Suggested metadata
- document_id
- title
- source_url
- page_owner
- doc_type
- last_updated
- chunk_id
- chunk_text
- security_label

---

## Phase 3: build the RAG API
Duration: 4–6 days

### Goal
Create FastAPI endpoints that:
- receive question
- retrieve relevant chunks
- construct prompt
- call vLLM
- return answer with citations

### Deliverables
- `/ask`
- `/health`
- `/feedback`
- prompt template
- citation formatting

### What you learn
- orchestration
- prompt design
- retrieval grounding
- API construction

### Checklist
- [ ] create FastAPI app
- [ ] implement retrieval query
- [ ] build prompt template
- [ ] send request to vLLM endpoint
- [ ] include citations in response
- [ ] add request IDs
- [ ] add structured logs
- [ ] add feedback capture

### Basic answer contract
Response should include:
- answer text
- source citations
- confidence hint or retrieval note
- request ID

---

## Phase 4: build a simple UI
Duration: 3–5 days

### Goal
Enable internal users to ask questions.

### Options
- simplest web chat
- Slack bot
- Teams bot

### Recommendation
Start with:
- **simple internal web UI**
because it gives you easiest debugging and controlled rollout

### Checklist
- [ ] login via internal auth
- [ ] question input box
- [ ] answer panel
- [ ] citations panel
- [ ] feedback buttons
- [ ] copy answer button
- [ ] session history for current user

---

## Phase 5: observability and safety
Duration: 3–5 days

### Goal
Make it operable.

### Deliverables
- request logs
- latency metrics
- error metrics
- GPU utilization visibility
- redaction decision
- retention policy

### Checklist
- [ ] log request metadata
- [ ] track response latency
- [ ] track retrieval time
- [ ] track model generation time
- [ ] dashboard error rate
- [ ] log top cited docs
- [ ] define log retention
- [ ] decide whether prompt content is stored fully or partially
- [ ] define who can access logs

---

# 8. Technical design notes

## RAG prompt pattern
Keep it simple:
- role/instructions
- retrieved context
- citation requirement
- user question

## Example prompt pattern
```text
You are an internal assistant for employee onboarding, team induction, architecture orientation, and policy Q&A.

Answer only using the provided context.
If the answer is not supported by the context, say you do not know.
Cite the source chunk IDs or document titles used.
Be concise and accurate.

Context:
[Chunk 1 | Doc: Payments Architecture Overview | URL: ...]
...

[Chunk 2 | Doc: Onboarding Exceptions Policy | URL: ...]
...

User question:
...
```

## Chunking guidance
For MVP:
- prefer medium chunks, not giant pages
- preserve title hierarchy
- include source metadata
- avoid losing section headings

## Retrieval guidance
For MVP:
- top 5 to top 8 chunks
- reranking optional later
- start simple and measure

---

# 9. Security and governance basics

## Minimum rules for pilot
- only approved internal docs
- no customer confidential data initially
- internal users only
- audit logging enabled
- explicit disclaimer: pilot assistant, may be incorrect
- human verification required for policy-sensitive decisions

## Questions to answer before rollout
- what data can be ingested?
- who can query it?
- what logs are stored?
- where are model and embeddings hosted?
- what retention period applies?
- how are access revocations handled?

## Mandatory user warning
Add this to UI:
- answers may be incomplete or incorrect
- verify with official policy/source documents
- do not use as sole basis for regulated decisions

---

# 10. Evaluation workbook

## Create a small evaluation set
Build 30–50 questions:
- 10 onboarding questions
- 10 architecture/system questions
- 10 policy questions
- 10 hard/ambiguous edge cases

## Evaluate these dimensions
- retrieval relevance
- factual correctness
- citation quality
- completeness
- conciseness
- refusal when answer not in context

## Simple scorecard
Score each answer 1–5 on:
- useful
- correct
- grounded in docs
- cites source clearly

## Failure categories
Track when answers fail because of:
- wrong retrieval
- missing docs
- poor chunking
- weak prompt
- model hallucination
- ambiguous question

---

# 11. Suggested cloud MVP bill of materials

## Minimal components
- 1 GPU VM
- 1 app VM or same VM initially
- 1 PostgreSQL instance
- 1 object storage bucket
- 1 reverse proxy / auth layer
- logs + simple dashboard

## Cost control principles
- one model only
- limited pilot users
- shut down nonessential GPU time when idle if possible
- small document corpus first
- avoid overengineering

---

# 12. Your weekly execution plan

## Week 1
- finalize use case and boundaries
- provision cloud environment
- run Qwen2.5 7B on vLLM
- confirm working inference

## Week 2
- build ingestion pipeline
- store embeddings in pgvector
- validate document retrieval

## Week 3
- build FastAPI orchestration
- create RAG prompt flow
- return answers with citations

## Week 4
- build simple UI
- add feedback and logging
- run internal alpha test

## Week 5
- improve retrieval/chunking
- improve prompt template
- create eval set and measure quality

## Week 6
- pilot with selected users
- review incidents/failures
- produce recommendation for next phase

---

# 13. What to put on your resume after this

If you complete this project, you can honestly say things like:
- deployed a self-hosted LLM inference service on cloud GPU infrastructure
- built a RAG pipeline using internal documentation and vector retrieval
- implemented FastAPI orchestration for grounded internal Q&A
- integrated observability, authentication, and pilot-safe governance controls
- evaluated LLM quality using prompt, retrieval, and citation-based metrics

That is real AI engineering foundation.

---

# 14. What “good” looks like for your first project

Good does **not** mean:
- perfect answers
- enterprise-wide launch
- agentic automation
- cutting-edge research

Good means:
- stable deployment
- useful answers
- traceable citations
- manageable cost
- safe pilot scope
- clear learning loop

---

# 15. Personal growth path: SRE to AI engineer

## What you already bring as SRE
- systems thinking
- reliability mindset
- incident/debugging discipline
- automation habit
- infrastructure confidence

## What you need to add
- prompt design
- embeddings/retrieval understanding
- model serving concepts
- LLM evaluation discipline
- product sense for AI use cases

## Your next learning steps after MVP
- reranking
- model quantization
- evaluation frameworks
- guardrails and policy filters
- fine-tuning basics
- multi-model routing
- batch inference
- cost/performance optimization

---

# 16. First deliverable template you can send to your manager

## Project title
Internal AI Assistant MVP for New Joiner Induction and Policy Q&A

## Goal
Deploy a small self-hosted LLM system that answers questions from approved internal Confluence documents with source citations.

## Initial scope
- one model: Qwen2.5 7B
- one cloud GPU instance
- one approved document corpus
- one internal user group
- one simple web UI

## Technical approach
- vLLM for model serving
- FastAPI for orchestration
- PostgreSQL + pgvector for retrieval
- object storage for document ingestion
- RAG with citation-based answers

## Success metrics
- useful answer rate > 70% in pilot
- median latency acceptable for internal use
- citation presence in > 90% of grounded answers
- zero high-severity security/compliance issues in pilot scope

## Risks
- retrieval quality may limit answer quality
- internal documents may be inconsistent
- model may hallucinate when context is weak
- governance and logging decisions must be reviewed early

## Timeline
6 weeks for pilot MVP

---

# 17. Daily operating checklist

Use this every day.

## System health
- [ ] model server up
- [ ] API healthy
- [ ] DB healthy
- [ ] object storage reachable
- [ ] latency normal
- [ ] error rate normal

## Data health
- [ ] ingestion jobs succeeded
- [ ] embeddings created correctly
- [ ] citations resolve to valid source docs
- [ ] no unauthorized docs ingested

## User quality
- [ ] review negative feedback
- [ ] inspect poor answers
- [ ] categorize failures
- [ ] improve prompt/retrieval if needed

---

# 18. Final advice to the graduate SRE

Your first AI project should be:
- **small**
- **grounded**
- **observable**
- **safe**
- **useful**

Do not try to impress with scale.
Impress with:
- working deployment
- disciplined scope
- clear citations
- measurable value
- good operational hygiene

If you want, I can turn this into either:
1. a **Notion-style checklist workbook**,  
2. a **30-60-90 day learning plan**, or  
3. a **hands-on implementation guide with folder structure, APIs, and sample FastAPI endpoints**.