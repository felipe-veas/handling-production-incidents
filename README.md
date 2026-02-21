# Production Incident Operations Handbook

## Purpose

This repository serves as the internal operational handbook for managing high-severity production incidents. It defines the baseline doctrine for incident response, coordination, communication, and escalation.

This is not a technical runbook for specific services, nor is it a configuration guide for our observability stack. It is a behavioral and operational framework designed to standardize how we act when production is degraded or failing. Operating mission-critical systems at scale is fundamentally a human coordination problem, bounded by technical constraints. When the pager fires, the technical symptoms are secondary to the operational discipline required to mitigate them safely.

This documentation is written for Senior Engineers, Site Reliability Engineers, Platform Engineers, and Engineering Managers who participate in the incident command structure.

## Structure

The doctrine is divided into the following modules, each addressing a specific operational domain during an incident lifecycle:

* **[Overview](overview.md)**: Establishes the operational reality of production incidents, detailing the cognitive load, the initial chaos of pager alerts, and the mechanics of system failure before mitigation begins.
* **[Severity Levels](severity-levels.md)**: Defines the reality of incident classification. It moves beyond theoretical definitions of severity to focus on business impact, common misclassification anti-patterns, and the organizational consequences of getting it wrong.
* **[Incident Coordination](incident-coordination.md)**: Dictates the structure of incident command. It addresses the risks of decentralized debugging, bystander effects, and the necessity of explicit role delegation during a crisis.
* **[Communication](communication.md)**: Outlines the protocols for maintaining signal-to-noise ratio. It defines how to communicate effectively with stakeholders, engineering teams, and executive leadership without derailing the mitigation effort.
* **[Escalation](escalation.md)**: Establishes the mechanics and thresholds for pulling additional context and authority into an incident. It covers the risks of premature escalation and the operational failure of escalating too late.

## Core Tenets

1. **Mitigation over Resolution**: The primary goal of incident response is to stop the bleeding. Root cause analysis (RCA) is a post-incident activity. Roll back, scale up, shed load, or fail over. Do not attempt to write a patch for a core library at 03:00 during a SEV1.
2. **Explicit Command**: Every incident has exactly one Incident Commander. If you do not know who the Incident Commander is, it might be you. If command is not explicitly handed off, it has not been handed off.
3. **Contain the Blast Radius**: Actions taken during an incident must strictly avoid compounding the problem. Do not introduce untested configurations or destructive state changes unless absolutely necessary to prevent catastrophic data loss.
4. **Signal over Noise**: Keep the main incident bridge and channels clear of speculative debugging. Communicate facts, confirmed hypotheses, and explicit actions.

Read this documentation before you go on call. During an incident, you will default to your highest level of preparation. Let this be that baseline.
