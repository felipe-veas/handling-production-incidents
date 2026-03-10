# Severity Levels: Operational Reality vs. Theory

## The Illusion of Theoretical Definitions

Standard ITIL or DevOps literature defines severity levels using clean matrices of "Impact" versus "Urgency." They rely on rigid thresholds like "Service unavailable for X% of users."

In production, these definitions fail.

If a payment gateway drops 15% of transactions on Black Friday, a strict matrix might classify it as "Severity 2: Degraded workflow." In reality, that's a multi-million dollar revenue loss demanding all-hands-on-deck—a SEV1. Conversely, a total outage of an internal analytics dashboard used by three people on a Sunday technically meets the criteria for "Service Unavailable," but treating it as a SEV1 destroys on-call trust.

Severity doesn't measure technical state. It measures business risk, reputational damage, and operational pain.

## What "Severity 1" Actually Means

A true SEV1 means the business is actively bleeding. It isn't defined by a Datadog metric crossing a line. It's defined by consequences.

A SEV1 implies:

* **Massive Revenue Impact**: Users can't complete core transactions (checkout, login, data ingestion).
* **Data Loss or Corruption**: Customer data is actively being destroyed, leaked, or corrupted.
* **Existential Security Breach**: An active intrusion with unauthorized access to production.
* **Executive Involvement**: Leadership is paged or demanding updates.

When you declare a SEV1, normal rules stop. Sprints pause. Feature work drops. The only priority is mitigation. Declaring a SEV1 for a non-critical issue burns responder goodwill.

## Determining Severity

Assess user impact, not technical symptoms. Ask these questions immediately:

1. **Who is impacted?** (All users, a specific tenant, internal staff?)
2. **What is the workflow?** (Is it the critical path to revenue, or an async background job?)
3. **Are there workarounds?** (Can the customer use a different path, or are they hard-blocked?)

Never base severity on the volume of alerts. One alert for "Checkout DB Connection Refused" is a SEV1. Fifty alerts for "CPU usage > 90% on batch cluster" might just be a SEV3 if the jobs safely retry.

**The Golden Rule of Triage**: If impact is unknown, assume the worst. Upgrade the severity to get the right people in the room, then aggressively downgrade once you confirm the actual impact is lower.

## Typical Misclassification Mistakes

**1. The "Everything is a SEV1" Anti-Pattern**
Teams with poor monitoring often treat every alert as a SEV1. This causes acute alert fatigue. If every page is an emergency, responders start treating real emergencies with the same cynical, slow response they give to false alarms. "Crying wolf" destroys on-call rotations.

**2. The Stealth Downgrade to Avoid Paperwork**
If an organization has heavy post-incident review (PIR) paperwork, engineers will sometimes deliberately classify a SEV1 as a SEV3 to avoid writing a report or facing executive review. This hides systemic risk and prevents the team from learning from massive near-misses. You have to decouple severity from blame.

**3. Anchoring on the Initial Symptom**
A responder might call an incident a SEV3 because the first alert was for a single failing background worker. Thirty minutes later, it turns out the worker is failing because a database lock is preventing all customer logins. The responder fails to upgrade to SEV1 because they mentally anchored to the initial "minor" alert. Severity is dynamic—re-evaluate it as new data arrives.

## The Cost of Misclassification

Getting severity wrong compounds failure.

Under-classifying an incident delays escalation. The right experts aren't paged, external communication stalls, and customer trust erodes as downtime stretches on.

Over-classifying burns out engineers. Waking up a principal engineer at 4:00 AM for a broken staging environment or a cron job failure destroys trust. Eventually, responders silence pagers or ignore alerts, creating a massive blind spot for the next real SEV1.

Determine severity accurately, focus on business impact, and adjust dynamically.
