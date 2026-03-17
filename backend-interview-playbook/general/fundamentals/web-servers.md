# Web Servers

## Overview

Backend engineers should understand what a web server and an application server do, even when frameworks hide most of the details. Request handling, connection management, and proxying all affect behavior in production.

## Core Concepts

- A web server accepts network requests and can terminate TLS, serve static assets, or proxy traffic.
- An application server executes the backend application logic for routed requests.
- Reverse proxies and load balancers are common boundaries between clients and services.
- Connection reuse, timeout behavior, and header forwarding affect real request handling.

## Why It Matters In Real Systems

Production behavior depends on how requests traverse proxies, app servers, and infrastructure. Misunderstanding forwarded headers, timeouts, or connection handling can create broken redirects, security issues, or misleading logs.

## Practical Example

An ASP.NET Core service may sit behind a reverse proxy that terminates TLS and forwards requests internally. The application must process forwarded headers correctly to generate proper URLs and security-sensitive behavior.

## Common Pitfalls

- assuming the application sees the raw client connection directly
- ignoring proxy and load-balancer behavior
- treating timeout defaults as safe without review

## Interview Takeaways

- Explain request flow from client to proxy to app.
- Mention reverse proxies, TLS termination, and forwarded headers.
- Connect the topic to production debugging and deployment reality.

## Related Topics

- [debugging-and-troubleshooting.md](debugging-and-troubleshooting.md)
- [../interview/web-servers.md](../interview/web-servers.md)
- [../../backend/dotnet/framework/aspnet-core.md](../../backend/dotnet/framework/aspnet-core.md)
