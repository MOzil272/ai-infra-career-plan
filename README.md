# AI Infrastructure Career Plan

This repo tracks a 16-week learning plan for moving from a backend/product SDE background into **Agent Infrastructure**, while building enough cluster-grade foundation to keep **Cluster Infrastructure** as a realistic adjacent path.

## Goal

Prepare for roles such as:

- Agent Infrastructure Engineer
- Claude / Codex execution infrastructure engineer
- AI compute / execution platform engineer
- Adjacent Cluster Infrastructure / Fleet Infrastructure engineer

The focus is not on building a ChatGPT clone or becoming a prompt engineer. The focus is on building the infrastructure that lets agents execute safely, reliably, observably, and at scale.

## Background

Current positioning:

```text
Backend / product SDE
+ AWS production operations
+ cross-account connectivity debugging
+ load balancer / HTTP-path analysis
+ reliability and infra-heavy product ownership
```

Target positioning:

```text
Agent Infrastructure Engineer
+ cluster-grade foundation in containers, Kubernetes, networking,
  workflow orchestration, reliability, security, and observability
```

## Strategy

Main track:

```text
Agent Infrastructure
```

Depth rule:

```text
If a topic is shared by Agent Infrastructure and Cluster Infrastructure,
learn it to cluster-grade depth.
```

Final extension:

```text
After the shared foundation is solid,
add Cluster Infra-specific topics:
fleet lifecycle, node lifecycle, CNI/eBPF, multi-cluster,
BGP, GPU cluster basics.
```

Temporarily de-prioritized:

```text
Deep Inference / GPU / CUDA / NCCL / RDMA internals
```

Inference and GPU concepts will be learned lightly for context, not as the primary track.

## Time Budget

Duration:

```text
16 weeks / about 3–4 months
```

Daily active study:

```text
45 min  LeetCode
75 min  Infra / project
```

Commute:

```text
1–1.5 hours reading, review, design notes, docs, and architecture thinking
```

## Main Project

Build a small but real:

```text
Mini Agent Execution Platform
```

This project should evolve through five versions:

1. **Local Sandbox Runner**
   - Run tasks in isolated local containers.
   - Enforce CPU / memory / timeout.
   - Collect stdout / stderr.

2. **Kubernetes Job Runner**
   - Convert tasks into Kubernetes Jobs.
   - Add RBAC, resource quota, NetworkPolicy, and log collection.

3. **Durable Workflow**
   - Add retry, checkpoint, resume, cancellation, and DLQ.
   - Compare Step Functions and Temporal-style durable execution.

4. **Agent Observability**
   - Add tool-call trace, sandbox trace, token/cost trace, failure reason, audit log, and correlation ID.

5. **Cluster-grade Extension**
   - Add node pool concept, task placement rules, simulated node failures, network failure simulation, capacity dashboard, and cluster-level reliability thinking.

## LeetCode Track

LeetCode runs continuously throughout the 16 weeks.

Goal:

```text
100–120 problems total
Mediums stable within 35–40 minutes
Core patterns reproducible under interview pressure
```

Pattern order:

1. Array / HashMap
2. Two pointers / Sliding window
3. Stack / Queue
4. Binary search
5. Intervals
6. Tree DFS / BFS
7. Graph BFS / DFS
8. Heap / Priority Queue
9. Topological sort
10. Union Find
11. Backtracking
12. Dynamic Programming
13. Greedy
14. Mixed mock interviews

## Control Plane Extension

Use Azure network infra mentoring to learn control-plane concepts:

```text
desired state
actual state
resource lifecycle
reconciliation loop
config propagation
eventual consistency
validation
rollback
canary
blast radius control
audit log
dependency graph
idempotency
multi-tenant isolation
quota
rate limit
```

Data-plane depth is intentionally limited. Learn enough for debugging HTTP/TCP/LB/CNI paths, but do not go deep into DPDK, kernel bypass, XDP internals, RDMA verbs, or packet processing implementation unless the target changes.

## Documents

- [`agent_infra_learning_plan.md`](./agent_infra_learning_plan.md): Full 16-week learning plan.
- `week1.md`: To be created next.
- `notes/`: Future design notes and reading summaries.
- `leetcode/`: Future LeetCode pattern notes and mistakes.
- `project/`: Future Mini Agent Execution Platform implementation.

## Final Interview Narrative

Target story:

```text
I started as a product/backend SDE maintaining production systems at Amazon.
I often handled cross-account connectivity, load balancer metrics,
HTTP request path debugging, and production reliability issues.

To move toward Agent Infrastructure, I systematized that infra experience
and built a mini agent execution platform. It supports sandboxed execution,
Kubernetes job scheduling, durable workflow, checkpoint/recovery,
agent observability, audit logs, and network isolation.

I intentionally learned the shared foundation to cluster-grade depth:
containers, Kubernetes, networking, workflow orchestration, reliability,
security, and control-plane design. This gives me a strong bridge from
product-side reliability into AI infrastructure.
```

## Success Criteria

By the end of 16 weeks:

- 100–120 LeetCode problems completed
- All core LeetCode patterns reviewed
- Mini Agent Execution Platform V1–V5 completed
- 8–10 design notes written
- 4 control-plane mentoring sessions completed
- Resume updated with honest infra-heavy story
- Able to discuss Agent Infra and adjacent Cluster Infra design with confidence
