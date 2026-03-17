# Docker

## Overview

Docker packages applications and their runtime dependencies into portable container images. For backend engineers, Docker is mainly about consistent packaging and deployment, not about changing the architecture of the application itself.

## Core Concepts

- Images are immutable build artifacts. Containers are running instances of those images.
- Multi-stage builds reduce final image size and keep build tools out of runtime images.
- Containerization improves environment consistency but does not eliminate the need for correct configuration, logging, and health handling.
- Smaller, simpler images are generally easier to secure, ship, and debug.

## Why It Matters In Real Systems

Containers reduce "works on my machine" problems and make deployment more predictable. But a poorly built image can still be slow to build, large to transfer, and difficult to operate. Container behavior also affects startup, shutdown, and observability.

## Practical Example

A .NET service can be built in one stage using the SDK image and run in a lighter runtime image. This keeps the final artifact smaller and avoids shipping unnecessary tooling into production.

## Common Pitfalls

- building large images with unnecessary files
- baking secrets into images
- assuming containers solve process-level bugs or poor architecture
- ignoring graceful shutdown and health signaling

## Interview Takeaways

- Explain images versus containers clearly.
- Mention multi-stage builds, security, and operational implications.
- Show that Docker is a packaging and runtime concern, not magic scalability.

## Related Topics

- [kubernetes.md](kubernetes.md)
- [deployment-strategies.md](deployment-strategies.md)
- [../examples/dockerfile-best-practices.md](../examples/dockerfile-best-practices.md)
