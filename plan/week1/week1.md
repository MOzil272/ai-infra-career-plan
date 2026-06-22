# Week 1 Plan — Local Sandbox Runner + LeetCode

## Week 1 Theme

Build the first minimal version of an agent execution platform.

The key mental model:

```text
FastAPI app = local control plane
Docker task container = per-task execution unit
```

Do **not** start by deploying the FastAPI app as a Docker image.

For Week 1, run FastAPI locally. The FastAPI service calls the Docker Engine API through the Python Docker SDK to create, start, wait for, inspect, log, and remove short-lived containers.

---

## Architecture

```text
curl / client
    |
    v
FastAPI app running on localhost
    |
    v
Docker SDK for Python
    |
    v
Local Docker daemon
    |
    v
Short-lived task container created from requested image
```

Example:

```json
{
  "image": "python:3.12-slim",
  "command": ["python", "-c", "print(1 + 1)"],
  "timeout_seconds": 10
}
```

FastAPI receives this request, then creates a container from `python:3.12-slim`, runs the command, captures stdout/stderr/exit code, removes the container, and returns a structured result.

---

## What This Is

This is a small local version of:

```text
agent task execution
sandboxed command execution
tool execution backend
future Kubernetes Job runner
```

This is the first step toward a production agent execution platform.

---

## What This Is Not

Week 1 is **not** about:

```text
Dockerizing the FastAPI runner service
deploying to cloud
Kubernetes
EKS
Terraform
Temporal
gRPC
async queue
frontend UI
production-grade sandbox security
```

Those come later.

---

## Daily Time Budget

Because Week 1 includes two LeetCode questions per day:

```text
60 min  LeetCode
60 min  sandbox runner project
Commute: docs, notes, review
```

If a day is too busy:

```text
30 min  LeetCode review
60 min  project
```

Do not skip LeetCode completely.

---

# Part 1 — LeetCode Plan

## Study-plan Basis

Week 1 follows the common pattern-based structure used by popular interview prep plans such as NeetCode 150, Blind 75, and LeetCode Top Interview 150.

Week 1 patterns:

```text
Arrays & Hashing
Two Pointers
Sliding Window
```

## Rule

If one problem takes more than 35 minutes:

```text
stop
read explanation
implement once
write the key insight
mark for review
```

The goal is pattern recognition, not getting stuck for two hours.

---

## Day 1 — Arrays & Hashing Basics

### Problems

1. **217. Contains Duplicate**
2. **242. Valid Anagram**

### Learn

```text
Set membership
HashMap frequency count
O(n) time / O(n) space
```

### Project

Create project skeleton:

```text
mini-agent-execution-platform/
  app/
    main.py
    models.py
    runner.py
  tests/
  notes/
  requirements.txt
  README.md
```

Create local FastAPI `/health` endpoint.

### Output

```text
GET /health -> { "ok": true }
```

---

## Day 2 — HashMap Lookup + Grouping

### Problems

3. **1. Two Sum**
4. **49. Group Anagrams**

### Learn

```text
HashMap lookup
Complement lookup
Canonical grouping key
Sorting key vs frequency tuple
```

### Project

Install dependencies:

```text
fastapi
uvicorn
docker
pydantic
pytest
```

Create `runner.py` with a rough function:

```python
def run_task(image: str, command: list[str], timeout_seconds: int):
    ...
```

For Day 2, it only needs to successfully run one container.

### Output

Run this through Python code, not necessarily API yet:

```text
image: python:3.12-slim
command: ["python", "-c", "print(1 + 1)"]
```

Expected stdout:

```text
2
```

---

## Day 3 — Frequency + Prefix/Suffix

### Problems

5. **347. Top K Frequent Elements**
6. **238. Product of Array Except Self**

### Learn

```text
Frequency map
Top-K: heap vs bucket
Prefix / suffix thinking
```

### Project

Define API models in `models.py`.

Request:

```json
{
  "image": "python:3.12-slim",
  "command": ["python", "-c", "print(1 + 1)"],
  "timeout_seconds": 10
}
```

Response:

```json
{
  "task_id": "task_123",
  "status": "COMPLETED",
  "exit_code": 0,
  "stdout": "2\n",
  "stderr": "",
  "duration_ms": 530
}
```

Create:

```text
POST /tasks
```

For Week 1, this endpoint can run synchronously.

### Output

`POST /tasks` works for a successful Python command.

---

## Day 4 — Two Pointers Basics

### Problems

7. **125. Valid Palindrome**
8. **167. Two Sum II - Input Array Is Sorted**

### Learn

```text
Two pointers
Sorted input invariant
Pointer movement reasoning
```

### Project

Add timeout handling.

Timeout test command:

```json
{
  "image": "python:3.12-slim",
  "command": ["python", "-c", "import time; time.sleep(100)"],
  "timeout_seconds": 3
}
```

Expected:

```json
{
  "status": "TIMEOUT",
  "exit_code": null,
  "stderr": "Task exceeded timeout_seconds=3"
}
```

Container must be stopped and removed after timeout.

### Output

```text
timeout works
container cleanup works
```

---

## Day 5 — Medium Two Pointers

### Problems

9. **15. 3Sum**
10. **11. Container With Most Water**

### Learn

```text
Sort + two pointers
Duplicate skipping
Area optimization
Explaining invariants in English
```

### Project

Add basic safety defaults.

Required:

```text
no host filesystem mount
no local environment variable propagation
no secrets
container cleanup after every run
```

Try if simple:

```text
network disabled
memory limit
CPU limit
```

Important:

```text
Docker is not a perfect production sandbox.
This is a learning sandbox runner.
Production-grade isolation comes later.
```

### Output

Basic safety choices documented in README or design note.

---

## Day 6 — Sliding Window Basics

### Problems

11. **3. Longest Substring Without Repeating Characters**
12. **424. Longest Repeating Character Replacement**

### Learn

```text
Variable-size sliding window
Window invariant
Left pointer update
```

For #3:

```text
Window invariant: no duplicate characters
```

For #424:

```text
Window is valid if:
window_size - max_frequency <= k
```

### Project

Add tests.

Required tests:

```text
test_success_command
test_failed_command
test_timeout_command
test_container_cleanup_after_timeout
```

Add README examples:

```text
curl /health
curl POST /tasks success
curl POST /tasks failure
curl POST /tasks timeout
```

### Output

At least 3 tests pass.

---

## Day 7 — Sliding Window Review + Hard Exposure

### Problems

13. **567. Permutation in String**
14. **76. Minimum Window Substring**

### Learn

```text
Frequency map inside sliding window
Need map vs window map
have / need counters
expand right
shrink left while valid
```

#76 is hard. Do not spend more than 35 minutes stuck. It is okay if you only understand and implement the template this week.

### Project

Write design note:

```text
notes/week1_local_sandbox_design.md
```

Suggested sections:

```text
Problem
Requirements
Non-goals
API contract
Architecture
Failure modes
Security considerations
Testing plan
Future work
```

### Output

Week 1 design note completed.

---

# Part 2 — Local Sandbox Runner Requirements

## Functional Requirements

### R1 — Health Endpoint

```text
GET /health
```

Response:

```json
{
  "ok": true
}
```

---

### R2 — Submit Task

```text
POST /tasks
```

Request:

```json
{
  "image": "python:3.12-slim",
  "command": ["python", "-c", "print(1 + 1)"],
  "timeout_seconds": 10
}
```

Response:

```json
{
  "task_id": "task_123",
  "status": "COMPLETED",
  "exit_code": 0,
  "stdout": "2\n",
  "stderr": "",
  "duration_ms": 530
}
```

---

### R3 — Command Must Be a List

Use:

```json
"command": ["python", "-c", "print(1 + 1)"]
```

Avoid:

```json
"command": "python -c 'print(1 + 1)'"
```

Reason:

```text
A list-based command avoids unnecessary shell parsing and makes execution behavior more explicit.
```

---

### R4 — Status Values

Support:

```text
COMPLETED
FAILED
TIMEOUT
INTERNAL_ERROR
```

Optional internal states:

```text
PENDING
RUNNING
```

---

### R5 — Capture Execution Result

Capture:

```text
task_id
status
exit_code
stdout
stderr
duration_ms
```

---

### R6 — Timeout

If command exceeds `timeout_seconds`:

```text
stop container
remove container
return status = TIMEOUT
```

---

### R7 — Cleanup

After every task:

```text
remove container
do not leave running containers
do not leave temporary files
```

---

### R8 — Basic Isolation

Default:

```text
no host filesystem mount
no local environment variables
no secrets
```

Try if simple:

```text
network disabled
memory limit
CPU limit
```

---

### R9 — Error Handling

Handle:

```text
image pull failed
image not found
command failed
timeout
Docker daemon unavailable
invalid request
unexpected internal error
```

---

### R10 — Tests

Required tests:

```text
success command
failed command
timeout command
container cleanup after timeout
invalid request
```

---

# Part 3 — Demo Commands

## Success

```bash
curl -X POST localhost:8000/tasks \
  -H "Content-Type: application/json" \
  -d '{
    "image": "python:3.12-slim",
    "command": ["python", "-c", "print(1 + 1)"],
    "timeout_seconds": 10
  }'
```

Expected:

```text
status = COMPLETED
exit_code = 0
stdout contains "2"
```

---

## Failure

```bash
curl -X POST localhost:8000/tasks \
  -H "Content-Type: application/json" \
  -d '{
    "image": "python:3.12-slim",
    "command": ["python", "-c", "raise Exception(\"boom\")"],
    "timeout_seconds": 10
  }'
```

Expected:

```text
status = FAILED
exit_code != 0
stderr contains traceback
```

---

## Timeout

```bash
curl -X POST localhost:8000/tasks \
  -H "Content-Type: application/json" \
  -d '{
    "image": "python:3.12-slim",
    "command": ["python", "-c", "import time; time.sleep(100)"],
    "timeout_seconds": 3
  }'
```

Expected:

```text
status = TIMEOUT
container is stopped and removed
```

---

# Part 4 — What To Read Before / During Week 1

Read only enough to build and explain the runner.

## FastAPI

```text
FastAPI First Steps
FastAPI Request Body / Pydantic models
```

## Docker

```text
Docker overview
Image vs container
Docker container lifecycle
Docker SDK for Python
Docker SDK Containers API
Docker resource constraints
```

## Conceptual Notes

By the end of Week 1, you should understand:

```text
Docker image vs Docker container
stdout / stderr / exit code
timeout
container cleanup
why command list is safer than shell string
why Docker alone is not a perfect production sandbox
how local Docker runner maps to future Kubernetes Job runner
```

---

# Part 5 — Interview Explanation

By the end of Week 1, you should be able to say:

```text
I built a local sandbox runner as the first version of an agent execution platform.

The FastAPI service acts as a small local control plane. It exposes a /tasks API,
validates the request, and uses the Docker Engine API through the Python Docker SDK
to create a short-lived container from the image specified in the request.

The task container runs one command, captures stdout, stderr, exit code, and duration,
and is then removed. The key reliability concerns are timeout handling, cleanup,
consistent status reporting, and preserving enough execution context for debugging.

This is not a production-grade security sandbox yet. Later versions should move
execution to Kubernetes Jobs and improve isolation with NetworkPolicy, RBAC, resource quota,
and eventually stronger sandboxing technologies such as gVisor or Firecracker.
```

---

# Part 6 — Daily Sync Template

Use this template when checking in:

```markdown
# Daily Sync — Week 1 Day X

## LeetCode
- Problems attempted:
- Finished:
- Stuck on:
- Pattern learned:
- Need review:

## Sandbox Runner
- Finished:
- Current blocker:
- Next step:

## Reading / Notes
- Read:
- Key takeaway:

## English / Interview Practice
- One sentence I want to explain better:

## Tomorrow
- LeetCode:
- Project:
```

---

# Definition of Done

Week 1 is complete when:

- [ ] 14 LeetCode problem slots attempted
- [ ] all problem notes created
- [ ] FastAPI app runs locally
- [ ] `GET /health` works
- [ ] `POST /tasks` success case works
- [ ] failed command case works
- [ ] timeout case works
- [ ] containers are cleaned up after execution
- [ ] at least 3 tests pass
- [ ] project README has runnable curl examples
- [ ] `notes/week1_local_sandbox_design.md` completed
- [ ] Week 1 retrospective completed
