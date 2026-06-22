# Week 1 Commute Resources

## Purpose

Use commute time for reading, watching, and building mental models.

Do not try to code during commute. The goal is to understand enough to implement the local sandbox runner during active study time.

Week 1 focus:

```text
FastAPI local control plane
Docker Engine API
Docker SDK for Python
short-lived task containers
stdout / stderr / exit code
timeout and cleanup
basic resource limits
```

---

## Reading Strategy

Read in this order:

```text
1. FastAPI basics
2. Docker image/container basics
3. Docker SDK for Python
4. Docker Containers API
5. Docker resource constraints
```

Do not read all Docker docs randomly. Stay focused on what is needed for the Week 1 sandbox runner.

---

# Day-by-Day Commute Plan

## Day 1 — FastAPI + Docker Concepts

### Required

1. FastAPI First Steps  
   https://fastapi.tiangolo.com/tutorial/first-steps/

2. Docker: What is an image?  
   https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/

3. Docker: What is a container?  
   https://www.docker.com/resources/what-container/

### Goal

By the end of Day 1 commute, you should be able to explain:

```text
FastAPI app = API service
Docker image = template / package
Docker container = running instance of an image
```

---

## Day 2 — Docker SDK for Python

### Required

1. Docker SDK for Python  
   https://docker-py.readthedocs.io/

2. Docker SDK Containers API  
   https://docker-py.readthedocs.io/en/stable/containers.html

### Goal

By the end of Day 2 commute, you should understand:

```text
client = docker.from_env()
client.containers.run(...)
detach=True
container.start()
container.wait()
container.logs()
container.remove()
```

This is the core API surface for Week 1.

---

## Day 3 — FastAPI Request Body / Pydantic

### Required

1. FastAPI Request Body  
   https://fastapi.tiangolo.com/tutorial/body/

2. FastAPI Body - Multiple Parameters  
   https://fastapi.tiangolo.com/tutorial/body-multiple-params/

### Goal

By the end of Day 3 commute, you should understand how this request maps to a Pydantic model:

```json
{
  "image": "python:3.12-slim",
  "command": ["python", "-c", "print(1 + 1)"],
  "timeout_seconds": 10
}
```

---

## Day 4 — Running Containers + Resource Limits

### Required

1. Docker: Running containers  
   https://docs.docker.com/engine/containers/run/

2. Docker: Resource constraints  
   https://docs.docker.com/engine/containers/resource_constraints/

### Goal

By the end of Day 4 commute, you should understand:

```text
command and args
container options
memory limit
CPU limit
why unconstrained containers are risky
```

---

## Day 5 — Docker Engine API / SDK Mental Model

### Required

1. Docker: Develop with Docker Engine SDKs  
   https://docs.docker.com/reference/api/engine/sdk/

2. Docker: Engine SDK examples  
   https://docs.docker.com/reference/api/engine/sdk/examples/

3. Docker SDK Containers API  
   https://docker-py.readthedocs.io/en/stable/containers.html

### Goal

By the end of Day 5 commute, you should be able to explain:

```text
Docker CLI is not the only interface.
Python can call the Docker Engine API through Docker SDK.
FastAPI can act as a small control plane over Docker.
```

---

## Day 6 — Review + Fill Gaps

### Required Review

Re-read:

1. Docker SDK Containers API  
   https://docker-py.readthedocs.io/en/stable/containers.html

2. Docker: Resource constraints  
   https://docs.docker.com/engine/containers/resource_constraints/

3. FastAPI Request Body  
   https://fastapi.tiangolo.com/tutorial/body/

### Goal

Use this day to answer:

```text
How do I create a container?
How do I wait for it?
How do I collect logs?
How do I handle timeout?
How do I remove the container?
How does the request JSON become a Python object?
```

---

## Day 7 — Design Note Review

### Required

No new reading unless behind.

Review your own notes and write:

```text
notes/week1_local_sandbox_design.md
```

### Design Note Checklist

Your design note should include:

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

---

# Optional YouTube Videos

Use videos only when docs feel dry. Do not let YouTube replace implementation.

## FastAPI

1. Python FastAPI Tutorial - Part 1  
   https://www.youtube.com/watch?v=7AMjmCTumuo

2. Python FastAPI Tutorial: Build a REST API in 15 Minutes  
   https://www.youtube.com/watch?v=iWS9ogMPOI0

## Docker

1. Docker Tutorial for Beginners  
   https://www.youtube.com/watch?v=b0HMimUb4f0

2. Docker Crash Course for Absolute Beginners  
   https://www.youtube.com/watch?v=pg19Z8LL06w

3. Docker Tutorial for Beginners - Full Course in 3 Hours  
   https://www.youtube.com/watch?v=3c-iBn73dDE

### Video Rule

```text
Watch at 1.25x or 1.5x.
Do not take detailed notes.
Extract only the concept needed for Week 1.
```

---

# Minimum Required Reading

If time is tight, only read these five:

1. FastAPI First Steps  
   https://fastapi.tiangolo.com/tutorial/first-steps/

2. FastAPI Request Body  
   https://fastapi.tiangolo.com/tutorial/body/

3. Docker: What is an image?  
   https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/

4. Docker SDK for Python  
   https://docker-py.readthedocs.io/

5. Docker SDK Containers API  
   https://docker-py.readthedocs.io/en/stable/containers.html

---

# What You Should Be Able To Say After Week 1

```text
I built a local sandbox runner as the first version of an agent execution platform.

The FastAPI service acts as a local control plane. It receives a task request,
validates the request body with Pydantic, and uses the Docker SDK for Python
to call the Docker Engine API.

For each task, it creates a short-lived container from the requested image,
runs the command, captures stdout, stderr, exit code, and duration,
then removes the container.

This gives me the first execution loop for agent tasks. Later, this local Docker runner
can evolve into a Kubernetes Job runner with better isolation, resource quota,
NetworkPolicy, RBAC, and stronger sandboxing.
```

---

# Notes Template

Use this during commute:

```markdown
# Commute Notes — Day X

## Resource

## Key concepts

-

## How this maps to the sandbox runner

-

## Confusing parts

-

## Question to ask ChatGPT

-
```
