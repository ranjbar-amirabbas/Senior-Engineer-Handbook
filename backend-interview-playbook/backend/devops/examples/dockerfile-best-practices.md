# Dockerfile Best Practices

## Problem Context

Many container images work functionally but are larger, less secure, and slower to build than necessary.

## Recommended Approach

Use multi-stage builds, keep the runtime image minimal, copy only what the application needs, and avoid embedding secrets or local development artifacts.

## Example Walkthrough

For a .NET API, restore and publish in the SDK stage, then copy the published output into a smaller runtime image. Keep layer ordering stable so dependency restore can be cached efficiently.

## Trade-Offs

Smaller images improve transfer speed and security posture, but aggressive minimization can make on-container debugging harder if taken too far. The right balance depends on operational needs.

## Common Mistakes

- copying the whole repository into the final runtime image
- running as root unnecessarily
- ignoring `.dockerignore`

## Related Topics

- [../fundamentals/docker.md](../fundamentals/docker.md)
- [../interview/docker-kubernetes.md](../interview/docker-kubernetes.md)
