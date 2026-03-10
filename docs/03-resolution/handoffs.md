# Handoffs: Managing Long-Running Incidents

## The Danger of Attrition

Most production incidents are resolved in under two hours. However, massive outages—like a total database corruption, a core network partition, or a multi-region cloud failure—can last for 6, 12, or even 24 hours.

When an incident spans multiple hours, the primary risk stops being technical complexity and becomes human fatigue. Sleep-deprived, stressed engineers lose situational awareness. They develop tunnel vision, anchor to incorrect hypotheses, and are more likely to execute destructive commands (e.g., "I'm just going to restart the entire cluster and see what happens").

## When to Execute a Handoff

Do not wait until the primary responder is completely exhausted. Handoffs must be proactive, not reactive.

**Mandatory Handoff Triggers:**

* **Duration:** The incident has been ongoing for 4 continuous hours.
* **Time of Day:** The incident started during the night shift (e.g., 2:00 AM) and normal business hours have begun, meaning fresh responders are available.
* **Cognitive Overload:** The Incident Commander (IC) or lead Operations engineer feels they have hit a wall, are out of new hypotheses, or are struggling to focus.

## The Handoff Protocol

A handoff is a formal transfer of state and authority. It cannot be done asynchronously via a brief Slack message. It requires a synchronous, structured conversation between the outgoing and incoming responders.

### 1. The Pre-Brief (Scribe/Comm Lead)

Before the incoming responder joins the bridge, the Scribe or Comm Lead should ensure the incident timeline and current status are clearly summarized in the main incident channel. The incoming IC should spend 5 minutes reading this timeline before joining the bridge.

### 2. The Sync (The SitRep)

The outgoing IC briefs the incoming IC using the SitRep (Situation Report) format:

1. **Status:** What is broken right now? (e.g., "Checkout DB is still read-only.")
2. **Impact:** What is the business impact? (e.g., "Zero transactions processing for the NA region.")
3. **Actions Taken:** What have we already tried? (e.g., "We tried failing over to the replica, but replication lag was too high.")
4. **Current Hypothesis:** What are we investigating right now? (e.g., "We believe a massive batch job is holding an exclusive lock on the payments table.")
5. **Next Steps/ETA:** What is the immediate plan? (e.g., "We are writing a script to identify and kill the locking queries.")

### 3. The Explicit Transfer of Command

As stated in the [Incident Coordination](../02-response/incident-coordination.md) doctrine, the transfer of power must be explicit.

* *Outgoing IC:* "I have briefed you on the SitRep. Do you accept the role of Incident Commander?"
* *Incoming IC:* "I accept. I am now the IC."

The Scribe must immediately announce this change in the main incident channel: `@here Alice is stepping down as IC. Bob is assuming command.`

## Forced Disconnect

Once an engineer hands off their role, **they must disconnect**.

If you hand off the IC or Operations role but stay on the Zoom bridge "just to listen," you are not resting. You are still carrying the cognitive load of the incident, and you are confusing the rest of the team about who is actually in charge.

The new IC has the authority to forcibly kick outgoing responders from the bridge. Go sleep.
