# Backend Interview Playbook

## Mission

This repository is a production-minded handbook for backend engineers who want to sharpen fundamentals, prepare for interviews, and strengthen engineering judgment.

The focus is not trivia. The goal is to explain trade-offs clearly, connect concepts to production behavior, and build answers that sound like an experienced backend engineer rather than a memorized textbook.

## Quick Navigation

- [Roadmap](ROADMAP.md)
- [Contributing Guide](CONTRIBUTING.md)
- [.NET](backend/dotnet/README.md)
- [Databases](backend/databases/README.md)
- [Distributed Systems](backend/distributed-systems/README.md)
- [Architecture](backend/architecture/README.md)
- [DevOps](backend/devops/README.md)
- [General Engineering](general/README.md)

## Intended Audience

- Backend engineers preparing for mid-level to senior-level interviews
- .NET engineers who want a structured study path
- Tech leads who want a teaching-oriented reference for mentoring
- Developers who want examples that connect theory to real systems

## Repository Structure

```text
backend-interview-playbook/
├─ README.md
├─ ROADMAP.md
├─ CONTRIBUTING.md
├─ templates/
├─ backend/
│  ├─ dotnet/
│  ├─ databases/
│  ├─ distributed-systems/
│  ├─ architecture/
│  └─ devops/
├─ general/
└─ assets/
```

## How To Use This Repository

Use the repository in three passes:

1. Read the fundamentals to build conceptual clarity.
2. Move to the interview folders to practice concise, structured answers.
3. Use the examples folders to connect abstract concepts to implementation choices.

The fundamentals files are the canonical place for concept explanations. The interview files are intentionally more direct and assume you are practicing delivery, trade-offs, and follow-up depth.

## Start Here

### If You Are Interviewing Soon

- [backend/dotnet/interview/dotnet.md](backend/dotnet/interview/dotnet.md)
- [backend/dotnet/interview/async-concurrency-threading.md](backend/dotnet/interview/async-concurrency-threading.md)
- [backend/databases/interview/sql.md](backend/databases/interview/sql.md)
- [backend/distributed-systems/interview/microservices.md](backend/distributed-systems/interview/microservices.md)
- [backend/distributed-systems/interview/kafka.md](backend/distributed-systems/interview/kafka.md)
- [backend/architecture/interview/system-design.md](backend/architecture/interview/system-design.md)
- [backend/devops/interview/docker-kubernetes.md](backend/devops/interview/docker-kubernetes.md)
- [general/interview/testing.md](general/interview/testing.md)

### If You Want A Deeper Technical Refresh

- [backend/dotnet/framework/aspnet-core.md](backend/dotnet/framework/aspnet-core.md)
- [backend/dotnet/framework/entity-framework.md](backend/dotnet/framework/entity-framework.md)
- [backend/databases/fundamentals/query-optimization.md](backend/databases/fundamentals/query-optimization.md)
- [backend/distributed-systems/fundamentals/consistency-and-reliability.md](backend/distributed-systems/fundamentals/consistency-and-reliability.md)
- [backend/architecture/fundamentals/rest-api-design.md](backend/architecture/fundamentals/rest-api-design.md)
- [backend/devops/fundamentals/monitoring-and-observability.md](backend/devops/fundamentals/monitoring-and-observability.md)
- [general/fundamentals/debugging-and-troubleshooting.md](general/fundamentals/debugging-and-troubleshooting.md)

## Suggested Study Path

Start with the sequence below if you want a clean progression:

1. General engineering fundamentals
2. C# and .NET internals
3. ASP.NET Core and Entity Framework
4. SQL and database design
5. Distributed systems and messaging
6. System design and architecture
7. DevOps, deployment, and operations

The detailed progression lives in [ROADMAP.md](ROADMAP.md).

## Why The Content Is Split Into Fundamentals, Interview, And Examples

Each topic is split intentionally:

- `fundamentals/` explains concepts, vocabulary, mechanisms, and trade-offs.
- `interview/` translates those concepts into question-and-answer form.
- `examples/` shows how ideas are applied in realistic backend scenarios.

This structure avoids a common documentation problem: repeating the same explanation in multiple styles across unrelated files. When topics overlap, the fundamentals file should stay the canonical explanation and the interview/example files should link back to it.

## Topic Map

| Area | Focus |
| --- | --- |
| [.NET](backend/dotnet/README.md) | C#, async, threading, DI, ASP.NET Core, EF Core, code structure |
| [Databases](backend/databases/README.md) | relational modeling, SQL, indexing, isolation, query tuning |
| [Distributed Systems](backend/distributed-systems/README.md) | microservices, Kafka, event-driven systems, consistency, resiliency |
| [Architecture](backend/architecture/README.md) | system design, REST APIs, scalability, availability |
| [DevOps](backend/devops/README.md) | CI/CD, Docker, Kubernetes, deployment safety, observability |
| [General](general/README.md) | testing, debugging, data structures, NoSQL, behavioral preparation |

## Contribution Note

Contributions are welcome, but consistency matters. Before adding content, read [CONTRIBUTING.md](CONTRIBUTING.md) and use the templates in [templates/](templates/).
