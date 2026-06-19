# KERNHELM Threat Model

This file defines the public threat model for the current KERNHELM proof lane.

The short version:

> The actor may want the wrong thing. The wall cares whether the privileged effect has admitted authority.

KERNHELM is not trying to make untrusted code morally correct, impossible to confuse, or impossible to compromise. It is trying to stop untrusted userland requestors from turning desire, confusion, compromise, or inherited ambient authority into privileged effects unless those effects were admitted through the trusted path.

## Governed actors

The governed actor may be:

- an AI agent
- an autonomous script
- a tool-using automation layer
- a provider-supplied agent installed on the user's machine
- a compromised dependency
- a confused deputy
- a normal userland process whose behavior is not fully predictable

The governed actor is not trusted to describe its own authority honestly.

It may:

- lie about what it is doing
- be prompt-injected
- be socially engineered by input data
- be compromised after launch
- attempt an effect outside the approved plan
- attempt to reuse stale authority
- attempt to widen delegated authority
- attempt to reach a different target than the one admitted

## Trusted side

The trusted side includes:

- the trusted authorizer/policy path
- the signing/minting path for permits
- the trusted bridge that injects bounded kernel-side allow-state
- the kernel wall and its enforcement hooks
- the stance/revocation state consumed by the wall
- the boot or pre-runtime corridor that establishes the wall and its trusted state

The current model assumes those pieces are not controlled by the untrusted requestor.

If the trusted authorizer, signing key, trusted bridge, kernel, or boot chain is compromised, that is outside the current wall claim. Other defenses may still matter, but this proof lane does not claim to solve that fight by itself.

## In-scope attacks

The current model is meant to address attacks where the requestor is untrusted but does not control the kernel or trusted admission path.

In scope:

- prompt injection that makes an agent want an unauthorized action
- a compromised dependency trying to read, execute, or delete governed file objects
- a process trying to inherit ambient authority it was never explicitly granted
- a requestor trying to act outside the admitted plan
- a requestor trying to use stale or expired authority
- a requestor trying to continue after revocation
- a requestor trying to widen a delegated permit
- target substitution between approval and use
- confused-deputy behavior where a tool tries to perform a privileged effect beyond the user's approved intent

The wall does not need to understand why the actor attempted the action. It only needs to determine whether the effect/target fits admitted authority at enforcement time.

## Current proof boundary

The current proof lane is focused on file-object and execution boundaries.

Current proven wall surfaces:

- `file_open`
- `bprm_check_security`
- `inode_unlink`

Current demonstrated effect families:

- protected file-object access
- execution control
- exact unlink/delete enforcement

Network egress, ptrace-like process inspection/control, device access, desktop capture, package mutation, and other privileged surfaces are valid architectural targets, but they should not be described as currently proven unless the corresponding public hook, test, and receipt evidence exist.

## Target identity and drift

For file-object effects, the wall does not rely on a human-readable path string at enforcement time.

The wall derives target identity from the kernel-visible object identity, such as device and inode identity. That means approval for one object does not automatically apply to another object reached later by a different path or after a substitution.

This closes target/effect drift for the current file-object wall.

It does not claim that file contents are frozen if the same inode's contents change after approval. That is a different property and should not be implied.

## Out-of-scope attacks

Out of scope for the current claim:

- kernel code execution by an attacker
- malicious kernel modules loaded outside the wall's control
- hardware compromise
- physical attacks against the machine
- signing-key compromise
- compromise of the trusted authorizer
- compromise of the trusted bridge that writes wall state
- a human approving a malicious plan on purpose or by mistake
- off-machine effects after an artifact leaves the governed system
- proving every future privileged surface before its hook exists
- production-hardening completeness

KERNHELM can still be one layer in a larger defense stack, but these are not problems the current proof lane should claim to solve alone.

## Human decision boundary

KERNHELM does not decide what the user should want.

The human or trusted policy path still decides whether a plan should be approved. KERNHELM addresses the separate problem that comes after approval: whether the untrusted requestor actually stays inside the authority that was admitted.

Bad human approval can still authorize bad behavior. The wall is not magic judgment. It is mechanical enforcement of the admitted boundary.

## Fail-closed law

For governed targets, missing truth does not become permission.

The wall should deny when required enforcement state is missing, stale, insufficient, expired, revoked, or mismatched to the effect being attempted.

That is the heart of the model:

```text
No admitted authority -> no privileged effect.
```

## Relationship to existing Linux defenses

KERNHELM is not a claim that existing Linux defenses are worthless.

SELinux, AppArmor, seccomp, namespaces, capabilities, cgroups, hardened allocators, exploit mitigations, and sandboxing can all still matter.

KERNHELM's claimed distinction is the authority shape: a trusted path admits scoped, plan-bound authority, and the kernel wall checks the actual effect/target against that live authority instead of trusting the requestor's identity, role, label, or explanation by itself.
