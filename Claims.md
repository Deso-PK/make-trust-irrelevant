# KERNHELM Claim Boundaries

This file is the public claim wall for the `make-trust-irrelevant` repo.

It is not the full paper. It is not a patent filing. It is the short map of what is being claimed, what is currently demonstrated, and what should not be implied beyond the current proof lane.

## Core claim

KERNHELM is a kernel-bound authority model for untrusted requestors.

The requestor may be an AI agent, script, automation layer, provider-supplied agent, compromised dependency, or other userland process whose behavior is not fully predictable. KERNHELM does not try to decide whether that actor is honest, safe, or well-reasoned. It asks whether the privileged effect being attempted has admitted authority from a separate trusted path.

The intended security primitive is:

```text
untrusted requestor
  -> concrete plan
  -> trusted authorizer
  -> signed, scoped permit
  -> trusted bridge
  -> bounded kernel-side allow-state
  -> kernel wall checks the actual effect/target at enforcement time
```

If the effect does not fit the admitted authority shape, it fails closed.

## Current proof scope

The current proof lane demonstrates the authority shape at file-object and execution boundaries.

The live kernel wall in the current proof lane covers:

- protected file-object access via `file_open`
- execution control via `bprm_check_security`
- exact unlink/delete enforcement via `inode_unlink`
- target identity derived from the kernel-visible object at enforcement time
- deny-first behavior for governed targets without matching allow-state
- rights, deadline, stance, and revocation-epoch checks
- ring-buffer receipts for wall decisions

This proof lane is enough to demonstrate the core authority primitive, but it is not a claim that every privileged Linux surface is governed today.

## Architecture claims

The architecture claims are these:

1. **The requestor does not mint its own authority.**  
   Untrusted code may request, propose, or report, but it cannot mint, widen, renew, or self-apply the authority the wall accepts.

2. **Authority is plan-bound.**  
   A permit binds to a concrete plan identity through a plan hash. Changing the plan changes the identity that was authorized.

3. **Authority is effect-scoped.**  
   A permit is not a standing admin role. It is scoped to specific effect rights, targets, deadlines, caps, stance requirements, and revocation state.

4. **The kernel wall enforces admitted authority, not the requestor's story.**  
   The trusted authorizer performs policy and signing work. The trusted bridge injects bounded allow-state. At the hook, the wall checks that live state against the object and effect actually being attempted.

5. **File-object target matching is not path-string trust.**  
   For current file-object enforcement, the wall derives target identity from the kernel-visible object identity. If execution reaches a different object than the one admitted, the authority no longer fits.

6. **Permits only shrink when delegated.**  
   Child authority may be narrower than the parent authority. It must not widen rights, targets, caps, stance, deadlines, or hop limits.

7. **Revocation is enforced at the wall.**  
   Stale or revoked authority does not keep working simply because the requestor still holds old material.

8. **Stances are posture inputs, not vibes.**  
   Stance state affects enforcement posture and record-retention behavior. Missing or insufficient stance state should not become a reason to get permissive.

9. **Receipts matter.**  
   Deny, allow, mint, revoke, and related events are meant to leave an audit trail tied back to the plan/authority identity, subject to the selected retention posture.

## Expansion targets, not current proof claims

The same primitive is intended to extend to other privileged effect classes when those surfaces are implemented and proven.

Design targets include:

- network egress
- process inspection/control such as ptrace-like surfaces
- device access
- brokered tool/facade effects
- microVM or worker-lane authority
- package/system mutation surfaces
- multi-agent or delegated worker authority

These are not all current proof claims. They should be described as expansion targets unless the repo contains the corresponding enforcement hook, test, and receipt evidence.

## What is not claimed

KERNHELM is not claimed to be:

- a prompt-injection detector
- a model-behavior classifier
- a replacement for human judgment about what should be approved
- a kernel-exploit defense
- a defense against a malicious trusted authorizer
- a defense against signing-key compromise by itself
- a claim that file contents are frozen if the same inode changes after approval
- a claim that network, ptrace, device, or desktop-capture enforcement is live in the current proof lane unless separately shown
- a production-hardening completion claim
- a commercial-readiness claim
- a statement that SELinux, AppArmor, seccomp, capabilities, or sandboxing are useless
- a guarantee that all misuse is impossible

The narrower claim is stronger:

> Under the stated threat model, privileged effects governed by the wall require admitted authority from the trusted path. If that authority is missing, stale, too broad, too weak, revoked, or mismatched to the actual effect/target, the effect fails closed.

## Safe wording

Use wording like this:

> The current proof lane demonstrates a kernel-bound authority wall for file-object access, execution, and exact unlink enforcement. The broader architecture applies the same plan-bound permit model to additional privileged effect classes as those hooks are implemented and proven.

Avoid wording like this:

> KERNHELM makes exploitation physically impossible.

Avoid wording like this unless the specific hook exists and is proven:

> KERNHELM currently governs all network, ptrace, device, and desktop-capture effects.

## Review requested

The useful critique is hostile and technical:

- where is the claim boundary too broad?
- where does the current proof not support the public wording?
- where does the threat model rely on an unstated assumption?
- where does this duplicate an existing Linux mechanism?
- which effect class should be proven next to make the architecture harder to dismiss?
