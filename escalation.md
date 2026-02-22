# Escalation: Judgment, Paths, and Risks

## Judging When to Escalate vs. When to Defer

Escalation pulls additional authority, context, or technical expertise into an active incident.

Ideally, the on-call engineer gets paged, diagnoses the issue, and fixes it. But during a severe outage, trying to be a hero is an operational liability. You escalate based on time limits and impact, not ego.

**When to Escalate Immediately:**

* You've investigated for 15 minutes and have no viable hypothesis.
* The required mitigation carries severe risk (e.g., dropping a table, failing over a primary datacenter) and requires higher-level authorization.
* The incident spans multiple domains (e.g., a network partition causing database deadlocks), and you lack expertise in one of them.
* Business impact is growing rapidly beyond the initial severity classification.

**When to Defer (Hold Escalation):**

* You have a confirmed hypothesis, a safe mitigation plan, and just need time for execution (e.g., waiting for a 10-minute rollback).
* The incident is contained, impact is stable, and you're actively applying a low-risk fix.

## Escalation Paths Most Teams Get Wrong

A common anti-pattern is escalating hierarchically instead of by domain.

When an engineer gets stuck on a bizarre Kafka partitioning issue, they often escalate to their Engineering Manager. Unless that manager is a Kafka internals expert, the escalation is useless. The manager joins the bridge, adds pressure, and asks for status updates without providing any technical leverage.

**Correct Escalation Paths:**

1. **Domain Expertise**: Escalate horizontally to Subject Matter Experts (SMEs). If the database is failing, page the DBAs. If the network drops packets, page core infrastructure.
2. **Incident Command**: If the incident grows too chaotic, escalate the *role* of Commander to a more senior incident manager or Staff Engineer. This frees the original IC to return to an operational role.
3. **Executive Authority**: Escalate hierarchically *only* when you need a business decision. (e.g., "We can restore service now by dropping the last hour of analytics data, or recover the data with 4 hours of downtime. VP of Engineering, we need a call.")

## Balancing Urgency with Stability

When you pull a Staff Engineer or external SME into an incident, they enter cold. They missed the last 30 minutes of debugging context.

The biggest risk here is letting the newly paged expert take uncoordinated action. Driven by SEV1 urgency, a senior engineer will often log straight into a production box and run commands based on gut instinct. This changes system state while the rest of the team is executing a different plan.

**Rule:** Escalated responders never bypass the Incident Commander.
When an SME joins the bridge, the IC pauses, gives a 60-second sitrep (Status, Impact, Actions taken, Current hypothesis), and assigns them a task. Urgency doesn't excuse cowboy engineering. Response stability comes first.

## The Operational Risks of Premature Escalation

Escalating too late prolongs outages. But escalating too early carries its own risks:

1. **Context Switching**: Paging three different teams in the first five minutes means 10 people jump into a channel demanding context before you even understand the alert. This spikes the primary responder's cognitive load, forcing them to manage people instead of investigating the system.
2. **Alert Fatigue**: If you hit "page all" every time database CPU hits 80%, SMEs will learn to ignore you. Respect the focus and sleep of other teams.
3. **Dilution of Ownership**: If a team always escalates to Platform or Core Infra at the first sign of trouble, they never build the operational muscle to debug their own services.

## How Escalation Interacts with Resolution

Escalation changes incident geometry. As more teams join, it shifts from localized debugging to a distributed effort.

The IC must ensure the expanded team stays focused on *mitigation*, not root-cause analysis. Escalated SMEs often want to find the exact line of code causing a memory leak. The IC needs to redirect: "I don't care why memory is leaking right now. What's the safest way to shed load or add capacity so we can recover?"

Escalate for leverage, keep strict command over new responders, mitigate the impact, and save the deep dive for the post-mortem.
