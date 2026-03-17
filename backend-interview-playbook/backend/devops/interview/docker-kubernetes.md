# Docker And Kubernetes Interview Questions

## Intro

This interview topic tests whether you understand container packaging and orchestration as operational tools. Strong answers focus on how these technologies affect reliability, deployment, and debugging in backend systems rather than only repeating object definitions.

## Use This File With

- [../fundamentals/docker.md](../fundamentals/docker.md)
- [../fundamentals/kubernetes.md](../fundamentals/kubernetes.md)
- [../examples/kubernetes-debugging.md](../examples/kubernetes-debugging.md)

## Containers

### Question

What is the difference between a Docker image and a container?

### Short Answer

An image is an immutable build artifact. A container is a running instance of that image with runtime state.

### Deep Explanation

The distinction matters operationally. Images are built, scanned, versioned, and promoted through environments. Containers are started, monitored, restarted, and terminated as part of runtime behavior.

### Senior-Level Notes

Senior candidates mention immutable delivery and reproducibility.

### Common Mistakes

- using the terms interchangeably

### Question

Why are multi-stage Docker builds useful?

### Short Answer

They let you build with a full toolchain in one stage and ship a smaller, cleaner runtime image in another.

### Deep Explanation

This reduces image size, attack surface, and unnecessary dependencies in production while keeping builds straightforward.

### Senior-Level Notes

The more realistic answer usually mentions what gets left behind in the runtime image: SDKs, build tools, test assets, and anything that does not belong in production. That sounds like someone who has actually maintained images.

### Common Mistakes

- shipping SDKs and build tools in runtime images

### Question

What should you avoid in Docker images for backend services?

### Short Answer

Avoid unnecessary files, embedded secrets, oversized base images, and assumptions that the container will manage process correctness for you.

### Deep Explanation

Large or messy images slow builds and deployments. Secrets in images are a security failure. Poor signal handling or startup design remains a problem even inside a container.

### Senior-Level Notes

Senior candidates mention least privilege and operational hygiene.

### Common Mistakes

- treating containerization as a substitute for good application behavior

## Kubernetes

### Question

What is the purpose of a Kubernetes Deployment?

### Short Answer

A Deployment manages rollout, replacement, and desired state for a set of pods running a stateless application workload.

### Deep Explanation

It ensures the requested number of replicas exists and supports rolling updates. It is a control mechanism for application instances rather than a generic wrapper for every kind of workload.

### Senior-Level Notes

Strong answers mention how rollout strategy and pod readiness affect live traffic.

### Common Mistakes

- confusing Deployments with Services or Pods

### Question

What is the difference between liveness and readiness probes?

### Short Answer

Readiness determines whether a pod should receive traffic. Liveness determines whether the container should be restarted because it is considered unhealthy.

### Deep Explanation

A pod can be alive but not ready, such as during warmup or dependency initialization. Misusing liveness can create restart loops. Misusing readiness can send traffic too early.

### Senior-Level Notes

The practical answer is to connect probes to rollout behavior: readiness protects traffic, liveness protects recovery, and startup probes stop slow boots from being misdiagnosed as dead containers.

### Common Mistakes

- using the same check blindly for every probe type

### Question

Why do resource requests and limits matter?

### Short Answer

Requests influence scheduling and guarantees, while limits cap consumption. Together they affect cluster efficiency, stability, and the application's runtime behavior.

### Deep Explanation

If requests are too low, the scheduler may pack too much onto a node. If limits are too aggressive, the application may be throttled or terminated. Resource settings are therefore part of performance tuning.

### Senior-Level Notes

Senior candidates usually mention that bad resource settings create two different failure modes: overpacking the cluster or throttling and killing the application. That operational framing is stronger than just defining requests and limits.

### Common Mistakes

- setting requests and limits arbitrarily

### Question

How do you debug a pod that keeps restarting?

### Short Answer

Check container logs, describe the pod, inspect probe failures, exit reasons, recent configuration changes, and dependency health before assuming the platform is at fault.

### Deep Explanation

Restart loops can come from app crashes, bad configuration, probe misconfiguration, OOM kills, or downstream dependency issues. The debugging process should narrow the failure mode systematically.

### Senior-Level Notes

The strongest answer is usually stepwise: check exit reason, probe failures, recent config changes, then application logs and dependency health. That sounds like a real debugging sequence instead of a grab bag of tools.

### Common Mistakes

- blaming Kubernetes before checking app behavior and probes

### Question

When is Kubernetes too much complexity?

### Short Answer

Kubernetes can be too much when the system is small, operational maturity is low, or the platform complexity outweighs the orchestration benefits.

### Deep Explanation

It solves real problems at scale, but it also adds concepts, tooling, and operational burden. The right choice depends on the team's needs and ability to run it well.

### Senior-Level Notes

Strong answers show tool judgment rather than assuming all production systems need Kubernetes.

### Common Mistakes

- choosing Kubernetes by default without operational justification

## Advanced Senior-Level Questions

### Question

How do you make container startup and rollout behavior safe for production traffic?

### Short Answer

I focus on fast predictable startup, correct readiness signaling, and rollout settings that limit blast radius. A container that starts eventually is not good enough if the platform sends traffic too early or replaces too many instances at once.

### Deep Explanation

Production safety depends on the interaction between image size, application warmup, probes, connection establishment, and deployment strategy. If readiness is optimistic or startup is too slow, rolling deploys can create avoidable errors even when the application code is correct.

### Senior-Level Notes

The better answer usually mentions that rollout safety is shared between the app and the platform. The app must signal health honestly, and the platform must respect that signal conservatively.

### Common Mistakes

- treating successful process start as production readiness
- ignoring warmup behavior during rolling deploys
- choosing aggressive rollout settings without failure data

### Question

When are stateful workloads on Kubernetes a good idea, and when are they not?

### Short Answer

They are a good idea when the team understands the operational model and the platform adds real value for that workload. They are a poor fit when the team expects Kubernetes alone to remove the inherent complexity of running stateful systems.

### Deep Explanation

Kubernetes can help with orchestration, but databases, brokers, and other stateful systems still have replication, backup, failover, and performance constraints of their own. The question is not whether Kubernetes can run them. It is whether the team can run them well there.

### Senior-Level Notes

The strongest answer avoids absolutism. Some organizations run stateful workloads well on Kubernetes. Others are better served by managed platforms or dedicated operational models.

### Common Mistakes

- assuming Kubernetes automatically simplifies stateful operations
- rejecting all stateful workloads on principle
- ignoring backup, restore, and failure-domain planning

### Question

How do you debug performance problems in containers that do not appear outside the orchestrated environment?

### Short Answer

I compare runtime assumptions first: CPU throttling, memory pressure, startup environment, network policy, resource requests and limits, and dependency topology. The issue is often the interaction between the app and the platform, not one layer alone.

### Deep Explanation

An application may look healthy in local or bare-host testing and behave differently under orchestration because of scheduling, cgroup limits, service discovery latency, or probe and sidecar behavior. Debugging requires telemetry from both the application and the cluster runtime.

### Senior-Level Notes

The stronger answer sounds systematic: confirm the symptom in-cluster, inspect resource and probe behavior, compare dependency timings, then narrow whether the issue is throttling, memory, networking, or app logic.

### Common Mistakes

- assuming local performance results carry over unchanged to the cluster
- looking only at application logs
- tuning resource settings blindly without observing actual behavior

## Related Topics

- [../fundamentals/docker.md](../fundamentals/docker.md)
- [../fundamentals/kubernetes.md](../fundamentals/kubernetes.md)
- [../fundamentals/monitoring-and-observability.md](../fundamentals/monitoring-and-observability.md)
- [deployment-and-operations.md](deployment-and-operations.md)
