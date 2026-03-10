# Post-Incident Reviews (Postmortems): Blameless RCA

## The Goal of a Postmortem

An incident is an unplanned investment in reliability. The Post-Incident Review (PIR), or postmortem, is how we extract a return on that investment.

The goal of a postmortem is not to assign blame, but to uncover systemic vulnerabilities and generate actionable work that prevents recurrence.

## Blameless Culture

Human error is never the root cause; it is a symptom of a poorly designed system. If an engineer brought down the database by typing the wrong command, the postmortem must ask why the system allowed that command to execute without a safety check, peer review, or dry-run validation.

**When writing a PIR:**

* Use objective language.
* Assume everyone acted rationally given the information and tools they had at the time.
* Focus on the failure of tooling, processes, and automation.

## Postmortem Structure

Every SEV1 and SEV2 incident must result in a documented postmortem within 48 hours of resolution. Use the following standard format:

### 1. Incident Summary

A high-level executive summary of what happened.

* **Date & Time:** (UTC)
* **Duration:** Total time from the initial failure to full mitigation.
* **Status:** Resolved, Mitigated, or Ongoing.
* **Severity & Impact:** Quantify the business impact (e.g., "30% of users could not complete checkout for 45 minutes, resulting in an estimated $50k revenue loss").

### 2. Timeline

The chronological sequence of events, compiled by the Scribe. Include detection time, major state changes, when mitigations were applied, and when the system recovered.

* Example:
  * `[10:00]` Alert triggered: High latency on API Gateway.
  * `[10:05]` On-call engineer acknowledged.
  * `[10:25]` Mitigation applied: Scaled read replicas.
  * `[10:30]` Metrics normalized. Incident resolved.

### 3. The 5 Whys (Root Cause Analysis)

Ask "Why?" recursively until you hit a systemic, process, or tooling failure.

1. **Why did the API timeout?** Because the database was overwhelmed.
2. **Why was the database overwhelmed?** Because a new massive query was deployed.
3. **Why did the query cause issues?** It lacked a proper index.
4. **Why was it deployed without an index?** The ORM generated it dynamically and CI tests ran on a tiny dataset.
5. **Why didn't we catch this?** We do not run automated load tests against production-like data volumes in staging. *(Root Cause)*

### 4. Action Items (AIs)

A postmortem is useless without Action Items. Every AI must be a tracked ticket (e.g., in Jira) with an owner and a deadline. AIs must fall into three categories:

* **Prevent:** Fixes that stop the root cause from happening again (e.g., "Add ORM query analysis bot to CI/CD pipeline").
* **Detect:** Improvements to monitoring so the failure is caught faster next time (e.g., "Create Datadog monitor for slow queries > 2s").
* **Mitigate:** Tooling or runbooks that make recovery faster if it happens again (e.g., "Write runbook for fast database replica scaling").

## The Review Process

Once the postmortem draft is complete, schedule a review meeting with the stakeholders involved. Do not read the document aloud. Assume everyone has read it beforehand. Spend the meeting debating the Action Items: are they realistic? Do they actually address the root cause? Are they prioritized correctly?
