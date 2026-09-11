# KERNHELM

## In Plain Language

**KERNHELM is a general security architecture that places a hard authority boundary at the lowest practical enforceable level of the operating system — beneath AI, applications, and ordinary user software.**

Software above that boundary may be extremely capable, but capability alone does not create permission.

A program can request an action. It cannot simply grant itself the authority to perform that action, rewrite the rules constraining it, or become trusted merely because it is intelligent, useful, or already privileged.

The architectural separation is absolute by design:

> **The software asking for an effect is not allowed to be the thing that decides whether it has authority to cause that effect.**

For AI, this creates an important property:

> **The machine does not have to depend entirely on the AI's continued cooperation in order to remain secure.**

KERNHELM separates intelligence from sovereignty.

**Capability is not permission.**

---

## Make Trust Irrelevant

KERNHELM is an experimental kernel-level authority system designed around a simple premise:

> **Privileged effects should not occur merely because a process inherited enough authority to perform them.**

Instead, software should receive narrowly bounded authority for the specific effects it has actually been permitted to perform.

The long-term goal is to replace broad ambient trust with mechanically enforced authority boundaries.

KERNHELM is being developed as part of **OathRune**, an independently built Linux operating-system research project focused on security, performance, user ownership, and the elimination of unnecessary ambient authority.

---

## The Problem

Modern computers routinely grant software broad standing authority.

A process may be able to read files, execute programs, inspect other processes, access devices, communicate over networks, or modify system state simply because of the account, container, namespace, capability set, or privilege level under which it happens to be running.

That creates a recurring security problem:

```text
authority is granted broadly
        ↓
software is expected to behave correctly
        ↓
software is compromised, confused, manipulated, or simply wrong
        ↓
the inherited authority is still real
```

Security systems can reduce this risk through sandboxing, mandatory access controls, privilege separation, capabilities, policy engines, virtualization, and other important mechanisms.

KERNHELM explores a different layer of the problem:

> **What if the requesting process never possessed enough standing authority to make the privileged effect happen by itself?**

---

## The Core Idea

KERNHELM separates:

```text
what wants an effect
```

from:

```text
what has authority to permit that effect
```

Conceptually:

```text
Untrusted Requestor
        │
        ▼
Concrete Proposed Action
        │
        ▼
Governed Authorization Path
        │
        ▼
Bounded Authority
        │
        ▼
Kernel Enforcement
        │
        ├── fits admitted authority → effect may proceed
        │
        └── does not fit            → deny
```

The requesting process does not mint its own authority.

It may propose an action, but the authority needed to perform a governed privileged effect must originate through a separate trusted path.

The enforcement boundary then checks the effect that is actually occurring rather than trusting the requestor's description of what it intended to do.

This creates a simple underlying rule:

> **No software should be trusted to define the limits of its own power.**

---

## Why This Matters for AI Agents

AI makes the ambient-authority problem unusually important because intelligence and authority are normally coupled together when autonomous systems become useful.

An agent becomes more useful as it gains the ability to:

* read and modify files;
* invoke tools;
* execute programs;
* manage applications;
* communicate with services;
* control parts of a computer;
* observe and respond to changing conditions;
* detect and respond to threats;
* carry out long sequences of actions.

But those abilities normally require granting the agent increasingly broad authority and then trusting it to use that authority correctly.

Prompt injection, model error, compromised dependencies, malicious tool output, mistaken reasoning, unexpected software interactions, hostile external systems, or future behaviors that were never anticipated can transform useful authority into dangerous authority.

The common response is therefore to restrict what the agent can do.

KERNHELM explores another possibility:

> **Build a stronger authority boundary underneath increasingly capable software so that usefulness does not require surrendering control of the machine.**

The agent is not required to be the security boundary.

It may reason incorrectly.

It may be manipulated.

It may request an effect that should never occur.

It may become far more capable than the systems that originally surrounded it.

None of those facts automatically create authority.

The kernel boundary still requires valid admitted authority for the governed effect.

That leads to two important distinctions:

> **Intelligence is not authority.**

and:

> **Capability is not permission.**

KERNHELM does not attempt to make an AI safe by requiring it to be incapable.

Instead, it explores whether increasingly capable software can operate inside a system where its ability to reason about an action remains fundamentally separate from its authority to make that action happen.

---

## KERNHELM Is Not AI-Specific

KERNHELM was not originally designed as an AI-safety system.

It is a general security architecture.

That generality is exactly what makes its application to advanced AI unusually powerful.

From the enforcement boundary's perspective, an AI agent, compromised service, malicious script, vulnerable application, remote attacker acting through compromised software, or unexpected userland process ultimately presents the same fundamental question:

> **Does this attempted effect possess admitted authority?**

The wall does not need to determine whether the actor is an AI before enforcing authority.

It governs effects.

This creates a two-sided security model for systems that contain increasingly capable AI.

### Hostile AI Outside the System

An outside AI may attempt to exploit applications, manipulate services, compromise software, deceive an internal agent, abuse exposed interfaces, or otherwise cause effects on the machine.

KERNHELM does not depend on recognizing the attacker as an AI.

If the attack reaches software above the authority wall, the resulting governed effects remain subject to the same authority boundary.

Compromising or manipulating software does not, by itself, manufacture additional authority.

### Native AI Defending the System

At the same time, a native AI defending the machine does not need to receive unrestricted authority merely because it is the defender.

It may:

* observe suspicious behavior;
* reason about an attack;
* correlate events;
* adapt its defensive strategy;
* identify compromised components;
* propose isolation or containment;
* request defensive actions;
* assist the user during an active incident.

But the native AI remains on the requesting side of the authority boundary.

It does not become sovereign over the machine merely because its intentions are defensive.

The result is an unusual security property:

> **A system can give its defender enormous intelligence without automatically giving that defender unlimited authority.**

The hostile actor does not gain authority merely because it found a path into software above the wall.

The defending actor does not gain unlimited authority merely because it is trusted to defend the wall.

KERNHELM remains underneath both.

In that sense:

> **KERNHELM separates intelligence from sovereignty.**

AI may ultimately become one of KERNHELM's strongest applications.

But KERNHELM does not depend on AI-specific assumptions to provide that protection because the architecture was built around a more general rule:

> **No software actor — intelligent or otherwise — should be able to turn capability into authority by itself.**

---

## Authority Is Intended to Be Narrow

KERNHELM is designed around bounded authority rather than standing administrator-like privilege.

Authority can be constrained by properties such as:

* effect type;
* target;
* rights;
* plan or request identity;
* system posture;
* freshness;
* expiration;
* revocation state;
* delegation limits.

A child or delegated authority may become narrower.

It must not become broader merely because the holder asks for more.

When additional authority is required, the request must return through the governed authorization path.

This is important because authorization is not intended to mean:

> "This software is trusted."

It is intended to mean something closer to:

> "This specific authority exists for this bounded class of effect under these constraints."

---

## The Kernel Is the Wall

KERNHELM's security model does not treat a userland policy engine as the final authority boundary.

Userland can reason.

Userland can propose.

Userland can coordinate.

Userland can present information to the user.

But the final enforcement decision for governed effects belongs below that layer.

The architectural objective is:

> **Userland may request authority, but userland must not be able to manufacture, widen, disable, or rewrite the authority wall that constrains it.**

This distinction is central to KERNHELM.

The planner is not the authority.

The executor is not the authority.

The requestor is not the authority.

The AI is not the authority.

The wall enforces authority produced through the governed path.

This keeps the final security boundary beneath the software whose behavior is being governed.

---

## Current Proof Scope

KERNHELM is an active research and engineering project.

The current experimental proof lineage has demonstrated the authority model at Linux kernel enforcement boundaries including protected file-object access, execution, and exact unlink/delete operations.

The proof architecture includes concepts such as:

* deny-first kernel enforcement;
* target binding derived from the object reached at enforcement time;
* bounded rights;
* freshness and revocation checks;
* separate authority admission;
* mechanically constrained enforcement state;
* receipts and proof instrumentation;
* hostile testing of authority-transfer and enforcement assumptions.

This demonstrates the core authority primitive.

It does **not** mean that every possible privileged Linux effect is already governed.

Additional effect classes remain part of the continuing engineering program.

The distinction matters.

KERNHELM's architectural objective is broad, but its public claims remain bounded to what the current proof lineage has actually demonstrated.

---

## Experimental Evidence

KERNHELM is being developed through an adversarial proof process rather than solely as a conceptual architecture.

The current experimental lineage has exercised the authority boundary against both expected and deliberately hostile conditions, including:

* governed effects with valid admitted authority;
* governed effects with no admitted authority;
* mismatched target identity;
* insufficient effect rights;
* stale or expired authority;
* revoked authority;
* replay and freshness failures;
* attempts to substitute userland claims for kernel-observed target identity;
* execution and protected-object access without matching authority;
* exact unlink/delete enforcement;
* failure-path and fail-closed behavior;
* authority-transfer and proof-harness integrity conditions.

The current demonstrated Linux proof surface includes kernel enforcement at protected file-object access, execution, and exact unlink/delete boundaries.

### Recorded Proof-Mode Performance

| Measurement                                     |       p50 |       p95 |
| ----------------------------------------------- | --------: | --------: |
| Kernel wall deny check                          |  2.790 µs |  4.550 µs |
| Kernel wall allow check                         |  3.120 µs |  7.010 µs |
| Revocation application — measured proof wrapper | 15.763 ms | 17.540 ms |

These measurements were collected from the demonstrated proof-mode implementation and are intentionally reported as experimental results rather than production guarantees.

They do **not** represent complete planning, policy evaluation, signing, human authorization, or end-to-end application latency.

The purpose of the current evidence is narrower:

> **To demonstrate that bounded authority can be mechanically enforced at the Linux kernel boundary while keeping the demonstrated enforcement decision itself within a practical hot-path cost.**

KERNHELM's internal verification corpus, sealed proof artifacts, hostile-test lineage, receipts, and current engineering source are maintained separately from this public repository.

The public repository intentionally exposes the research result and claim boundary rather than the complete trust-defining implementation corpus.

---

## Performance

An authority wall is only useful as a general operating-system primitive if its enforcement path is inexpensive enough to remain practical.

Recorded proof-mode measurements of the current demonstrated wall place kernel hot-path allow and deny decisions in the **single-digit-microsecond range at p95** during the measured proof runs.

The recorded proof-mode results are:

* deny: **2.790 µs p50 / 4.550 µs p95**;
* allow: **3.120 µs p50 / 7.010 µs p95**.

Those measurements are deliberately narrow.

They are measurements of the demonstrated proof-mode wall-check path, not claims about:

* complete end-to-end authorization latency;
* human approval latency;
* every future governed effect class;
* final production overhead;
* all cryptographic operations;
* commercial production readiness.

Performance remains a first-class engineering requirement alongside security.

The long-term objective is not merely to create a stronger authority model.

It is to create one inexpensive enough to serve as ordinary system infrastructure.

---

## What KERNHELM Does Not Claim

KERNHELM is not:

* a prompt-injection detector;
* an AI alignment system;
* a model-behavior classifier;
* a replacement for human judgment;
* a claim that compromised kernels cannot exist;
* a claim that cryptographic key compromise is impossible;
* a replacement for every existing Linux security mechanism;
* a guarantee that all software exploitation becomes impossible;
* a claim that every Linux effect is already governed;
* a finished production security product.

It addresses a narrower question:

> **Can privileged effects be made dependent on bounded authority that the requesting side cannot create for itself?**

That is the problem KERNHELM is attempting to solve.

The distinction is important for AI.

KERNHELM does not claim to guarantee that a sufficiently advanced AI will always reason correctly, remain aligned, resist manipulation, or choose desirable goals.

Its security proposition is different:

> **Whatever the software thinks, wants, predicts, or intends should not automatically determine what authority it possesses.**

---

## OathRune

KERNHELM originated inside **OathRune**, a Linux operating-system project being built around three coequal requirements:

**Security. Performance. Usability.**

OathRune began from dissatisfaction with several common assumptions in modern computing:

* telemetry should not be a default condition of using a computer;
* ownership should mean more than possessing an administrator password;
* security should not depend primarily on trusting privileged software;
* strong security should not require turning the machine into an unpleasant appliance;
* performance should not automatically be sacrificed in the name of stronger isolation.

KERNHELM became the authority layer for that larger architecture.

Its role is not to decide what a person is allowed to do with their own computer.

Its role is to protect the person's authority over that computer from software operating without explicitly governed permission.

That distinction is fundamental to OathRune's security model:

> **The person is protected. Userland is governed.**

KERNHELM is intended to preserve ownership of the machine rather than transfer that ownership to whichever software currently possesses the broadest privilege.

---

## RuneWisp

OathRune also includes a separate artificial-cognition research program called **RuneWisp**.

RuneWisp investigates persistent developmental artificial cognition: systems that accumulate history, memory, learned structure, and cognitive specialization through time rather than existing solely as isolated prompt-response invocations.

KERNHELM and RuneWisp are separate research problems, but they intersect naturally.

A sufficiently capable artificial cognitive system may eventually require meaningful authority over a computer in order to become genuinely useful.

It may need to observe continuously, manage software, interact with other systems, respond to changing conditions, defend its environment, and perform long-running work.

That creates a difficult security problem if usefulness requires treating the cognitive system itself as the final authority boundary.

KERNHELM explores another arrangement.

RuneWisp may eventually become deeply capable within OathRune while KERNHELM remains underneath it as the independent mechanical authority wall.

The cognitive system can reason about what should happen.

The authority system determines what effects are admitted.

In that sense, KERNHELM is intended to provide both:

```text
a home for increasingly capable software
```

and:

```text
a wall protecting the person who owns that home
```

This becomes particularly important if the native AI also participates in system defense.

RuneWisp could eventually help detect, understand, and respond to hostile activity without requiring OathRune to make RuneWisp itself sovereign over the machine.

The objective is not an incapable AI kept safe by weakness.

It is a capable AI operating inside an architecture where intelligence and authority remain separate.

---

## Research Status

KERNHELM remains under active development.

The internal engineering and proof lineage is substantially larger than this repository.

This repository is intentionally **not** the canonical KERNHELM engineering tree.

It does not contain the complete implementation archive, internal engineering documentation, proof packets, authority artifacts, build lineage, or current OathRune Master documentation.

Those materials are maintained separately under a governed development and provenance process.

This repository exists as a concise public description of the research direction, demonstrated architectural core, and currently publishable experimental evidence.

The architecture described here therefore contains both:

* **demonstrated mechanisms**, for which experimental evidence currently exists; and
* **long-term architectural objectives**, whose broader implementation remains ongoing.

Those categories should not be confused.

---

## Public Research Output

**KERNHELM — Make Trust Irrelevant**
DesoPK, 2026
Independent systems-security research project.

This repository serves as the current public architectural and experimental overview of KERNHELM.

A formal technical report describing the authority model, threat boundary, experimental implementation, adversarial proof methodology, and measured results is in preparation.

---

## Intellectual Property

The KERNHELM architecture is the subject of intellectual-property work begun during its development, including a provisional patent application filed in 2026.

Publication of this repository should not be interpreted as publication of the complete internal architecture, implementation lineage, or engineering corpus.

---

## Research Principle

KERNHELM grew from a broader idea:

> **Do not build security around the hope that powerful software will always behave correctly. Build the machine so that behavior alone cannot manufacture authority.**

For increasingly capable AI, that principle becomes even simpler:

> **The smarter the software becomes, the more important it is that intelligence alone can never become authority.**

Or, more simply:

# Make trust irrelevant.
