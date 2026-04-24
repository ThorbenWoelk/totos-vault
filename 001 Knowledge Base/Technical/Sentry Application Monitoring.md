---
created: 2025-07-22
last_edited: 2026-04-24
tags:
  - sentry
  - error-tracking
  - performance-monitoring
  - distributed-tracing
  - alerts
  - integrations
  - observability
  - release-monitoring
  - debugging
connections:
  - "[[001 Knowledge Base/Technical/_index|Technical Index]]"
  - "[[Bloom Filter Folding in Parquet]]"
  - "[[DevOps Principles]]"
ai_generated: true
human_approved: false
category:
  - Technical
---
Sentry is an open-source application monitoring and error tracking platform that helps developers detect, trace, and fix issues in real-time production environments.

## Key Capabilities
- **Error Tracking**: Captures unhandled exceptions and manually logged errors with full stack traces
- **Performance Monitoring**: Tracks application performance, transaction times, and bottlenecks
- **Real-time Alerts**: Instant notifications via Slack, email when issues occur
- **Context Collection**: Captures environment, device, OS, release version, user actions leading to errors
- **Distributed Tracing**: End-to-end visibility across microservices and distributed systems

## Core Architecture
- **DSN (Data Source Name)**: URL endpoint where error reports are sent
- **SDKs**: Available for JavaScript, Python, Ruby, Java, PHP, Go, Rust, C#, and more
- **Breadcrumbs**: Trail of events (user interactions, API calls) leading to errors
- **Error Grouping**: Intelligent classification of similar errors into common incidents

## Key Integrations
- **Issue Tracking**: Jira, GitHub Issues (auto-create tickets from errors)
- **Version Control**: GitHub, GitLab (link errors to commits, PRs)
- **CI/CD**: Track releases and correlate errors with deployments
- **Communication**: Slack notifications, custom webhooks

## Primary Use Cases
1. **Production Error Monitoring**: Real-time detection of application failures
2. **Performance Optimization**: Identify slow transactions and bottlenecks
3. **Release Monitoring**: Track error rates and crash-free sessions per version
4. **Debugging**: Rich context for reproducing and fixing production issues

## Setup Pattern
```javascript
// Basic initialization
Sentry.init({
  dsn: 'https://key@org.ingest.sentry.io/project',
  tracesSampleRate: 1.0, // Adjust in production
});

// Manual error capture with context
Sentry.captureException(error, {
  tags: { feature: 'checkout' },
  extra: { userId: 'user-123' }
});
```

## Key Benefits
- **Proactive Issue Detection**: Catch errors before users report them
- **Rich Debugging Context**: Full stack traces, user sessions, environment data
- **Team Collaboration**: Auto-assignment, issue tracking integration
- **Release Quality**: Monitor stability across deployments

