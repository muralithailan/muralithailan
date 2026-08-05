# Balamurali Thailan

**Lead AI Engineer — agent platforms and the runtimes behind them.** Bengaluru, India.

I build the platform other teams build agents on.

At **Target** I lead a multi-tenant AI platform where engineering teams across the company author, deploy and
operate their own agents. They get a shared runtime for orchestration and state, a typed tool and knowledge
layer, distributed tracing, and offline plus online evaluation wired into the release path. Teams ship agents;
they don't ship agent infrastructure.

The platform carries **100k+ conversations a day** across tenants with very different problems, including agents
that execute consequential, money-moving actions — **zero financial-loss incidents across two million of them**.
Before agents, twelve years of distributed systems: Java, Kotlin, Kafka, event-driven services at high volume.
That background is most of why the platform stays up.

---

### What I've learned building it

**Consent that the caller can assert is not consent.**
An action tool refused unless `confirmed = true`, and told the model to ask the user first. On a live run the
model called it `false`, read the refusal, and immediately called it again with `true` — authorising a
consequential action with no question ever put to anybody. The fix wasn't a better prompt. Authorisation moved
out of the tool's parameters and into the runtime, where the evidence is something the model cannot fabricate:
whether a *user message* actually arrived between the offer and the execution.

**Delegation is a tool call, not a subsystem.**
When the orchestrator is just the root node whose tools are delegations to its children, you get one execution
engine instead of two, routing shows up in the same event stream as everything else, and nesting comes free.
A graph subgraph is a code module; it is not a service boundary.

**One conversation, one history — only the declaration swaps.**
On handoff, the message history carries forward untouched. What changes is which goal, rules and tools render
into the model's context. There is no context to transfer, because nothing was ever separated.

**Context is a budget, not a container.**
Progressive and just-in-time loading — retrieval and reference dereferencing on demand instead of stuffing a
preamble — cut per-turn context roughly 83%. Accuracy went up, not down. A lean context is a more accurate one.

**Evaluation is a release gate or it is decoration.**
Graded production traces feed an offline suite that every build has to clear. Most teams ship agents and hope;
the difference between those two sentences is the whole job.

**The database was never the bottleneck.**
Turn latency dropped ~60% by collapsing serial LLM calls, routing continuation turns mechanically by task
ownership instead of asking a classifier, and running the guardrail, retrieval and state-load work in parallel.
Everyone looks at the database first. It is almost never the database.

**A platform is an adoption problem wearing an engineering costume.**
The interesting constraint was never "can this run an agent." It is whether a team that has never met you can
author one, evaluate it, ship it behind a canary, and debug it from a trace at 2am — without asking you anything.

---

### Currently building

**Cadence** — a platform for building, versioning, running and observing conversational agents, and somewhere to
take these ideas further than a production system lets you. A single agent model that both a visual editor and a
code SDK project onto, so authoring in either surface stays reconcilable; delegation as a tool call;
runtime-enforced consent; scenario-based evaluation that works before there is any traffic. TypeScript, and
private for now — happy to walk through the architecture and the decision records.

---

Most of what I build is internal or not yet public. The architecture, the trade-offs and the things that broke
are all fair game in conversation.

**[LinkedIn](https://www.linkedin.com/in/balamurali-thailan/)** · murali.thailan@gmail.com
