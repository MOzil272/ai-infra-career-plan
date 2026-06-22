# Local Sandbox Runner — Clarified Week 1 Requirements

## Short Answer

Do **not** start by deploying the FastAPI app as a Docker image.

For Week 1, run the FastAPI app locally on your machine. The FastAPI app is the **control plane**. It uses Docker SDK to ask the local Docker daemon to create short-lived **task containers**.

Later, you can containerize the FastAPI service itself. That is not the Week 1 goal.

---

## Mental Model

There are two different things:

## 1. Runner Service

This is your FastAPI app.

It receives API requests and controls execution.

```text
curl / client
  -> FastAPI runner service
  -> Docker Engine API
  -> create task container
```

## 2. Task Container

This is the container created for one user/agent task.

Example:

```text
image: python:3.12-slim
command: ["python", "-c", "print(1 + 1)"]
```

The task container is created, runs, returns stdout/stderr/exit code, and is removed.

---

## Week 1 Architecture

```text
User / curl
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
Short-lived task container
```

The FastAPI app itself does **not** need to run in Docker for Week 1.

---

## What You Should Build

Build a local API service:

```text
GET /health
POST /tasks
```

The service should run commands inside Docker containers and return structured execution results.

---

## What You Should Not Build Yet

Do not build these in Week 1:

```text
Kubernetes
EKS
Terraform
Cloud deployment
CI/CD
async job queue
database persistence
user auth
gRPC
production security sandbox
containerized runner service
```

Those come later.

---

## API Contract

## GET /health

Response:

```json
{
  "ok": true
}
```

---

## POST /tasks

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

## Status Values

Use:

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

## Required Test Cases

## 1. Successful command

Input:

```json
{
  "image": "python:3.12-slim",
  "command": ["python", "-c", "print(1 + 1)"],
  "timeout_seconds": 10
}
```

Expected:

```text
status = COMPLETED
exit_code = 0
stdout contains "2"
stderr empty
```

---

## 2. Failed command

Input:

```json
{
  "image": "python:3.12-slim",
  "command": ["python", "-c", "raise Exception('boom')"],
  "timeout_seconds": 10
}
```

Expected:

```text
status = FAILED
exit_code != 0
stderr contains traceback
```

---

## 3. Timeout command

Input:

```json
{
  "image": "python:3.12-slim",
  "command": ["python", "-c", "import time; time.sleep(100)"],
  "timeout_seconds": 3
}
```

Expected:

```text
status = TIMEOUT
container is stopped/removed
```

---

## 4. Invalid image

Input:

```json
{
  "image": "not-a-real-image:latest",
  "command": ["echo", "hello"],
  "timeout_seconds": 10
}
```

Expected:

```text
status = INTERNAL_ERROR or FAILED
error message explains image problem
```

---

## Basic Implementation Plan

## Step 1 — FastAPI Skeleton

Create:

```text
app/main.py
app/models.py
app/runner.py
```

`main.py` owns API endpoints.

`models.py` owns request/response models.

`runner.py` owns Docker execution logic.

---

## Step 2 — Docker Runner Function

Implement:

```python
def run_task(image: str, command: list[str], timeout_seconds: int) -> TaskResult:
    ...
```

It should:

```text
create container
run command
wait for completion
capture stdout/stderr
capture exit code
measure duration
remove container
return result
```

---

## Step 3 — Timeout Handling

If timeout happens:

```text
stop container
remove container
return TIMEOUT
```

---

## Step 4 — Safety Defaults

For Week 1:

```text
no host mounts
no secret passing
no local env propagation
network disabled if easy
memory limit if easy
CPU limit if easy
```

Important:

```text
Docker is not a perfect production sandbox.
This is a learning sandbox runner.
Production-grade isolation comes later with Kubernetes, gVisor, Firecracker, Kata, etc.
```

---

## Optional Later Step — Containerize the Runner Service

After the local runner works, you can package the FastAPI app into a Docker image.

That would look like:

```text
curl
  -> FastAPI container
  -> mounted Docker socket
  -> Docker daemon
  -> task container
```

But this requires mounting:

```text
/var/run/docker.sock
```

That is powerful and dangerous. It effectively gives the FastAPI container control over the host Docker daemon.

So do not do this first.

---

## Interview Explanation

A good English explanation:

> In Week 1, I am not trying to deploy the FastAPI service itself. I am building a local control-plane service. The service exposes a `/tasks` API and uses the Docker Engine API to launch short-lived task containers. Each task container runs one command, captures stdout, stderr, exit code, and duration, and is then removed. This gives me the first version of an agent execution platform. Later, I can move the execution layer to Kubernetes Jobs and improve isolation with stronger sandboxing technologies.

---

## Definition of Done

Week 1 local sandbox runner is done when:

- [ ] FastAPI app runs locally
- [ ] `GET /health` works
- [ ] `POST /tasks` works
- [ ] successful command returns stdout/stderr/exit code
- [ ] failed command returns stderr and non-zero exit code
- [ ] timeout command is stopped and cleaned up
- [ ] containers do not remain running after task completion
- [ ] at least 3 tests pass
- [ ] README includes curl examples
- [ ] design note explains architecture and non-goals
