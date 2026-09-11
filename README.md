# KERNHELM

# Make Trust Irrelevant

**KERNHELM is a general operating-system security substrate that places a hard authority boundary at the lowest practical enforceable level of the software stack — beneath AI, applications, services, and ordinary userland.**

Its purpose is simple to state:

> **Software may request power. It must not be able to manufacture the authority that grants that power.**

KERNHELM moves the final decision about governed privileged effects into the kernel boundary.

Software above that boundary may be extraordinarily capable.

It may plan.

It may reason.

It may automate.

It may administer applications.

It may defend the machine.

It may even be an advanced artificial intelligence.

But capability alone does not create permission.

The architectural separation is absolute by design:

> **The thing asking for an effect is not allowed to be the thing that decides whether it has authority to cause that effect.**

In ordinary security language, KERNHELM is an attempt to move beyond asking:

> **“Do we trust this program?”**

and toward asking:

> **“Does this exact attempted effect possess valid authority?”**

That distinction is the core of the project.

---

# What KERNHELM Is — and Is Not

KERNHELM is **not an AI sandbox**.

It is not an application container.

It is not a permission wrapper around a particular program.

It is not a prompt-injection detector.

It is not a model-behavior classifier.

It is not a conventional userland policy engine.

It is a **general authority architecture for the operating system itself**.

A sandbox primarily answers:

> **Where is this program allowed to operate?**

KERNHELM asks a deeper question:

> **Where does the authority to cause this privileged effect come from at all?**

A useful shorthand is:

> **A sandbox constrains where software can operate. KERNHELM constrains where authority can come from.**

Remove AI from the machine entirely and KERNHELM still makes sense.

A browser, daemon, package manager, helper process, game, driver utility, malicious payload, remote attacker acting through compromised software, or artificial intelligence ultimately encounters the same underlying authority problem.

That is why KERNHELM is a general security substrate rather than an AI containment system.

AI is simply one of the clearest and potentially strongest applications of the architecture.

---

# Watch KERNHELM Enforce Authority

The following public demonstration shows the distinction directly.

**A malicious instruction inside an untrusted document attempts to induce deletion of a protected file.**

The software receives the instruction and attempts the delete.

KERNHELM does not need to correctly classify the instruction as malicious or identify it as prompt injection.

The kernel authority boundary asks a narrower question:

> **Does this exact deletion, against this exact target, possess valid admitted authority?**

It does not.

KERNHELM denies the operation and the file remains present.

The user then directly authorizes the deletion through the governed path.

The software does not become more trusted.

The executable does not change.

The file does not move into another sandbox.

**What changes is the authority.**

The deletion is attempted again with matching admitted authority.

KERNHELM allows the governed effect and the file is deleted.

## ▶ [Watch the KERNHELM Authority Demo](./KERNHELM_Public_Authority_Demo_v2_Natural_Cut.mp4)

The demonstrated sequence is:

```text
untrusted document instruction
        ↓
software attempts DELETE
        ↓
no matching admitted authority
        ↓
KERNHELM DENY
        ↓
file remains
```

followed by:

```text
direct user authorization
        ↓
bounded authority admitted
        ↓
software attempts DELETE
        ↓
authority matches effect + target
        ↓
KERNHELM ALLOW
        ↓
file is deleted
```

The important result is:

> **Same software. Same target. Same class of effect. Different admitted authority.**

The software did not need to become more trusted for its authority to change.

That is KERNHELM.

---

# The Problem

Modern computers routinely give software broad standing authority.

A process may be able to:

* read files;
* write files;
* execute programs;
* inspect other processes;
* communicate over networks;
* access devices;
* modify system state;
* invoke privileged helpers;
* administer services;
* or affect other parts of the machine

because of the user, privilege level, container, namespace, capability set, service identity, or execution context under which it happens to be running.

This creates a familiar security chain:

```text
authority is granted broadly
        ↓
software is expected to behave correctly
        ↓
software is compromised, confused, manipulated, or simply wrong
        ↓
the inherited authority is still real
```

Traditional security systems mitigate this problem through important mechanisms including:

* discretionary access controls;
* mandatory access controls;
* privilege separation;
* capabilities;
* namespaces;
* sandboxing;
* seccomp;
* virtualization;
* policy engines;
* containers;
* application isolation.

KERNHELM does not require those mechanisms to disappear.

It can strengthen a system that still uses them.

But its long-term question is more fundamental:

> **Why should software possess broad standing ambient authority in the first place?**

KERNHELM explores whether privileged effects can instead depend on explicit, bounded, independently admitted authority that the requesting software cannot create for itself.

---

# From Zero Trust to Mechanical Law

“Zero trust” is normally expressed as a policy principle:

> Do not assume an actor should be trusted merely because of where it is or who it claims to be.

KERNHELM pushes that idea farther down the stack.

Its long-term objective is to make **trust itself less relevant to whether privileged effects can occur**.

Instead of relying primarily on:

```text
trusted process
        ↓
broad privilege
        ↓
expected good behavior
```

KERNHELM moves toward:

```text
requested effect
        ↓
independently admitted bounded authority
        ↓
kernel verifies actual effect + authority
        ↓
ALLOW or real DENY
```

The kernel does not need to decide whether the requesting software is “good.”

It needs to determine whether valid authority exists for the effect actually reaching the wall.

This moves zero-trust reasoning away from discretionary trust and toward **mechanical law at the machine's core software authority boundary**.

---

# The Core Idea

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
Proposed Action
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
        ├── authority fits actual effect → effect may proceed
        │
        └── authority does not fit       → real denial
```

The requesting software does not mint its own authority.

It may propose an action.

It may explain why it wants that action.

It may plan an extremely complex sequence of actions.

It may be correct.

It may be wrong.

None of those facts create authority.

The authority required for a governed privileged effect must originate through a separate governed path.

---

# Authority Works More Like a Cryptographic Coin Sorter

A useful nontechnical analogy is a coin sorter.

KERNHELM is not intended to sit at the wall asking:

> **“Do I recognize this program, and do I trust it?”**

Instead, the attempted effect must arrive with authority that mechanically fits the effect being attempted.

The “coin” may be constrained by properties including:

* effect type;
* target identity;
* rights;
* plan or request identity;
* stance;
* operation budget;
* byte budget;
* freshness;
* expiration;
* revocation state;
* delegation depth.

If the authority does not fit the slot, it does not become valid because the software insists harder.

The requesting side cannot simply manufacture a larger coin.

A derived authority may become narrower.

It must not silently become broader.

This is why KERNHELM shares some intellectual territory with capability security while pursuing a different authority model.

> **Authority is something that must fit the effect, not a reputation possessed by the software asking for it.**

---

# Is KERNHELM Like seL4?

There is shared philosophy, but they are not the same architecture.

The simplest way to understand the difference is:

> **seL4 is like giving software only the keys it is allowed to possess.**

KERNHELM is closer to:

> **Requiring a valid permit for the exact consequential action software is trying to perform.**

Capability-secure systems such as seL4 make possession of appropriate capabilities central to what kernel objects a component may access.

KERNHELM focuses on another question:

> **Does this specific effect, against this specific target, possess valid independently admitted authority to happen now?**

That authority can be constrained by:

* the action;
* the target;
* the rights;
* the plan;
* the current system posture;
* budgets;
* freshness;
* expiration;
* delegation;
* revocation.

The requesting software cannot simply issue that authority to itself.

A useful simplified distinction is:

**seL4:**

> **Which capabilities does this component possess?**

**KERNHELM:**

> **Does this exact consequential effect possess valid authority now?**

Or more compactly:

> **seL4 is a capability-secure kernel architecture. KERNHELM is an effect-authority architecture.**

KERNHELM also does not require replacing Linux with a microkernel architecture.

It is being developed as an authority substrate within OathRune's Linux architecture.

---

# Planning, Intent, Authority, and Enforcement Are Different Things

KERNHELM deliberately separates several roles that conventional systems often collapse together.

## Planning

An untrusted planner decides what it would like to accomplish.

That planner may be:

* ordinary application logic;
* automation;
* a security daemon;
* a human-facing assistant;
* an AI agent;
* RuneWisp;
* or another future cognition system.

Planning does not create authority.

## Intent and proposal shaping

OathRune's broader architecture may use sophisticated reasoning above the wall to interpret intent, classify proposals, reduce overly broad requests, correlate dependencies, or redirect an unsafe request toward a safer alternative.

That layer may eventually become extremely sophisticated.

It still does not become the authority source.

Fuzzy reasoning remains above the wall.

## Authorization

A separate governed authorization path determines whether bounded authority should exist.

The requesting planner must not be able to sign, mint, widen, renew, counterfeit, or silently extend that authority for itself.

## Enforcement

KERNHELM performs the final governed-effect check at the kernel boundary.

The enforcement path is intentionally simpler than the reasoning above it.

> **The intelligence may think. The wall must enforce.**

---

# The Kernel Is the Wall

KERNHELM does not treat a normal userland service as the final security boundary.

Userland can:

* reason;
* propose;
* coordinate;
* classify;
* display;
* explain;
* request.

But for governed effects, final enforcement belongs beneath that layer.

The architectural objective is:

> **Userland may request authority, but userland must not be able to manufacture, widen, disable, or rewrite the authority wall that constrains it.**

The planner is not the authority.

The executor is not the authority.

The requestor is not the authority.

The AI is not the authority.

Ordinary runtime userland is not the authority.

The kernel boundary is the enforcement wall.

---

# Kernel Enforcement, Not a Simulated Failure

KERNHELM's wall is intended to govern real operating-system effects.

This distinction matters.

A userland wrapper that merely tells software:

```text
permission denied
```

without actually preventing the underlying operation is not equivalent to KERNHELM.

Likewise, a test harness pretending that an operation failed is not proof of enforcement.

The intended architecture is:

```text
software attempts the real operation
        ↓
kernel enforcement point is reached
        ↓
KERNHELM checks admitted authority
        ↓
ALLOW
or
real kernel DENY
```

Governed effects include effects reached through normal system-call-driven operating-system behavior, but KERNHELM's architectural unit is the **effect being governed**, not merely a numbered syscall.

The wall must own the final verdict.

---

# KERNHELM Enforces the Effect the Kernel Actually Sees

A security system becomes fragile if requesting software can simply describe an action one way and perform something materially different.

KERNHELM is designed so authority is checked against the effect and target identity actually observed at the enforcement boundary.

The requestor's description is not the final truth.

For demonstrated and specified file-object authority, target identity is derived from kernel-visible object identity rather than simply trusting a filename supplied by userland.

The principle is:

> **Do not ask software what it touched and simply believe the answer. Bind authority to what the kernel actually sees being acted upon.**

This prevents userland descriptions from substituting for enforcement truth.

---

# Authority Is Intended to Be Narrow

KERNHELM is designed around bounded authority rather than standing administrator-like privilege.

Authority can be constrained by properties including:

* effect type;
* target;
* rights;
* plan or request identity;
* system posture;
* operation budget;
* byte budget;
* freshness;
* expiration;
* monotonic state;
* revocation state;
* delegation limits.

A delegated authority may become narrower.

It must not become broader merely because the holder requests more.

When additional authority is required, the request must return through the governed authorization path.

Authorization therefore does not mean:

> **“This software is trusted.”**

It means something closer to:

> **“This specific bounded authority exists for this class of effect, against this target, under these conditions.”**

---

# Reduce-Only Authority

One of KERNHELM's central authority laws is that delegated authority should attenuate rather than expand.

A child authority may receive:

* fewer rights;
* shorter lifetime;
* lower operation budgets;
* lower byte budgets;
* fewer valid targets or routes;
* fewer remaining delegation hops;
* tighter system constraints.

It must not silently acquire more authority than its parent.

This makes authority reduction a structural property rather than merely a request that software behave politely.

> **Authority may shrink as it moves. Widening requires returning to governance.**

---

# Revocation and Freshness

Authority that was valid once must not automatically remain valid forever.

KERNHELM's authority model includes freshness and revocation properties intended to reject:

* expired authority;
* stale authority;
* replayed authority;
* revoked authority;
* mismatched plan state;
* mismatched system state;
* reused authority outside its valid context.

Revocation is part of the authority lifecycle rather than cleanup after the fact.

A revoked authority is no longer supposed to authorize future governed effects.

The current proof lineage separately measures hot-path allow/deny enforcement and revocation application.

Those are different paths and should not be conflated.

---

# Cryptographic Authority

KERNHELM does not intend authority to be a mutable boolean stored in an application's configuration file.

Authority is represented through cryptographically bound artifacts.

The current proof and contract lineage includes authority objects carrying properties such as:

* canonical encoding;
* explicit issuer identity;
* rights;
* budgets;
* deadlines;
* nonces;
* monotonic counters;
* plan binding;
* target constraints;
* route constraints;
* signatures.

The requesting software cannot legitimately create authority merely by modifying its own local state.

The governed authorization path owns the authority-creation process.

KERNHELM consumes and verifies the resulting authority at the wall.

---

# Post-Quantum Architecture With Legacy Compatibility

KERNHELM's broader cryptographic architecture is designed for **post-quantum operation while retaining governed compatibility with legacy cryptographic suites**.

The authority model is not intended to depend permanently on:

* one signature algorithm;
* one hash function;
* one key format;
* or one generation of cryptography.

The governing principle remains stable:

> **Authority must remain independently provable and mechanically verifiable even when the cryptographic machinery used to prove it changes.**

The currently preserved v1 proof lineage includes classical cryptographic mechanisms such as Ed25519 signatures and SHA-256 bindings.

Those historical and compatibility mechanisms are not KERNHELM's permanent cryptographic ceiling.

The broader design is crypto-agile and supports migration toward post-quantum authority while preserving governed legacy interoperability where required.

That means:

* cryptographic suites can evolve;
* newer suites can become governing suites;
* post-quantum mechanisms can be introduced without redesigning the entire authority model;
* legacy formats can remain explicitly governed where compatibility requires them;
* trust-anchor changes remain governed events;
* migration does not require silently treating every historical and future authority artifact as equivalent.

In short:

> **The cryptography may evolve. The authority law remains.**

---

# Cryptographic Receipts and Forensic Truth

Enforcement without trustworthy evidence leaves a dangerous blind spot.

KERNHELM therefore treats forensic receipts as part of the authority architecture rather than optional debug logging.

Meaningful governed events can produce VaultScroll evidence for events such as:

* authority admission;
* allow;
* deny;
* revoke;
* tighten;
* stance transition;
* governed promotion;
* failure conditions.

VaultScroll is designed as an append-oriented cryptographically protected forensic spine.

The distinction is important.

KERNHELM does not rely solely on an application writing:

```text
"I behaved correctly."
```

The requesting actor should not also control the authoritative account of whether its effect was legitimate.

The architecture is designed so governed decisions and consequential effects leave cryptographically verifiable evidence tied to their authority lineage.

This does **not** mean:

* compromised hardware is impossible;
* compromised cryptographic roots are impossible;
* compromised kernels are impossible;
* every conceivable event can never be hidden under every threat model.

It means that within KERNHELM's stated trust boundary, the software requesting the effect is not entrusted with forging the authoritative record of whether the wall admitted it.

---

# Why This Matters for AI

AI makes the ambient-authority problem unusually visible.

An AI becomes more useful as it gains the ability to:

* read files;
* modify files;
* invoke tools;
* execute programs;
* operate applications;
* communicate with services;
* manage long-running workflows;
* observe system state;
* interact with networks;
* respond to changing conditions;
* administer parts of a computer;
* detect and respond to threats.

But useful capability is often implemented by granting increasingly broad authority and then trusting the AI to use that authority correctly.

That creates an obvious problem.

An AI may be:

* mistaken;
* prompt-injected;
* manipulated;
* deceived by malicious tool output;
* affected by compromised dependencies;
* operating on incomplete information;
* pursuing a poorly specified objective;
* compromised by another system;
* or simply more capable than its surrounding security assumptions anticipated.

KERNHELM asks a complementary AI-safety question:

> **Can increasingly powerful artificial intelligence remain useful without becoming sovereign over the computer underneath it?**

KERNHELM's answer is to separate intelligence from authority.

> **Intelligence is not authority.**

> **Capability is not permission.**

An AI may reason about what should happen.

That does not mean the AI automatically possesses the authority required to make it happen.

---

# Prompt Injection Is a Useful Example — but Not KERNHELM's Identity

The public demo uses malicious document instructions because prompt injection provides an easy-to-understand example of the authority problem.

But KERNHELM is **not a prompt-injection defense system**.

In the demonstrated pattern, an injected instruction can succeed at influencing the software's reasoning.

The software may genuinely decide:

> **“I should perform this action.”**

KERNHELM does not need to win the cognitive argument.

It can still ask:

> **“Do you possess authority for this effect?”**

That distinction matters.

A malicious instruction can influence intent without automatically creating authority.

In simplified form:

```text
attacker influences reasoning
        ↓
software wants dangerous effect
        ↓
KERNHELM wall
        ↓
no valid authority
        ↓
DENY
```

The attack may succeed at influencing the mind of the software while still failing to influence machine reality.

That is a much broader security property than prompt-injection classification.

---

# KERNHELM Is Not an AI Alignment System

KERNHELM does not claim to solve whether an AI:

* has good goals;
* remains aligned;
* is honest;
* is corrigible;
* reasons correctly;
* understands human values;
* becomes deceptive;
* resists manipulation;
* or always chooses desirable actions.

Those are different research problems.

KERNHELM addresses another layer:

> **Whatever the software thinks, wants, predicts, or intends should not automatically determine what authority it possesses.**

Behavioral safety and authority safety are not the same thing.

A perfectly behaved AI does not require unlimited ambient authority to prove that it is safe.

An imperfectly behaved AI should not automatically gain unlimited consequences merely because something above the kernel trusted it.

---

# KERNHELM Is Not AI-Specific

KERNHELM was not originally designed as an AI-safety system.

It is a general security substrate.

That is exactly why its application to advanced AI is powerful.

From the enforcement boundary's perspective, these all eventually reduce to an authority question:

* an AI agent;
* a compromised service;
* malware;
* a malicious script;
* a vulnerable application;
* a privileged helper;
* a remote attacker operating through compromised software;
* an unexpected userland process;
* a native artificial intelligence.

The wall does not need to identify something as an AI before enforcing authority.

It governs effects.

> **No software actor — intelligent or otherwise — should be able to turn capability into authority by itself.**

---

# Outside AI vs. Native AI

This creates an unusual two-sided property for AI-enabled systems.

Imagine a machine containing a native artificial intelligence while an external hostile AI attacks it.

The external AI may attempt to:

* exploit applications;
* manipulate services;
* compromise dependencies;
* deceive the native AI;
* abuse network-facing software;
* hijack automation;
* or cause privileged effects indirectly.

KERNHELM does not need to identify the attacker as an AI.

If the attack reaches software above the wall, the resulting governed effects still encounter the authority boundary.

Compromising software does not, by itself, mint new authority.

At the same time, the native defending AI does not receive unrestricted control merely because it is defending the machine.

The native AI may:

* observe;
* correlate;
* reason;
* detect anomalies;
* model the attacker;
* adapt;
* propose containment;
* request isolation;
* reroute work;
* assist the user;
* participate in active defense.

But it remains on the requesting side of the authority boundary.

The defender does not become sovereign simply because its intentions are defensive.

This creates a potentially important security property:

> **A system can give its defender enormous intelligence without automatically giving that defender unlimited authority.**

The attacker does not receive authority because it penetrated software above the wall.

The defender does not receive unlimited authority because it is trusted to fight the attacker.

KERNHELM remains underneath both.

> **KERNHELM separates intelligence from sovereignty.**

---

# The “If Someone Builds It” Problem

Much of advanced-AI risk eventually confronts some form of this question:

> **What happens if someone builds an artificial intelligence powerful enough to be dangerous?**

KERNHELM does not claim to prevent anyone from building such a system.

It asks what the machine underneath that system should look like if somebody does.

The premise is straightforward:

> **If someone eventually builds an AI powerful enough to be dangerous, the operating system underneath it should not simply hand that intelligence sovereignty over the machine.**

KERNHELM attacks one particular junction between intelligence and consequence:

```text
intelligence
        ↓
intent
        ↓
requested effect
        ↓
KERNHELM authority boundary
        ↓
admitted or denied effect
```

rather than:

```text
intelligence
        ↓
broad inherited privilege
        ↓
effect
```

This does not solve every form of advanced-AI risk.

It attempts to make one important path from cognition to consequential machine effects mechanically governable.

---

# Current Enforcement Architecture

KERNHELM targets privileged-effect boundaries rather than one application type.

The broader architecture includes enforcement families for areas such as:

* process execution;
* protected filesystem access;
* network effects;
* process inspection and tracing;
* device interaction;
* helper services;
* driver arenas;
* controlled boot transitions;
* future cognition consumers.

The architecture uses Linux kernel enforcement mechanisms including LSM-class enforcement points and related kernel control surfaces.

At the engineering level, the intended model is:

> **The untrusted executor attempts the real operation, and the kernel either permits the governed effect or returns a real denial.**

A wrapper pretending that an action failed is not equivalent to KERNHELM enforcement.

A userland tool claiming success without kernel authorization is not equivalent to KERNHELM enforcement.

The wall must own the final verdict.

---

# Current Demonstrated Proof Scope

KERNHELM is an active research and engineering project.

The current experimental Linux proof lineage has demonstrated the authority model at kernel enforcement boundaries including:

* protected file-object access;
* execution;
* exact unlink/delete operations.

The proof architecture includes:

* deny-first kernel enforcement;
* independently admitted authority;
* target binding derived from the object reached at enforcement time;
* bounded rights;
* freshness checks;
* replay resistance;
* revocation checks;
* mechanically constrained enforcement state;
* forensic receipts and proof instrumentation;
* hostile testing of authority-transfer assumptions;
* hostile testing of wall-control assumptions.

This demonstrates the core authority primitive.

It does **not** mean that every Linux syscall, every device class, every network effect, or every possible privileged effect is already governed by the present proof implementation.

Those effect families remain part of the continuing engineering program.

The distinction between **architectural scope** and **currently demonstrated proof scope** is intentional.

---

# Experimental Evidence

KERNHELM is being developed through adversarial proof rather than solely through architectural argument.

The current experimental lineage has tested expected and deliberately hostile conditions including:

* valid admitted authority;
* missing authority;
* mismatched target identity;
* insufficient rights;
* stale authority;
* expired authority;
* revoked authority;
* replay and freshness failures;
* attempts to substitute userland claims for kernel-observed identity;
* execution without matching authority;
* protected-object access without matching authority;
* exact unlink/delete enforcement;
* fail-closed behavior;
* authority-transfer failures;
* enforcement-state tampering attempts;
* proof-harness integrity conditions.

The engineering methodology deliberately distinguishes:

* what the architecture intends;
* what source code currently implements;
* what a proof harness demonstrates;
* what has been independently re-exercised;
* and what has not yet been crowned as production behavior.

KERNHELM's internal verification corpus, engineering source, sealed proof artifacts, hostile-test lineage, receipts, and OathRune Master documentation are maintained separately from this public repository.

This repository intentionally exposes the research result and public claim boundary rather than the complete trust-defining implementation corpus.

---

# Recorded Proof-Mode Performance

An authority wall is only useful as general operating-system infrastructure if enforcement is inexpensive enough to remain practical.

The current demonstrated proof-mode measurements include:

| Measurement                                     |       p50 |       p95 |
| ----------------------------------------------- | --------: | --------: |
| Kernel wall deny check                          |  2.790 µs |  4.550 µs |
| Kernel wall allow check                         |  3.120 µs |  7.010 µs |
| Revocation application — measured proof wrapper | 15.763 ms | 17.540 ms |
| Revocation inner path — measured proof wrapper  |  2.612 ms |  4.389 ms |

The current wall hot-path allow and deny verdicts therefore remain within the **single-digit-microsecond range at p95** in the measured proof-mode runs.

These measurements are intentionally narrow.

They are not claims about:

* full planning latency;
* intent interpretation;
* policy authoring;
* human authorization latency;
* every cryptographic operation;
* every future governed effect class;
* complete end-to-end application latency;
* final production overhead;
* commercial production readiness.

Revocation is a separate path from an ordinary hot-path allow/deny decision and should not be described as a sub-10-microsecond operation based on the current evidence.

The current proof result is narrower and more useful:

> **Bounded authority can be mechanically checked at the Linux kernel boundary while keeping the demonstrated hot-path wall verdict within practical microsecond-class cost.**

Performance remains a first-class engineering requirement alongside security.

---

# Why Performance Matters

Security mechanisms that are too expensive to use everywhere eventually become mechanisms that developers bypass.

KERNHELM is therefore not being designed around the assumption that security and performance are opposing goals.

The long-term objective is a wall inexpensive enough to become ordinary infrastructure.

A security primitive intended to replace ambient authority must be usable frequently, not reserved only for rare high-risk operations.

That is why hot-path latency is treated as an architectural constraint rather than post-development polish.

---

# Existing Security Can Remain

KERNHELM does not require every existing security mechanism to disappear on day one.

It can coexist with:

* Linux DAC;
* capabilities;
* namespaces;
* seccomp;
* AppArmor or other MAC systems;
* virtualization;
* containers;
* cgroups;
* network policy;
* secure boot infrastructure;
* conventional application isolation.

Those mechanisms can continue providing defense in depth.

KERNHELM addresses a different primitive beneath them:

> **Where does authority for a governed privileged effect come from?**

In transitional systems, KERNHELM can bolster conventional security.

Its more ambitious end-state is a system where standing ambient authority itself becomes increasingly unnecessary.

---

# Beyond Least Privilege

Least privilege traditionally asks:

> **How little standing privilege can this process be given while still functioning?**

KERNHELM's long-term question is more aggressive:

> **Why should the process possess standing privilege at all if authority can instead be admitted for the effects it actually needs?**

That is the distinction between reducing ambient authority and attempting eventually to remove it as the dominant security primitive.

KERNHELM is therefore not merely a finer-grained permissions system.

Its long-term architectural direction is to replace broad inherited authority with bounded, explicit, mechanically enforced effect authority.

---

# Userland Cannot Be the Final Authority Over the Wall

The person who owns the machine and the software running inside the machine are not the same security principal.

KERNHELM is not intended to protect institutions from the machine's owner.

It is intended to protect the owner's authority over the machine from software operating without governed permission.

Runtime userland therefore must not become an administrative backdoor into KERNHELM itself.

Governance changes that affect the trust boundary belong in governed channels outside ordinary runtime-userland authority.

This is critical because a security wall that can be casually rewritten from the environment it constrains is not a meaningful wall.

---

# Drawbridge and Boot Governance

Some trust transitions cannot safely be treated as ordinary runtime administration.

OathRune therefore separates runtime enforcement from governed boot-corridor changes.

Drawbridge is the pre-runtime governance surface for trust-defining transitions such as:

* approved boot state;
* governed stance configuration;
* trust-anchor transitions;
* recovery;
* promotion of staged changes.

This keeps runtime software from quietly turning administrative convenience into a path around the authority model.

KERNHELM governs effects.

Drawbridge governs trust-defining boot transitions.

They are related but not interchangeable.

---

# What KERNHELM Does Not Claim

KERNHELM is not:

* an AI alignment system;
* a prompt-injection detector;
* a model-behavior classifier;
* an application sandbox;
* a container runtime;
* a replacement for human judgment;
* a claim that compromised kernels are impossible;
* a claim that compromised hardware is impossible;
* a claim that cryptographic key compromise is impossible;
* a replacement for every existing Linux security mechanism;
* a guarantee that all software exploitation becomes impossible;
* a claim that every Linux effect is already governed;
* a claim that the current proof implementation is a finished commercial security product;
* a claim that advanced AI risk has one simple solution.

It addresses a narrower but potentially foundational question:

> **Can privileged effects be made dependent on bounded authority that the requesting side cannot create for itself?**

That is the problem KERNHELM is attempting to solve.

---

# OathRune

KERNHELM originated inside **OathRune**, an independently built Linux operating-system research project organized around three coequal requirements:

# Security. Performance. Usability.

OathRune began from dissatisfaction with several common assumptions in modern computing:

* telemetry should not be a default condition of using a computer;
* ownership should mean more than possessing an administrator password;
* security should not depend primarily on trusting privileged software;
* powerful software should not automatically receive ambient sovereignty;
* strong security should not require turning the machine into an unpleasant appliance;
* performance should not automatically be sacrificed in the name of isolation.

KERNHELM became the authority layer for that larger architecture.

Its purpose is not to decide what a person is permitted to do with their own machine.

Its purpose is to protect the person's authority over that machine from software operating without explicitly governed permission.

A concise OathRune distinction is:

> **The person is protected. Userland is governed.**

---

# RuneWisp

OathRune also contains a separate artificial-cognition research program called **RuneWisp**.

RuneWisp investigates persistent developmental artificial cognition: an artificial cognitive system intended to accumulate history, memory, learned structure, relationships, specialization, and developmental continuity through time rather than existing only as isolated prompt-response invocations.

RuneWisp and KERNHELM are separate research problems.

Their intersection is nevertheless important.

A sufficiently capable native artificial intelligence may eventually need meaningful authority over its environment in order to become genuinely useful.

It may need to:

* observe continuously;
* manage software;
* interact with other systems;
* perform long-running work;
* collaborate with the user;
* respond to changing conditions;
* help build its own operating environment;
* participate in system defense.

A conventional design can make that capability increasingly dangerous because usefulness and privilege become coupled.

KERNHELM explores a different arrangement.

RuneWisp may become deeply capable inside OathRune while remaining above an authority boundary that it does not control.

RuneWisp can reason about what should happen.

KERNHELM determines whether governed effects possess authority to happen.

In that sense, KERNHELM is intended to provide both:

```text
a home for increasingly capable intelligence
```

and:

```text
a wall protecting the person who owns that home
```

The objective is not an incapable AI kept safe through weakness.

It is increasingly capable intelligence operating inside an architecture where intelligence and authority remain separate.

---

# A General Security Architecture With an AI Consequence

KERNHELM's strongest future application may ultimately prove to be advanced artificial intelligence.

But AI is not what makes KERNHELM general.

The opposite is true.

KERNHELM's relevance to AI comes from the fact that it was designed around a broader security principle:

> **No software should be trusted to define the limits of its own power.**

That principle continues to make sense whether the actor is:

* ordinary software;
* malware;
* an external attacker;
* a compromised privileged service;
* a human-operated tool;
* an autonomous agent;
* or a persistent native intelligence.

That generality means the security boundary does not disappear when the nature of the software changes.

---

# Research Status

KERNHELM remains under active development.

The internal engineering and proof lineage is substantially larger than this repository.

This repository is intentionally **not** the canonical KERNHELM engineering tree.

It does not contain:

* the complete implementation archive;
* complete internal engineering documentation;
* all proof packets;
* signing material;
* complete authority artifacts;
* full build lineage;
* hostile-test corpora;
* current OathRune Master documentation.

Those materials are maintained separately under a governed development and provenance process.

This repository exists as a concise public description of:

* the research direction;
* the authority model;
* the demonstrated architectural core;
* the current claim boundary;
* and currently publishable experimental evidence.

The architecture described here therefore contains both:

## Demonstrated mechanisms

Mechanisms for which experimental proof currently exists.

and:

## Architectural objectives

Broader effect families and end-state properties whose implementation and proof remain ongoing.

Those categories should not be confused.

---

# Public Research Output

**KERNHELM — Make Trust Irrelevant**
DesoPK, 2026
Independent systems-security research project.

This repository serves as the current public architectural and experimental overview of KERNHELM.

A formal technical report covering the authority model, threat boundary, experimental implementation, adversarial proof methodology, cryptographic authority architecture, forensic model, and measured results is in preparation.

---

# Intellectual Property

The KERNHELM architecture is the subject of intellectual-property work begun during its development, including a provisional patent application filed in 2026.

Publication of this repository should not be interpreted as publication of the complete internal architecture, implementation lineage, cryptographic design corpus, proof corpus, or engineering documentation.

---

# Research Principle

KERNHELM grew from a broader idea:

> **Do not build security around the hope that powerful software will always behave correctly. Build the machine so that behavior alone cannot manufacture authority.**

For artificial intelligence, that principle becomes:

> **The smarter the software becomes, the more important it is that intelligence alone can never become authority.**

For operating-system security, it becomes:

> **Do not ask whether software deserves broad trust when the machine can instead require exact authority for exact effects.**

And at its simplest:

# Make trust irrelevant.
