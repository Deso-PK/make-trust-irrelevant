# KERNHELM Measurements

This file records the public measurement boundary for the current KERNHELM proof lane.

The purpose is not to claim production maturity. The purpose is to keep the performance claim precise enough that hostile readers know exactly what was measured and what was not.

## Source boundary

The current numbers come from the W6.7 proof-mode metrics addendum.

Important boundary:

- proof-mode wrapper metrics
- PID-scoped proof wall
- 25 iterations per reported wall/revoke metric
- not a production-shipping object
- not a full commercial hardening benchmark
- not a measurement of every future effect class

The safe public performance claim is:

> The current proof-mode wall hot path measures at single-digit microseconds at p95 for the demonstrated allow/deny checks.

## Hot-path wall check

| Metric | Iterations | p50 | p95 | Max |
|---|---:|---:|---:|---:|
| Wall hot-path deny | 25 | 2.790 µs | 4.550 µs | 17.241 µs |
| Wall hot-path allow | 25 | 3.120 µs | 7.010 µs | 16.800 µs |

These values measure the proof-mode wall check path for the demonstrated current wall surface. They should not be described as full user approval latency, plan evaluation latency, signing latency, UI latency, Drawbridge/admin latency, or production benchmark latency.

## Revocation measurements

| Metric | Iterations | p50 | p95 | Max |
|---|---:|---:|---:|---:|
| Revoke total | 25 | 15.763 ms | 17.540 ms | 19.802 ms |
| Revoke inner | 25 | 2.612 ms | 4.389 ms | 6.652 ms |

The safe public revocation claim is:

> In this proof-mode wrapper run, revoke total p95 measured in the tens of milliseconds.

Do not turn this into a production service-level guarantee.

## Heartbeat/cadence measurement

| Metric | Iterations | p50 | p95 | Max |
|---|---:|---:|---:|---:|
| Heartbeat interval | 25 | 250.003 ms | 252.009 ms | 252.299 ms |

This shows the proof cadence stayed near the configured 250 ms period with small scheduler jitter in the measured run. This is in the pre-boot configuration menu's not the live enviroment. 

## What the numbers mean

The numbers support this narrow statement:

> For the current proof-mode wall, the hot-path deny and allow checks measured in single-digit microseconds at p95 in the recorded run.

The numbers do not prove:

- production hardening is finished
- every future effect class has the same cost
- network, ptrace, device, or desktop-capture enforcement is currently measured
- authorizer planning and signing are microsecond operations
- human approval is microsecond latency
- Drawbridge/admin apply latency is runtime hot-path latency
- the proof BPF object is production-shipping ore

## Recommended public wording

Use:

> Proof-mode hot-path wall measurements: deny p50 2.79 µs / p95 4.55 µs; allow p50 3.12 µs / p95 7.01 µs. These are proof-mode wall-check measurements, not final production-hardening benchmarks.

Avoid:

> KERNHELM adds no overhead.

Avoid:

> KERNHELM performs full cryptographic signature verification in the kernel hot path at single-digit microseconds.

Avoid:

> These numbers prove all future surfaces will have the same performance.

## Measurement hygiene

Every public use of these numbers should carry three labels:

1. **proof-mode**
2. **current wall hot path**
3. **not production hardening**

That keeps the claim sharp without handing hostile readers a free overclaim.
