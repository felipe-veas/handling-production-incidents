# Production Incident Operations Handbook

## Purpose

This repository is our internal handbook for managing high-severity production incidents. It defines our baseline doctrine for incident response, coordination, communication, and escalation.

This isn't a technical runbook for specific services or a configuration guide for our observability stack. It's a behavioral framework that standardizes how we operate when production fails. Operating mission-critical systems at scale is a human coordination problem bounded by technical constraints. When the pager fires, technical symptoms are secondary to the operational discipline required to mitigate them safely.

We wrote this for Engineers (SRE, Platform, Backend) and Engineering Managers participating in incident command.

## Structure

The doctrine covers specific operational domains during an incident lifecycle:

* **[Overview](overview.md)**: The reality of production incidents. Covers cognitive load, initial alert chaos, and how systems fail before mitigation starts.
* **[Severity Levels](severity-levels.md)**: Incident classification in practice. Focuses on business impact, misclassification anti-patterns, and the cost of getting it wrong.
* **[Incident Coordination](incident-coordination.md)**: The structure of incident command. Addresses the risks of decentralized debugging, the bystander effect, and role delegation.
* **[Communication](communication.md)**: Protocols for maintaining a high signal-to-noise ratio. How to update stakeholders and executives without derailing the engineering effort.
* **[Escalation](escalation.md)**: Mechanics for pulling additional context and authority into an incident. Covers the risks of escalating too early or too late.

## Core Tenets

1. **Mitigation over Resolution**: The primary goal is to stop the bleeding. Root cause analysis (RCA) happens after the incident. Roll back, scale up, shed load, or fail over. Do not try to write a patch for a core library at 03:00 during a SEV1.
2. **Explicit Command**: Every incident has exactly one Incident Commander (IC). If you don't know who the IC is, it might be you. If command isn't explicitly handed off, it hasn't transferred.
3. **Contain the Blast Radius**: Do not compound the problem. Avoid untested configurations or destructive state changes unless they are the only way to prevent catastrophic data loss.
4. **Signal over Noise**: Keep the main incident channel clear of speculative debugging. Stick to facts, confirmed hypotheses, and explicit actions.

Read this before you go on call. Under pressure, you don't rise to the occasion; you default to your level of training. Let this be your baseline.
