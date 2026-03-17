# Web Servers Interview Questions

## Intro

These questions test basic production web-serving concepts that affect backend behavior.

## Request Flow

### Question

What is the role of a reverse proxy in front of an application server?

### Short Answer

A reverse proxy can terminate TLS, route traffic, serve static assets, apply limits, and forward requests to backend applications.

### Deep Explanation

It becomes an important operational boundary for traffic handling and security. Backend applications often depend on forwarded headers and proxy-aware configuration to behave correctly.

### Senior-Level Notes

Strong answers mention load balancing and request metadata handling.

### Common Mistakes

- assuming the app server always talks directly to the client

### Question

Why do forwarded headers matter?

### Short Answer

They tell the application about the original client-facing protocol, host, and IP when proxies sit in front of it.

### Deep Explanation

Without them, applications may generate wrong URLs, log misleading addresses, or make incorrect security decisions.

### Senior-Level Notes

Senior answers connect this to deployment and debugging.

### Common Mistakes

- ignoring proxy topology in production environments
