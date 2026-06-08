---
name: monitoring
description: "Application monitoring, logging, and observability for production applications. Use when: setting up error tracking, adding logging, creating health endpoints, configuring alerting, preparing for production, or when debugging production issues."
---

# Monitoring

## Overview

Monitoring turns invisible failures into visible signals. The goal is knowing when something breaks before users do.

## 1. Structured Logging

Use JSON format with consistent fields across all log entries.

**Required fields:** timestamp, level, message, request_id  
**Useful fields:** user_id (not PII), service_name, duration_ms, status_code

**Log levels:**
- `error` — Something failed and needs attention
- `warn` — Degraded state but still functioning (slow query, retry succeeded)
- `info` — Important business events (user signup, payment processed)
- `debug` — Development detail — never in production

**Never log:** Passwords, API keys, tokens, session secrets, PII (emails, addresses).

## 2. Error Tracking

Configure Sentry, LogRocket, or Bugsnag. Capture stack traces, group by root cause, alert on new error types. Tag errors with release version for regression detection.

## 3. Health Endpoints

**`/health`** — Liveness check. Returns 200 if the process is running. Used by load balancers.

**`/ready`** — Readiness check. Verifies dependencies: database, cache, external APIs. Returns 503 if any dependency is down.

## 4. Uptime Monitoring

External ping service (UptimeRobot, Pingdom, Better Stack) hits `/health` every 1–5 minutes from multiple regions. Alert on 2+ consecutive failures.

## 5. Alerting Strategy

**Page (immediate):** 5xx error rate spike, health check failure, database unreachable.

**Notify (review soon):** Slow response times (p95 > threshold), elevated error rate.

**Avoid alert fatigue:** Every alert should have a clear action. If you ignore an alert regularly, fix it or remove it.

## Key Metrics

| Metric | Target |
|---|---|
| Response time p95 | < 500ms |
| Error rate (5xx / total) | < 0.1% |
| Uptime | 99.9%+ |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll add monitoring when we need it" | You need it the moment a real user touches your app. |
| "Console.log is fine" | Console.log in production is noise. Structured logging is signal. |
| "Dashboards are enough" | Nobody watches dashboards at 3am. Alerts catch what dashboards miss. |

## Verification

- [ ] Structured logging in place (JSON, consistent fields, correct levels)
- [ ] No PII, secrets, or tokens in log output
- [ ] Error tracking configured and capturing stack traces
- [ ] /health endpoint returns 200 when process is alive
- [ ] /ready endpoint checks all critical dependencies
- [ ] Uptime monitoring pinging from external service
- [ ] Alerting configured for critical failures
- [ ] Key metrics tracked (response time, error rate, uptime)
