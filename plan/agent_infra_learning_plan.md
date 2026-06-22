# Agent Infrastructure Learning Plan

**Goal:** Prepare for Agent Infrastructure roles while building cluster-grade foundations where the knowledge overlaps with Cluster Infrastructure.

**Time budget**
- Active study: ~2 hours/day
- Commute reading/review: ~1–1.5 hours/day
- Duration: 16 weeks / about 3–4 months

---

## Target Positioning

Current positioning:

> Backend / product SDE with AWS production operations experience, cross-account connectivity debugging, load balancer / HTTP-path analysis, and strong interest in infra.

Target positioning:

> Agent Infrastructure Engineer with cluster-grade foundations in containers, Kubernetes, networking, orchestration, reliability, security, and observability.

The goal is not to become a prompt engineer or generic AI app developer. The goal is to become the person who can make agents run safely, reliably, observably, and at scale in production.

---

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
fleet lifecycle, node lifecycle, CNI/eBPF, multi-cluster, BGP, GPU cluster basics.
```

Temporarily de-prioritized:

```text
Deep Inference / GPU / CUDA / NCCL / RDMA internals
```

Only learn inference/GPU concepts lightly for now.

---

## Daily Study Structure

### Active study: 2 hours/day

```text
45 min  LeetCode
75 min  Infra / project
```

LeetCode is non-negotiable and continues every month.

### Commute: 1–1.5 hours/day

Use commute for:
- Reading
- Official docs
- Architecture diagrams
- Design notes
- Flashcards
- Reviewing yesterday's project / bugs / concepts

Avoid commute coding. Use it for input and consolidation.

---

## LeetCode Plan

### Goal

```text
100–120 problems total
Mediums stable within 35–40 minutes
Core patterns reproducible under interview pressure
```

### Weekly rhythm

```text
Monday:    New problem
Tuesday:   New problem
Wednesday: Review old problem + one smaller/new problem
Thursday:  New problem
Friday:    New problem
Saturday:  Timed mock, 2 problems / 90 min
Sunday:    Mistake review + pattern notes
```

### Review states

```text
NEW
REVIEW_1_DAY
REVIEW_7_DAY
REVIEW_21_DAY
DONE
```

For each problem, record:
- Pattern
- Key insight
- Why it was hard / why it went wrong
- Complexity
- Similar problems

### Pattern order

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

---

## Main Project

Build a small but real:

```text
Mini Agent Execution Platform
```

Do not build a ChatGPT clone.

Build the backend execution platform that resembles a small version of Codex / Claude Code infrastructure.

---

## Project Milestones

### V1: Local Sandbox Runner

Capabilities:
- Accept task request
- Start isolated container
- Execute command
- Enforce CPU / memory / timeout
- Collect stdout / stderr
- Return structured result

Concepts:
- Linux process model
- Namespace
- cgroup
- seccomp
- container image
- container runtime
- filesystem isolation
- network egress control

Suggested stack:
- Python
- FastAPI
- Docker SDK
- SQLite or Postgres
- Simple task table

---

### V2: Kubernetes Job Runner

Capabilities:
- Convert task into Kubernetes Job
- Per-task Pod
- Resource requests / limits
- Timeout / cancellation
- Log collection
- RBAC / ServiceAccount
- NetworkPolicy
- Namespace or label-based isolation

Concepts:
- Pod lifecycle
- Job
- Deployment
- Service
- Ingress
- ConfigMap / Secret
- RBAC
- ServiceAccount
- ResourceQuota
- Scheduler basics
- Controller loop
- etcd basics
- CNI basics
- Pod-to-pod and Pod-to-service paths

---

### V3: Durable Workflow

Capabilities:
- Task state machine
- Retry
- Timeout
- Checkpoint
- Resume
- Cancel
- Dead-letter queue

Example task states:

```text
PENDING
RUNNING
WAITING_FOR_TOOL
FAILED_RETRYABLE
FAILED_FINAL
COMPLETED
CANCELLED
```

Concepts:
- Step Functions
- Temporal
- durable execution
- workflow history
- deterministic replay
- idempotency
- compensation
- event log
- state machine

---

### V4: Agent Observability

Capabilities:
- Tool-call trace
- Sandbox execution trace
- stdout / stderr
- token / cost trace
- permission decision log
- failure reason
- retry path
- correlation ID
- structured task timeline

Concepts:
- logs
- metrics
- traces
- OpenTelemetry
- SLO
- alerting
- high-cardinality metrics
- incident review
- runbook

---

### V5: Cluster-grade Extension

Capabilities:
- Node pool concept
- Task placement rule
- Resource quota
- Simulated node failure
- Reschedule task after failure
- Node drain simulation
- Network failure simulation
- Simple capacity dashboard
- Cross-account tool access model

Concepts:
- fleet lifecycle
- node lifecycle
- cordon / drain
- cluster autoscaler
- scheduler queue
- CNI deeper dive
- Cilium / eBPF overview
- service mesh overview
- BGP basics
- Direct Connect / Transit Gateway system model
- GPU cluster concepts
- NCCL / RDMA / InfiniBand basics

---

## Control Plane Extension

Use Azure network infra friend as a control-plane mentor.

Focus on control plane, not deep data plane.

### Topics to learn

```text
desired state
actual state
resource lifecycle
reconciliation loop
config propagation
eventual consistency
validation
admission
rollback
canary
blast radius control
audit log
dependency graph
idempotent operation
multi-tenant isolation
quota
rate limit
```

### Data plane depth limit

Learn enough to debug:

```text
HTTP / TCP path
DNS
load balancer behavior
routing
CNI / eBPF role
timeout vs reset
latency / packet loss symptoms
```

Do not go deep into:

```text
DPDK
kernel bypass
NIC offload
XDP internals
RDMA verbs
packet processing implementation
```

---

## Suggested Azure Network Friend Sessions

### Session 1: Network Control Plane Overview

Ask:
- How does a VNet / peering / private endpoint go from API request to active connectivity?
- How is desired state represented?
- How does actual state converge?
- How is failure handled?
- How is config propagated safely?

Expected output:
- One architecture diagram
- One design note

---

### Session 2: Connectivity Debugging

Ask:
- How do you systematically debug cross-account / cross-subscription connectivity?
- How do you order DNS, route, policy, LB, backend health, auth?
- Which metrics are useful?
- Which metrics are misleading?

Expected output:
- Debugging checklist
- Request path diagram

---

### Session 3: Blast Radius / Rollback

Ask:
- How does the network control plane limit blast radius?
- How is canary done?
- How does rollback work?
- How do you prevent bad config fanout?
- How are audit logs designed?

Expected output:
- Control-plane safety note
- Failure-mode table

---

### Session 4: Design Review

Use the project:

> I have a Kubernetes-based agent sandbox execution platform. Each task needs network isolation, egress control, and cross-account tool access. How would you design the network control plane?

Expected output:
- Design review notes
- Follow-up learning list

---

## 16-Week Roadmap

### Month 1: Container / Sandbox + LeetCode Foundations

Infra:
- Linux process model
- namespace / cgroup / seccomp
- Docker / container lifecycle
- local sandbox runner
- CPU / memory / timeout limits
- stdout / stderr capture

LeetCode:
- Array / HashMap
- Two pointers
- Sliding window
- Stack
- Binary search
- Intervals
- Tree basics

Commute:
- Docker docs
- Linux containers
- Kubernetes basics
- DDIA chapters on replication / partitioning if time allows

Deliverables:
- V1 local sandbox runner
- 25–30 LeetCode problems
- 2 design notes

---

### Month 2: Kubernetes + Networking + Continuous LeetCode

Infra:
- K8S Job runner
- Pod lifecycle
- Service / Ingress
- RBAC
- NetworkPolicy
- CNI basics
- K8S task cancellation
- Log collection

Networking:
- DNS
- TLS / HTTP lifecycle
- ALB / NLB
- target group health
- cross-account connectivity
- PrivateLink / VPC peering / TGW
- route table / SG / NACL
- IAM boundary

LeetCode:
- Tree DFS / BFS
- Graph BFS / DFS
- Heap
- Backtracking
- Topological sort
- Union Find

Commute:
- K8S official docs
- K8S networking
- Azure/AWS network control plane notes
- Friend sessions 1–2

Deliverables:
- V2 Kubernetes Job runner
- 25–30 additional LeetCode problems
- Request-path diagram
- Cross-account connectivity debugging note

---

### Month 3: Durable Workflow + Observability + Security

Infra:
- Step Functions vs Temporal
- durable execution
- checkpoint / retry / resume
- task state machine
- DLQ
- idempotency
- compensation
- event log

Observability:
- logs / metrics / traces
- OpenTelemetry
- SLO
- alerting
- agent trajectory
- tool call trace
- sandbox stdout / stderr
- token / cost trace
- permission decision trace

Security:
- IAM
- STS
- cross-account role
- least privilege
- secret management
- KMS
- audit log
- RBAC
- sandbox egress policy
- delegated auth

LeetCode:
- DP
- Greedy
- Prefix sum
- Binary search on answer
- Mixed graph/tree problems

Commute:
- Temporal docs
- SRE book
- OpenTelemetry docs
- IAM / STS docs
- Friend session 3

Deliverables:
- V3 durable workflow
- V4 agent observability
- 20–25 additional LeetCode problems
- Control-plane safety note

---

### Month 4: Cluster-grade Extension + Interview Prep

Infra:
- node lifecycle
- cordon / drain
- scheduler basics
- autoscaling
- CNI deeper dive
- Cilium / eBPF overview
- service mesh overview
- BGP basics
- Direct Connect / Transit Gateway system model
- GPU cluster concepts
- NCCL / RDMA / InfiniBand basics

Project:
- node pool concept
- task placement rule
- quota
- simulated node failure
- task reschedule
- network failure simulation
- capacity dashboard

Interview:
- system design prompts
- resume bullets
- project demo
- mock interviews
- LeetCode mixed review

LeetCode:
- mixed mock
- old mistakes
- timed medium
- occasional hard
- company-style mock

Commute:
- Cluster Infra / Fleet notes
- Control-plane design notes
- Friend session 4

Deliverables:
- V5 cluster-grade extension
- 20 additional LeetCode problems
- final design doc
- resume bullet set
- mock interview notes

---

## Weekly Design Prompts

Start from Week 5.

1. Design a sandboxed code execution service
2. Design a durable workflow system
3. Design a task queue for long-running jobs
4. Design observability for agent execution
5. Design cross-account tool access
6. Design a Kubernetes-based job runner
7. Design a cluster capacity dashboard
8. Design a secure multi-tenant execution platform

---

## Final Interview Story

Target narrative:

> I started as a product/backend SDE maintaining production systems at Amazon. I often handled cross-account connectivity, load balancer metrics, HTTP request path debugging, and production reliability issues.  
>
> To move toward Agent Infrastructure, I systematized that infra experience and built a mini agent execution platform. It supports sandboxed execution, Kubernetes job scheduling, durable workflow, checkpoint/recovery, agent observability, audit logs, and network isolation.  
>
> I intentionally learned the shared foundation to cluster-grade depth: containers, Kubernetes, networking, workflow orchestration, reliability, security, and control-plane design. This gives me a strong bridge from product-side reliability into AI infrastructure.

---

## Success Criteria

By the end of 16 weeks:

- 100–120 LeetCode problems completed
- All core LeetCode patterns reviewed
- Mini Agent Execution Platform V1–V5 completed
- 8–10 design notes written
- 4 control-plane mentoring sessions completed
- Resume updated with honest infra-heavy story
- Able to discuss Agent Infra and adjacent Cluster Infra design with confidence
